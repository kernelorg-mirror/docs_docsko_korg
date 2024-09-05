How to set up your ssh access
=============================
Setting up your ssh access will depend on whether you're using your PGP
Auth subkey for ssh purposes, a FIDO2 key, or if you were issued a
private key from kernel.org.

If you received a ssh private key from kernel.org
-------------------------------------------------
Follow this procedure if you received an encrypted tarball containing the SSH
private key to use for accessing your kernel.org account. Place that private
key into your ``~/.ssh`` directory, e.g.::

    cp korg-username ~/.ssh/id_korg

You can change the automatically generated key passphrase using ``ssh-keygen
-p``.

.. important:: You should always keep your ssh key protected by a passphrase.

Add the following entries into your .ssh/config::

    Host gitolite.kernel.org
      User git
      IdentityFile ~/.ssh/id_korg
      IdentitiesOnly yes
      ClearAllForwardings yes
      # Below are very useful for speeding up repeat access
      ControlPath ~/.ssh/cm-%r@%h:%p
      ControlMaster auto
      ControlPersist 1H
      # Helps behind some NAT-ing routers
      ServerAliveInterval 60

If we used your PGP Authentication subkey
-----------------------------------------
If we found an Authentication (**[A]**) subkey on your PGP key, then we have
set up your access to use that key, instead of creating new ssh private keys.
This is what you need to do to configure your ssh client to use that subkey:

Add this to your ``.bashrc``::

    export SSH_AUTH_SOCK=$(gpgconf --list-dirs agent-ssh-socket)

You will need to kill the existing gpg-agent process and start a new login
session for the changes to take effect::

    $ killall gpg-agent
    $ bash
    $ ssh-add -L

The very first entry in the output should be the ssh public key derived from
your PGP Auth subkey -- it should have "``cardno:XXXXXXXX``" at the end in the
comment section.

Now add this to your .ssh/config::

    Host gitolite.kernel.org
      User git
      ClearAllForwardings yes
      HostKeyAlgorithms ssh-ed25519,ecdsa-sha2-nistp256,ssh-rsa
      # Below are very useful for speeding up repeat access
      ControlPath ~/.ssh/cm-%r@%h:%p
      ControlMaster auto
      ControlPersist 1H
      # Helps behind some NAT-ing routers
      ServerAliveInterval 60

To verify if everything is working, run ``ssh git@gitolite.kernel.org
help``.

If you sent in your FIDO2 ssh key
---------------------------------
You should just need the following in your .ssh/config::

    Host gitolite.kernel.org
      User git
      ClearAllForwardings yes
      HostKeyAlgorithms ssh-ed25519,ecdsa-sha2-nistp256,ssh-rsa
      ControlPath ~/.ssh/cm-%r@%h:%p
      ControlMaster auto
      ControlPersist 1H
      # Helps behind some NAT-ing routers
      ServerAliveInterval 60

To verify if everything is working, run ``ssh git@gitolite.kernel.org
help``.

.. note::

   If your FIDO2 device is protected by a PIN, you may get an error
   saying that "agent refused operation." This can be fixed by adding
   ``IdentityAgent none`` to the above section.

SSH host fingerprints
---------------------
Your kernel.org account grants you access to gitolite.kernel.org, which
you will use both  for accessing your git trees (see
:doc:`gitolite/index`) and for uploading tarball releases (see
:doc:`kup`).

======= =======================================================
Key     MD5 Fingerprint
======= =======================================================
RSA     ``MD5:b1:33:44:9d:3f:77:59:14:f8:05:d7:33:5d:b1:40:7b``
ECDSA   ``MD5:7c:a6:a2:e0:96:5f:e2:9a:9b:53:b6:41:29:66:f8:47``
ED25519 ``MD5:30:f1:e6:8f:ff:76:45:e7:5b:45:b0:bd:bd:ca:14:9c``
======= =======================================================

======= =======================================================
Key     SHA256 Fingerprint
======= =======================================================
RSA     ``SHA256:S1b2ARCfjjhsPJeqbCwkG+2ukBPCApogEfRTkVqEj4g``
ECDSA   ``SHA256:n5cYLTSXgZ97jR9DfOcFxHeHAt3BBqU89TpTQspqFxo``
ED25519 ``SHA256:KTfZsrwphTMpYOYr0Acfdk25gtg6zui3Oh8QOawAm5M``
======= =======================================================

Here they are PGP-signed::

    -----BEGIN PGP SIGNED MESSAGE-----
    Hash: SHA512

    # ssh-keygen -E sha256 -lf <(ssh-keyscan gitolite.kernel.org)
    2048 SHA256:S1b2ARCfjjhsPJeqbCwkG+2ukBPCApogEfRTkVqEj4g gitolite.kernel.org (RSA)
    256  SHA256:n5cYLTSXgZ97jR9DfOcFxHeHAt3BBqU89TpTQspqFxo gitolite.kernel.org (ECDSA)
    256  SHA256:KTfZsrwphTMpYOYr0Acfdk25gtg6zui3Oh8QOawAm5M gitolite.kernel.org (ED25519)

    # ssh-keygen -E md5 -lf <(ssh-keyscan gitolite.kernel.org)
    2048 MD5:b1:33:44:9d:3f:77:59:14:f8:05:d7:33:5d:b1:40:7b gitolite.kernel.org (RSA)
    256  MD5:7c:a6:a2:e0:96:5f:e2:9a:9b:53:b6:41:29:66:f8:47 gitolite.kernel.org (ECDSA)
    256  MD5:30:f1:e6:8f:ff:76:45:e7:5b:45:b0:bd:bd:ca:14:9c gitolite.kernel.org (ED25519)
    -----BEGIN PGP SIGNATURE-----

    iHUEARYKAB0WIQR2vl2yUnHhSB5njDW2xBzjVmSZbAUCZtipmgAKCRC2xBzjVmSZ
    bMoOAQCrHcd3B7ddx5qoF2eKCV3zDRYKPApTyuaRFOg1rm5yBAEA57ZebTHwiN9G
    rd4YTmJ6RfVLWEhuwSLyCzUXhT0/Ugo=
    =d0xn
    -----END PGP SIGNATURE-----

