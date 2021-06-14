(BETA) linux.dev mailbox hosting
================================

.. note::

   This is a BETA offering. Currently, it is only available to people
   listed in the MAINTAINERS file. We hope to be able to offer it to
   everyone else who can demonstrate an ongoing history of contributions
   to the Linux kernel (patches, git commits, mailing list discussions,
   etc).

Linux development depends on the ability to send and receive emails.
Unfortunately, it is common for corporate gateways to post-process both
outgoing and incoming messages with the purposes of adding lengthy legal
disclaimers or performing anti-phishing link quarantines, both of which
interferes with regular patch flow.

While it is possible to `configure free services like GMail`_ to work
well with sending and receiving patches, it may not be available in all
geographical locales -- or there may be other reasons why someone may
prefer not to have a gmail.com address.

For this reason, we have partnered with Migadu_ to provide a mail
hosting service under the linux.dev domain.

.. _`configure free services like GMail`: https://git-scm.com/docs/git-send-email#_examples
.. _Migadu: https://migadu.com

Who qualifies for a linux.dev address
-------------------------------------
Please check that all of the below applies to you:

1. You have an actual need to have a mailbox that is compatible with
   git-send-email and IMAP mail clients. **This is NOT a vanity email
   address service** -- it exists to help developers do their work. If you
   really want a vanity address, you can `get a lifetime linux.com
   forwarding alias`_ for a very reasonable price. Alternatively, Migadu
   will happily host your personal domain for a low annual fee.
2. **You are listed in the MAINTAINERS file** as a M: (maintainer) or R:
   (reviewer), **OR**
3. *(NOT YET AVAILABLE) You can demonstrate that you have ongoing history
   with Linux kernel development, such as prior git commits in your
   name, mailing list discussions accessible via lore.kernel.org, or
   similar.*

.. _`get a lifetime linux.com forwarding alias`: https://docs.linuxfoundation.org/lfx/my-profile/purchasing-linux-email

Rules and restrictions
----------------------
Please make sure that you're okay with all of the following:

1. **You will use this service for Linux kernel work.** This doesn't mean
   you can't use it for an occasional email that has nothing to do with
   Linux, but please keep it within reason.
2. **You will not use this service for sending spam or in violation of the
   Code of Conduct** (`see here`_). Such behavior will result in
   immediate suspension or termination of your account.
3. **Your address will be created as givenname.familyname@linux.dev**,
   unless you can convincingly demonstrate that this does not apply to
   your individual case (e.g. you do not have a family name following
   common Western tradition; or you prefer your family name to be listed
   first; or if revealing your full name would put you into any kind of
   personal jeopardy). The hope is to exactly match what you put in your
   git commit "Author" info. Collisions will be handled on a
   case-by-case basis with the hope to satisfy all parties involved.
4. **You will not use this address to subscribe to high-volume mailing
   lists** (LKML, netdev, etc). Migadu applies daily limits on the number
   of incoming and outgoing messages, so violating this request may
   result in mail being held/throttled for all users of linux.dev. We
   will be providing alternative ways to "subscribe" to mailing lists
   hosted on lore.kernel.org in the near future that don't require
   traditional SMTP delivery (or you can already use a `NNTP reader`_
   for this purpose).
5. **You need to already have another email address in order to apply for
   this service** -- which is a bit of a chicken and egg situation, but
   it is what it is. This service is not intended to be your primary
   online identity, merely one that is friendly for Linux development.
6. We hope to provide this service for as long as email is used for
   Linux development, but **we offer no guarantee that this service will
   be around forever**.

.. _`see here`: https://www.kernel.org/code-of-conduct.html
.. _`NNTP reader`: nntp://nntp.lore.kernel.org/

How to apply
------------
We are still hashing out a streamlined mechanism for submitting new
account requests, but for now simply send an email to
helpdesk@kernel.org using the following pattern::

    Subject: firstname.lastname@linux.dev account request

    Full name: [Firstname Lastname]
    Contact address: [myotheraddress@example.org]

    Reasons for needing this account:
    [Describe why your current email situation is not suitable for
    kernel work.]

If your request is approved, you will receive an invitation link at the
address you specify.
