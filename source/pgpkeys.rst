Kernel developer PGP keyring
============================

If you regularly contribute code to the Linux kernel, you are encouraged
to submit your key to be included in the PGP keyring repository. For us
to be able to accept it, it must have at least one signature from
someone whose key is already in that repository, so we can trace each
key's trust lineage to the head maintainer (Linus Torvalds).

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
weekly updates to pgpkeys.git with the latest key data from keys.openpgp.org.
