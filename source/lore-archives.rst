Adding list archives to lore.kernel.org
=======================================

This document describes the process for adding a new mailing list to
lore.kernel.org. This is rarely needed, as most kernel-related lists are
already archived.

.. note:: For information about accessing lore.kernel.org archives, see
   :doc:`lore`.

Prerequisites
-------------

While any mailing list used for Linux kernel development can qualify, we give
priority to mailing lists served via vger.kernel.org.

Since vger.kernel.org has existed for over 20 years, there is one important
caveat we require before adding an existing list to lore.kernel.org -- you must
provide as complete of an archive as possible.

Requesting archival of an existing list
---------------------------------------

- Send a request to helpdesk@kernel.org stating that you would like to have a
  mailing list added to lore.kernel.org.
- Wait till you receive a response, indicating that:

  - Your request is approved
  - We have subscribed the collector to the list and started collecting
    preliminary messages

Collecting archive donations
----------------------------

You will need to provide an as-complete-as-possible archive of list messages.
If you have been subscribed to the list for a while, you can start with your
own archives.

Exporting archives from Gmail
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

If you've been subscribed to the list via Gmail, it is easy to collect the
initial set of archives.

- Start by creating a label

    - Gear->Settings
    - Select the "Labels" tab
    - Click "Add label"
    - Give it a name like "export-[listname]"

- Switch to "Filters and blocked addresses" tab

    - Click "Create a new filter"
    - In the pop-up window, put the list address in the "Include these words"
      field

        - This is better than using the "List-Id" because doing so will exclude
          your own messages sent to that list
        - Do not worry, if anything extra is caught in this sweep, it will get
          filtered out during the next step

    - Click the "Search" button to make sure this looks sane
    - Click the dropdown arrow in the search field to bring up the popup again
    - Choose "Create filter"
    - Check the box "Apply the label"

        - Choose the label you created in the previous step
        - MAKE SURE you check "also apply filter to matching conversations"

    - Click "Create filter"
    - Wait till you get "Your filter was created" response (big lists will take
      a while)

- Go to Google's `Data takeout <https://takeout.google.com/settings/takeout>`_.

    - Click "Select none" to uncheck everything
    - Locate the "Mail" entry and enable it
    - Click on "All mail" and choose "Select labels"
    - Uncheck all labels except the one you created in the previous step
    - Scroll to the bottom of the page and choose "Next"
    - Choose the archive format you prefer (probably ".tar.gz")
    - The maximum size doesn't really matter, so leave it at the default setting
    - Choose "Send download link via email"
    - Click "Create archive"

Wait till you receive the download link email (may take a while) and
follow the instructions you receive. The archive should contain one or
several files in .mbox format that you need. Extract them and proceed to
the next section.

Sanitizing the archive
~~~~~~~~~~~~~~~~~~~~~~

We provide a list-archive-maker script that will remove unnecessary headers
from the mbox file and make sure that all messages in it belong to the mailing
list.

- Download the list-archive-maker script:

    - https://git.kernel.org/pub/scm/linux/kernel/git/mricon/korg-helpers.git/plain/list-archive-maker.py
    - it should work with most versions of python3 and doesn't require special
      libraries

- Run it on any file in .mbox format to make a sanitized archive broken down by
  month

Example with linux-kernel-announce@vger.kernel.org::

    mkdir ~/linux-kernel-announce
    ./list-archive-maker.py -s export-linux-kernel-announce.mbox \
        -e ~/linux-kernel-announce \
        -k ~/linux-kernel-announce-known-ids.txt \
        -l linux-kernel-announce.vger.kernel.org

This will sanitize the archives and put them into
~/linux-kernel-announce in your home directory.

Soliciting archives from others
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Once you have your initial archive, you can solicit donated archives
from others, who may have been subscribed to the list for longer, or
have messages that did not get delivered to you for some reason.

You will need the know-ids.txt file from the previous step, to make sure
that others don't send you messages you already have.

Send a message to the list, something like this (edit to your liking)::

    Hi, everyone:

    I'm working to add this list to lore.kernel.org. As one of prerequisites
    they require that we provide full existing archives of all list messages
    (or, at least, as complete as possible). I've collected mine already, but
    would really appreciate if you could pitch in from your own collection.

    Just follow the instructions on this page:
    https://korg.docs.kernel.org/lore-archives.html

    I've attached the list of message-ids that I already have. You'll need it
    during the archive sanitization process to pass to the -k switch.

    Please tar up and xz -9 the resulting directory with mbox files and send
    the archive to me so I can add it to what I already have.

    Thanks!

Merging multiple archives into one
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

If you receive any archives, you can merge them with what you already
have using the same script. Make sure you're pointing at the same
known-ids.txt file as the one you attached to the solicitation email,
otherwise you will have unnecessary duplicate entries::

    ./list-archive-maker.py -s dir-with-mbox-files/* \
        -e ~/linux-kernel-announce \
        -k ~/linux-kernel-announce-known-ids.txt \
        -l linux-kernel-announce.vger.kernel.org

Send the archive to helpdesk
~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Once you've done your best collecting archives, tar them up and compress
them::

    tar cf full-archives.tar dir-with-mbox-files
    xz -9 full-archives.tar

If the resulting file is smaller than a few MB, you can attach it to the
follow-up email to helpdesk@kernel.org. If the archive is larger than
that, please upload it to a file sharing service, such as Google Drive:

- Upload the .tar.xz file to drive.google.com
- Right-click on the file name and choose "Get shareable link"
- Click on "Sharing settings" and make sure that it is set to "Anyone with the
  link"
- Paste the link in the email response to helpdesk@kernel.org.

The kernel.org administrators will combine the archive they receive from
you with the messages they have pre-collected, to make sure there is no
interruption between when your archive collection was completed and when
public-inbox starts receiving new messages. You will be notified when
the mailing list is ready to be accessed via lore.kernel.org.

Can we use Mailman archives?
~~~~~~~~~~~~~~~~~~~~~~~~~~~~

We *can* use Mailman's pipermail archives, but it's best if it's done as
a final filler for anything not collected elsewhere, for these reasons:

- Mailman's pipermail .gz archives are not, technically, valid mailboxes
  (they don't properly escape "From " in message bodies). They must be
  mangled to be properly imported.
- They have bare minimal headers that don't include any "Received"
  lines, original recipients, cc's, or List-ID.
- They mangle the mime structure of the message to scrub any
  attachments, including digital signatures.
- They escape email addresses to make @->" at " replacements, which must
  then be replaced back.

We suggest you proceed with archive collection per above, and then fill
in any missing bits with pipermail archives as the last step (the
list-archive-marker.py script has automated functionality to do this).
