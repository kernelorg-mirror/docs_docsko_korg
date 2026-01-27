Korgalore
=========

Korgalore is a tool for delivering public-inbox archives directly into mail
targets (Gmail, JMAP, IMAP, or local maildir) as an alternative to traditional
mailing list subscriptions. It provides a workaround for Gmail's hostility to
high-volume technical mailing list traffic.

We recommend korgalore for kernel developers who want to receive mailing list
messages directly in their inbox without dealing with subscription-related
delivery issues.

Why use korgalore?
------------------

Gmail routinely throttles incoming mailing list messages, marks them as spam,
or drops them based on unknown internal heuristics. This throttling causes
hundreds of thousands of messages to sit in the kernel.org mail queue waiting
for delivery.

Korgalore bypasses these issues by fetching messages from lore.kernel.org
archives and importing them directly into your mailbox via API, avoiding the
traditional email delivery path.

Features
--------

- Direct integration with lore.kernel.org archives
- Multiple delivery targets: Gmail, JMAP (Fastmail), IMAP, local maildir
- Thread tracking for following specific discussions
- Subsystem tracking using MAINTAINERS file queries
- GNOME taskbar application for background syncing
- Bozofilter for blocking unwanted senders

Installation
------------

.. code-block:: bash

   pipx install korgalore

Gmail credentials for kernel.org account holders
------------------------------------------------

Gmail requires OAuth credentials to access the API. Kernel.org account
holders can obtain pre-configured credentials by running:

.. code-block:: bash

   ssh git@gitolite.kernel.org get-kgl-creds

Place the downloaded file at ``~/.config/korgalore/credentials.json``.

Basic usage
-----------

1. Create a configuration file:

   .. code-block:: bash

      kgl edit-config

2. Authenticate with your mail target:

   .. code-block:: bash

      kgl auth

3. Pull messages from configured mailing lists:

   .. code-block:: bash

      kgl pull

4. Import a specific thread on demand:

   .. code-block:: bash

      kgl yank --thread https://lore.kernel.org/lkml/msgid@example.com/

Documentation
-------------

Full documentation is available at:

- https://korgalore.docs.kernel.org/

For support, contact tools@kernel.org.
