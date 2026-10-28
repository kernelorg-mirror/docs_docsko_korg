Kernel developer PGP keyring
============================

If you regularly contribute code to the Linux kernel, you are encouraged
to submit your key to be included in the PGP keyring repository. For us
to be able to accept it, it must have at least one signature from
someone whose key is already in that repository, so we can trace each
key's trust lineage to the head maintainer (Linus Torvalds). Use the
:doc:`ksmap` to find developers who can sign your key.

Getting the pgpkeys.git repository
----------------------------------

You can clone the repository from the following location:

 - https://git.kernel.org/pub/scm/docs/kernel/pgpkeys.git

There are currently the following directories in this repository:

 - keys/:    ascii-armoured keys
 - graphs/:  svg graphs showing trust paths to Linus Torvalds' key
 - scripts/: auxiliary helper scripts

Importing keys
--------------

Every file in the keys/ directory contains all UIDs belonging to each
key, so you can just grep for the person you need::

    $ grep -il torvalds *.asc
    79BE3E4300411886.asc

You can then ``gpg --import 79BE3E4300411886.asc`` into your keyring.

Alternatively, you can import all keys at once by running ``gpg --import
keys/*.asc``.

Automatically refreshing keys
-----------------------------

First, you should assign full trust to Linus's key (after importing it
into your keyring)::

    $ gpg --import keys/79BE3E4300411886.asc
    $ gpg --edit-key ABAF11C65A2970B130ABE3C479BE3E4300411886
    gpg> trust
    gpg> 4
    gpg> q
    $ gpg --check-trustdb

Now, copy the ``scripts/korg-refresh-keys`` script to your ``~/bin`` and
edit it according to the instructions.

That script will first verify that the latest commit to the repository
is signed by a valid key (a key directly signed by you or Linus), and
will only process any changes if the commit signature validates.

By default, ``korg-refresh-keys`` will run a "merge-only" import --
meaning that it will ignore any *new* keys added to the git repository
and will only refresh keys that you already have imported into your
keyring. If you would like to automatically import all new keys as they
are added, remove ``--import-options merge-only`` from the
``IMPORTFLAGS`` variable.

Make sure to run ``chmod a+x ~/bin/korg-refresh-keys`` after you are
done editing the file.

The last step is to set up a nightly cronjob by adding this to your
``crontab -e``::

    @daily ~/bin/korg-refresh-keys -q

Alternatively, if you are running a systemd-enabled system, set up a
timer instead::

    $ cat ~/.config/systemd/user/korg-refresh-keys.timer
    [Timer]
    OnCalendar=daily
    Persistent=yes

    [Install]
    WantedBy=sockets.target

    $ cat ~/.config/systemd/user/korg-refresh-keys.service
    [Service]
    ExecStart=%h/bin/korg-refresh-keys -q
    Type=oneshot

    $ systemctl enable --user korg-refresh-keys.timer
    $ systemctl start  --user korg-refresh-keys.timer
    $ systemctl start  --user korg-refresh-keys.service

Submitting keys to the keyring
------------------------------

If your key is *not already* in the kernel.org keyring, do the
following::

    gpg -a --export your@email.addr > export.asc

Send a message to keys@linux.kernel.org with that file as attachment.

You should also upload that file to https://keys.openpgp.org/upload/ to
have it listed on the openpgp.org keyserver.

Note, that anything you send to keys@linux.kernel.org will be archived
on https://lore.kernel.org/keys for record-keeping purposes.

Updating keys in the keyring
----------------------------

If you've made any changes to your key, please export it again::

    gpg -a --export your@email.addr > export.asc

Then upload export.asc to https://keys.openpgp.org/upload/. We perform
periodic updates to pgpkeys.git with the latest key data from keys.openpgp.org.

If the update is urgent, such as when you need to revoke a key or update
the expiration date on a key that's about to expire, follow the
"Submitting keys to the keyring" procedure described above.

.. _extend-expiration:

Extending your key's expiration date
------------------------------------

Most OpenPGP keys have an expiration date. After that date, tools like
git, b4 and sq no longer accept signatures made with the key. If you
have a kernel.org account, we send a reminder to your kernel.org address
30, 10 and 3 days before your key in the kernel.org keyring expires.

If you have a kernel.org account, keeping your key valid is also what
keeps your ssh access working, see :ref:`keeping-your-account-active`.

Extending the date is quick, and you can do it as many times as you
like. You don't need a new key, and nobody needs to sign your key again.

Find your key fingerprint
~~~~~~~~~~~~~~~~~~~~~~~~~

The reminder email shows your key fingerprint. You can also find it with
``gpg``:

.. code-block:: console

    $ gpg --list-secret-keys your@email.addr
    sec#  ed25519 2022-12-20 [C] [expires: 2026-12-19]
          000000000000000000000000AAAABBBBCCCCDDDD
    uid           [ultimate] Alice Dev <adev@kernel.org>
    ssb>  cv25519 2022-12-20 [E] [expires: 2026-12-19]
    ssb>  ed25519 2022-12-20 [S] [expires: 2026-12-19]

The long line under ``sec`` is the fingerprint. In the commands below,
replace ``[fpr]`` with it.

Make your Certify key available
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

You need your Certify key to change the expiration date. If ``gpg``
shows ``sec#`` instead of ``sec``, the Certify key isn't on this
computer. If you followed the `Kernel Maintainer PGP guide`_, you keep
it on your offline storage. Mount it and tell GnuPG to use it:

.. code-block:: console

    $ export GNUPGHOME=/media/disk/foo/gnupg-backup
    $ gpg --list-secret-keys

Make sure that you now see ``sec`` and not ``sec#``.

Extend the expiration date
~~~~~~~~~~~~~~~~~~~~~~~~~~

Extend the main key, then its subkeys:

.. code-block:: console

    $ gpg --quick-set-expire [fpr] 5y
    $ gpg --quick-set-expire [fpr] 5y '*'

You need both commands. Your signing subkey can expire before your main
key does, and then you can't sign with your key, even though the main
key is still valid.

Instead of ``5y`` (five years from today), you can also use a specific
date that is easy to remember, such as ``2031-07-01``.

The ``'*'`` only changes subkeys that haven't expired yet, so it leaves
old subkeys alone. If your signing subkey has already expired, name it
by its fingerprint instead of ``'*'``. You can see the fingerprints of
your subkeys with ``gpg --list-keys --with-subkey-fingerprints [fpr]``:

.. code-block:: console

    $ gpg --quick-set-expire [fpr] 5y [subkey-fpr]

These commands also repair keys with SHA-1 self-signatures, which sq
rejects. GnuPG makes the new self-signatures with a modern hash, and
they replace the old ones. For this to work, use a recent version of
GnuPG, and make sure that your ``gpg.conf`` does not set
``cert-digest-algo SHA1``.

If you used your offline storage, copy the changes back to your
everyday GnuPG directory, then unmount the storage:

.. code-block:: console

    $ gpg --export | gpg --homedir ~/.gnupg --import
    $ unset GNUPGHOME

Send us your updated key
~~~~~~~~~~~~~~~~~~~~~~~~

Export your public key:

.. code-block:: console

    $ gpg -a --export [fpr] > export.asc

Upload ``export.asc`` to https://keys.openpgp.org/upload/, so that others
can get the new date too. Then send it to us in one of these ways:

- Reply to the reminder email with ``export.asc`` attached.
- Send a new message to keys@linux.kernel.org with ``export.asc``
  attached.

Both go to keys@linux.kernel.org, a public mailing list archived at
https://lore.kernel.org/keys/. Only send your public key and nothing
that you want to keep private.

Please send your key at least a few days before it expires. We add
updated keys to the keyring by hand, so this can take a few days.

.. _`Kernel Maintainer PGP guide`: https://docs.kernel.org/process/maintainer-pgp-guide.html
