Recommended email hosting options
=================================

This document provides recommendations for email hosting setups suitable for
kernel maintainers, especially given the increasing limitations of consumer
email providers like Gmail and Outlook.

Own domain with Fastmail
------------------------
The preferred setup is to use your own domain with Fastmail. This is not a
free option, but costs approximately 100 USD per year with a .net domain.

**Advantages:**

- You own your domain and address; if Fastmail stops being suitable in the
  future, your identity is not tied to them and you can migrate to another
  hosting service
- You can purchase your domain through Fastmail itself, and it will be
  preconfigured correctly for immediate use (you can still transfer it to a
  different registrar if you wish); .net domains are cheapest at approximately
  30 USD per year
- You can send plaintext email through their webmail service (see
  https://useplaintext.email/#fastmail)
- Their mobile app supports sending plaintext email
- Full SMTP and IMAP support for use with any mail client
- Can be configured to send kernel.org mail through our servers, allowing
  direct use of your kernel.org alias
- JMAP support is available; lei/korgalore support uploading messages to your
  inbox with JMAP, which is faster and more reliable than Gmail or IMAP

**Disadvantages:**

- Fastmail is an Australian company with servers in the US
- Their server stack is largely proprietary, though they open-source parts of
  their software

linux.dev hosting
-----------------
The Linux Foundation offers linux.dev mailbox hosting for anyone listed in the
MAINTAINERS file, providing a first.last@linux.dev address. The provider is
Migadu, and LF pays an annual fee to offer this service at no cost to
maintainers.

See :doc:`linuxdev` for more information on applying for an account.

**Advantages:**

- Available to any maintainer at no cost
- Uses free software services (Postfix, Dovecot)
- Can send plaintext email through webmail
- Standard IMAP/SMTP that work well with mobile apps (e.g., FairEmail)
- Servers are located in Switzerland

**Disadvantages:**

- Naming convention is enforced (first.last@linux.dev) to avoid collisions
- Owned by LF; your inbox can be accessed by the LF admin team
- You may encounter throttling issues with large mail volumes
- Outgoing nodes occasionally end up on blocklists, causing bounces

Self-hosted with kernel.org address
-----------------------------------
You can use your kernel.org alias with your own mail server. The server needs
a DNS entry and must accept incoming connections on port 25 for forwarding.
You can self-host a Nextcloud instance, for example, and use the bundled
webmail and IMAP access for your mail. For outgoing mail, you can use
kernel.org SMTP servers.

See :doc:`mail` for information on configuring your kernel.org mail settings.

**Advantages:**

- You own everything and nobody else has access to your email
- Completely free (excluding time spent on setup and maintenance)
- No blocklist concerns when sending via smtp.kernel.org

**Disadvantages:**

- You are responsible for running your own infrastructure

Gmail/Outlook with korgalore
----------------------------
The korgalore tool works well for mailing list subscriptions. If
you use Gmail only as a mail storage backend while using a client like
mutt/neomutt/Thunderbird to read and send email, you can continue to use Gmail
along with your kernel.org alias.

See :doc:`korgalore` for more information.

**Advantages:**

- Use the Gmail/Outlook frontend for AI integrations or other features to read
  and organize mail

**Disadvantages:**

- Cannot send plaintext email via mobile apps
- Relies on continued IMAP and SMTP availability from these providers
