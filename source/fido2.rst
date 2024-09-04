2-factor auth with FIDO2 keys
==============================
OpenSSH version 8.3 and newer are able to use FIDO2 security keys to
isolate private key material and to require "proof of presence" before
performing cryptographic operations.

If you have a FIDO2 key, you can request that we switch to it for your
ssh access, which will add strong multi-factor protection to your
credentials.

Recommended FIDO2 keys
----------------------
There are many keys to choose from -- as long as you obtain them from a
reputable vendor, they all should be well-suited for this task. If it is
important to you that the key is open-hardware and free-software
friendly, we can recommend the following options:

- `SoloKeys`_
- `Nitrokey 3`_

Proprietary options are also available:

- `Yubikey 5`_
- `Titan Security Key`_

If you do not have any USB-A ports on your system, then you probably
want to get a USB-C key. You can also use the same device to secure your
access to many other accounts online, so you may want to consider
getting a NFC-capable version so you can use it for authenticating with
services on your smartphone.

.. note::

   It is not possible to have two identical FIDO2 devices with the same
   ssh key, so you should consider getting two devices just so you have a
   backup option, and sending in both your primary and backup ssh keys.

.. _`SoloKeys`: https://solokeys.com/
.. _`Nitrokey 3`: https://shop.nitrokey.com/shop?&search=nitrokey%203
.. _`Yubikey 5`: https://www.yubico.com/products/yubikey-5-overview/
.. _`Titan Security Key`: https://store.google.com/product/titan_security_key

Initial PIN setup
-----------------
Before you do anything else, you should set up a PIN on your device. We
do not recommend using a device without a PIN, because this removes an
important authentication factor ("something you know") and allows anyone
in possession of your device to authenticate as you.

You can use the manufacturer's tools (e.g. Yubikey-Manager) to set up a
PIN for your device, or you can use any Chromium based browser for
the same purpose:

- https://blog.4loeser.net/2020/05/chromium-manage-fido2-security-keys.html

Generating a ssh key
--------------------
It is not possible to load a pre-existing ssh key onto a FIDO2 token --
you have to generate one directly on the device. For this reason we
recommend getting two devices and repeating the procedure for both of
them, if you are worried that you'd be locked out if you lose access to
your primary one.

To generate a ssh key on your device::

    ssh-keygen -t ed25519-sk -O resident -O verify-required -C "Some smart comment"

If you set up a PIN on your device, you can leave the passphrase blank.

.. note::

   It's possible that your device does not support ed25519 cryptography.
   In that case, use ``-t ecdsa-sk``.

If you have a backup device, repeat the process and save the keys into a
different set of files.

Verifying that it works
-----------------------
Before you send in your new key, you should make sure that you are able
to use it for ssh connections. You can add the public key to your local
account and then try to ssh to localhost (assuming you have sshd
enabled on your workstation)::

    cat .ssh/id_ed25519_sk.pub >> .ssh/authorized_keys
    chmod 0600 .ssh/authorized_keys
    ssh -i .ssh/id_ed25519_sk localhost

You should be prompted to enter your PIN, and then touch the device to
confirm physical presence.

If everything is working as expected, you are ready to send in your
FIDO2 ssh key to the helpdesk.

Submitting your FIDO2 ssh key
-----------------------------
We will continue to use PGP to verify kernel developers' digital
identity, so you will need to send in your key in a message signed by
the PGP key that we have on file for you.

This is the easiest mechanism to do so::

    cat .ssh/id_ed25519_sk.pub | gpg --clearsign > signed_sk_key.txt

Send a message to helpdesk@kernel.org requesting that we switch your
access to a FIDO2 ssh key and attach ``signed_sk_key.txt``.

.. note::

   Make sure it's ``id_ed25519_sk.pub``, not ``id_ed25519_sk``. While
   you won't really be leaking your private key (it's just a key handle
   pointing at the device with the actual key), we can't do anything
   useful with its contents.

If you've made a backup key, send them both as two different
attachments.

Setting up your FIDO2 key on another computer
---------------------------------------------
If you've switched computers, you will need to set up your FIDO2 key
with openssh on the new system. It is sufficient to insert your FIDO2
device and run::

    ssh-keygen -K

This will require entering your PIN and touching the device, and will
write out the private key handle and the public key that you can then
configure with ssh.

Configuring ssh
---------------
See :doc:`access` for details on how to configure your ssh access.
