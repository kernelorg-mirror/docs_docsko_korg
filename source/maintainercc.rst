maintainer.cc mail forwarding
=============================
Many maintainers want a company-independent address for their kernel work, but
do not want to manage a separate inbox. The maintainer.cc service provides a
simple mail forwarding address in the form firstname.lastname@maintainer.cc
that delivers to an existing mailbox of your choice.

This is a third address option offered to kernel maintainers, alongside the
two described in :doc:`email-hosting`:

- a :doc:`kernel.org account <accounts>`, which also provides mail forwarding;
- a :doc:`linux.dev account <linuxdev>`, which provides a dedicated inbox.

Compared to a kernel.org account, maintainer.cc is purely an email service: it
does not include ssh access to git repositories, which many maintainers do not
need or want. It also does not require you to go through the PGP key
cross-signing process. Compared to a linux.dev account, you continue to use
your existing mailbox instead of managing a new one.

.. important::

   If you are employed to work on the kernel, check with your employer before
   using a maintainer.cc address for your work. Some organizations have
   policies that restrict using a third-party domain for work-related
   correspondence, and you are responsible for making sure that your use of
   the address complies with them.

Who qualifies for an address
----------------------------
Anyone listed in the MAINTAINERS file as a M: (maintainer) or R: (reviewer)
qualifies for a maintainer.cc address. The qualification rules are similar to
those used for linux.dev accounts; see :doc:`linuxdev` for the full details.

How to apply
------------
To request an account, send an email to helpdesk@kernel.org using the
following template::

    Subject: firstname.lastname@maintainer.cc account request

    Full name: [Firstname Lastname]
    Forwarding address: [foo@example.org]

Sending outgoing mail
---------------------
You can also send outgoing mail from your maintainer.cc address by configuring
your mail client to use the SMTP server below. The exact steps vary between
clients, but the settings are the same. For example, in a webmail provider
such as Gmail you would add the address under a "Send mail as" setting; in a
desktop client you would add it as an outgoing (SMTP) account.

Use the following SMTP server details:

- **SMTP server:** smtp.forwardemail.net
- **Port:** 465
- **Connection security:** SSL/TLS
- **Username:** your full maintainer.cc email address
- **Password:** the password you received when your account was created

After adding the address, your client will usually send a verification message
to confirm that you control it.
