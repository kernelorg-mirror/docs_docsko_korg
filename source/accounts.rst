Getting a kernel.org account
============================
Kernel.org accounts are reserved for Linux kernel maintainers or
high-profile developers. If you do not fall under one of these two
categories, it's unlikely that an account will be issued to you. If you
would like to apply for an account, please e-mail the following
information to helpdesk@kernel.org using the following template::

    Subject: Account request for [your name here]

    preferred username: [username]
    forwarding address: [your@email.address]

    Reasons for requiring a kernel.org account:
    [specify your reasons here]

    [Attachment: export.asc]

The Kernel.org admin team will then review your request and you should
receive a response back within a few days. If you are listed in
`MAINTAINERS`_ and have enough signatures on your PGP key to be in the
web of trust, your account will be issued without delay.

.. important::

   If we find an A (authentication) subkey on your PGP key, we will
   assume you will want to use that for your ssh access. If that is not
   the case, please mention it in the request and you'll be issued a new
   ssh private key instead.

.. _`MAINTAINERS`: https://git.kernel.org/pub/scm/linux/kernel/git/torvalds/linux.git/tree/MAINTAINERS

Exporting your public key
-------------------------
We no longer rely on keyservers for signature information, so please
attach a copy of your public key to the request. You can generate it
using the following command::

    gpg2 -a --export --export-options export-clean [YOURKEYID] > export.asc

PGP Web of Trust
----------------
.. warning::

   With extremely rare exceptions, accounts will not be issued unless
   the there are enough signatures on the PGP key to satisfy the web of
   trust.

We use the PGP web of trust to help ensure that only trusted kernel
developers are able to get an account on kernel.org. Before you send the
email, make sure that your PGP key is signed by *at least two other
people who already have an active kernel.org account*.

PGP signing events at conferences are usually a good place to start, or
you can find kernel developers who live in your area. If meeting in
physical space is not an option for you, read below for the recommended
video conferencing procedure.

.. important::

   Remember, the goal is not to verify someone's government-issued
   credentials, but to build a web of trusted contributors. When you are
   signing someone's key, you are effectively stating: "I have worked
   with this person and I vouch for their identity by signing their key
   with my own."

Keysigning via video conferencing
---------------------------------
If you are unable to attend a physical keysigning, it is acceptable to
have your key signed via video conferencing. You will need at least two
people who are:

- members of the kernel.org keyring (have active kernel.org accounts)
- EITHER have met you personally previously (e.g. via a conference)
- OR have worked with you for some period of time

Procedure for the signee
~~~~~~~~~~~~~~~~~~~~~~~~

1. Arrange a video conference using a platform acceptable for everyone
   involved.
2. Export and send your public key to all members who will be attending
   the call ahead of its scheduled time (using ``gpg --export -a -o
   unsigned.asc [your@address]``).
3. During the conference, establish your identity with the signers.
4. When everyone is ready, read your public key fingerprint out loud
   (you can display it using ``gpg --fingerprint [your@address]``).
5. Make sure everyone has verified the fingerprint.
6. Finish the call.
7. Wait for the signed key to be send to you.
8. Import the signatures into your keyring using ``gpg --import
   export.asc``.
9. Once you have received all the signatures, export the public key
   using ``gpg --export -a -o signed.asc [your@address]``.
10. Submit the exported key with your account request.

Procedure for the signers
~~~~~~~~~~~~~~~~~~~~~~~~~

1. Import the public key sent by the signee prior to the conference into
   your keyring (``gpg --import unsigned.asc``).
2. Attend the video conference call arranged by the signee.
3. During the call, establish the identity of the signee either using
   their appearance or by chatting about your prior shared experiences
   working on the Linux kernel (the patches they sent, the discussions
   you had together, etc). Please insist on the webcam use and be wary
   that being in the Linux kernel keyring is sufficiently interesting to
   attackers to attempt "deepfakes" or other video trickery.
4. If you are sufficiently assured that the person on the call is who
   they say they are, confirm the public key they sent to you by asking
   the signee to read it out loud (you can display it using ``gpg
   --fingerprint [their@address]``).
5. If the fingerprint matches, finish the call.
6. Sign the key (e.g. using ``gpg --quick-sign-key [their-key-id]``).
7. Export the signed key using ``gpg --export -a -o signed.asc
   their@address``.
8. Send the signed key back to the signee via email.
