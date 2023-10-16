Using your username@kernel.org alias
====================================

We do not provide mailbox hosting to kernel.org members, but we do
provide a redirect alias to the address you specified when requesting an
account. The only other service we offer in addition to the redirect is
outgoing mail SMTP.

Sending outgoing mail
---------------------
If you want to use your kernel.org address for sending outgoing mail,
you *must* use our SMTP server, otherwise your messages will be rejected
by many providers due to DMARC policy violations.

First, you will need to obtain your SMTP password using the following
command::

    ssh git@gitolite.kernel.org getsmtppass

This will produce the following output::

    SMTP password for user [username] was set to: [randomstring]

    Example mutt settings:

        set smtp_url     = "smtps://[username]@mail.kernel.org:465/"
        set smtp_pass    = "[randomstring]"
        set from         = "[username]@kernel.org"

    Example git config settings:

        [sendemail]
                smtpserver     = mail.kernel.org
                smtpserverport = 465
                smtpencryption = ssl
                from           = [username]@kernel.org
                smtpuser       = [username]
                smtppass       = [randomstring]

    This passphrase will be shown only once. You can get a new one by
    running:

        ssh git@gitolite.kernel.org getsmtppass reset

    This will reset any existing smtp password, so use with care.

You should be able to use this output to configure any other client for
authenticated SMTP sending. The automatically generated random password
is not used for anything else, so there is little concern in leaving it
cleartext in your configuration files.

Adding a kernel.org UID to your PGP key
---------------------------------------
If you are sending PGP-signed mail using your username@kernel.org email
address, you should add that UID to the public key (should be the same
key you used to apply for your kernel.org account)::

    gpg --quick-add-uid [keyid] 'Firstname Lastname <username@kernel.org>'

To find out your keyid, you can run::

    gpg --list-secret-keys

Your keyid (either the full fingerprint, or the last 16 characters)
should be listed right under the sec line.

The kernel.org Web Key Directory
--------------------------------

We publish the `Web Key Directory <https://wiki.gnupg.org/WKDHosting>`_
(WKD) for all accounts, so once you add the kernel.org uid to your
public key, people will be able to obtain your key automatically if they
use an email client that supports automatic WKD key retrieval. If they
use the default TOFU trust mechanism, the key retrieved from kernel.org
will be automatically marked as trusted.

To check which key we have in the WKD, you can run the following
command::

    GNUPGHOME=$(mktemp -d) gpg --auto-key-locate wkd --locate-keys [username]@kernel.org

The output should display which key ID was retrieved from the WKD. If
instead you see an error message like this::

    gpg: key [keygrip]: no valid user IDs
    gpg: Total number processed: 1
    gpg:           w/o user IDs: 1
    gpg: error retrieving '[username]@kernel.org' via WKD: No fingerprint

This means there is no corresponding username@kernel.org uid on the key
retrieved from the directory. If you've just added the kernel.org uid to
your key, it takes about 24 hours for the WKD to be regenerated. If you
repeatedly get this error even after 24 hours, please contact
helpdesk@kernel.org.

Changing your forwarding address
--------------------------------

If you have a kernel.org ssh account
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

You can self-administer your forwarding address if you have an active
ssh account allowing you to access gitolite.kernel.org. To list your
current forwarding address(es)::

    ssh git@gitolite.kernel.org mailforward list

To add another forwarding address in addition to what is already present::

    ssh git@gitolite.kernel.org mailforward add foo@example.com

To replace your current forwarding address(es)::

    ssh git@gitolite.kernel.org mailforward set foo@example.com

For other commands and more information, run::

    ssh git@gitolite.kernel.org mailforward help

If you don't have an active ssh account
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
Please send a request to helpdesk@kernel.org with your username and the
new desired destination.

.. important:: The new address must match one of the UIDs on your PGP
   key and exist on keyservers. See the section above for instructions
   on how to add a UID to your PGP key.

We can also forward to multiple destinations. Follow the same procedure
to request additional destinations.

Topical addresses
-----------------
We support using ``+topic`` addressing, so you can use any number of
``username+topic@kernel.org`` addresses when doing things like
subscribing to mailing lists. E.g.:

* username+lkml@kernel.org

They will be correctly delivered to your forwarding address.
