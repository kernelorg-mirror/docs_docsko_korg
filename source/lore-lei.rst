Using lei with lore.kernel.org
==============================

lei (local email interface) is a tool from `public-inbox`_ that allows you to
search lore.kernel.org archives and save queries that deliver matching messages
to local storage. This is useful for following specific topics, patches, or
mailing lists without traditional subscriptions.

.. _public-inbox: https://public-inbox.org/

.. note:: :doc:`korgalore` simplifies many aspects of configuring and
   running lei, so you may want to check that document first.

Installation
------------

On Fedora::

    dnf install lei

On Debian/Ubuntu::

    apt install public-inbox

For other distributions, see the `public-inbox INSTALL documentation
<https://public-inbox.org/INSTALL.html>`_.

Quick start
-----------

The basic workflow is:

1. Create a query with ``lei q``
2. Update the query periodically with ``lei up``

Example delivering to a maildir
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

lei is a powerful tool and can deliver to various targets and formats.
Here is an example of subscribing to LKML and delivering it to a local
maildir, starting with a day's worth of messages (there's usually
several thousand messages a day, so be careful changing that to a
broader time span)::

    lei q -o ~/Mail/lkml -I https://lore.kernel.org/all \
      'l:linux-kernel.vger.kernel.org AND rt:1.day.ago..'

Fetching saved searches
~~~~~~~~~~~~~~~~~~~~~~~

lei saves your queries automatically. To fetch new messages::

    # Update a specific search
    lei up ~/Mail/lkml

    # Update all saved searches
    lei up --all

List your saved searches with::

    lei ls-search

Search syntax
-------------

lei uses the same search syntax as the lore.kernel.org web interface:

**Basic prefixes:**

- ``s:`` - Subject line (e.g., ``s:"memory leak"``)
- ``f:`` - From header (e.g., ``f:torvalds``)
- ``t:`` - To header
- ``c:`` - Cc header
- ``l:`` - List-Id (e.g., ``l:linux-kernel.vger.kernel.org``)
- ``b:`` - Message body
- ``d:`` - Date range (e.g., ``d:2024-01-01..2024-02-01``)
- ``rt:`` - Received time, relative (e.g., ``rt:1.month.ago..``)

**Diff-specific prefixes (for patch searches):**

- ``dfn:`` - Filename in diff (e.g., ``dfn:drivers/gpu/*``)
- ``dfhh:`` - Diff hunk header/function name (e.g., ``dfhh:schedule_*``)
- ``dfb:`` - Added lines in diff
- ``dfa:`` - Removed lines in diff

Example queries
---------------

Follow a subsystem's patches::

    lei q -I https://lore.kernel.org/all -o ~/Mail/drm -t \
      '(dfn:drivers/gpu/drm/* OR l:dri-devel.lists.freedesktop.org) AND rt:1.week.ago..'

Track a specific topic::

    lei q -I https://lore.kernel.org/all -o ~/Mail/rust -t \
      '(s:rust OR dfn:rust/*) AND rt:2.weeks.ago..'

Search for patches touching specific files::

    lei q -I https://lore.kernel.org/all -o ~/Mail/sched -t \
      'dfn:kernel/sched/* AND rt:1.month.ago..'

Managing searches
-----------------

Edit an existing search::

    lei edit-search ~/Mail/lkml

This opens the search configuration in your editor, allowing you to modify the
query or other parameters.

Remove a saved search::

    lei forget-search ~/Mail/lkml

Automation
----------

Set up a cron job or systemd timer to keep your searches updated::

    # Add to crontab
    */30 * * * * lei up --all

Or create a systemd user timer at ``~/.config/systemd/user/lei-up.timer``::

    [Timer]
    OnBootSec=5min
    OnUnitActiveSec=30min

    [Install]
    WantedBy=timers.target

With a corresponding service at ``~/.config/systemd/user/lei-up.service``::

    [Service]
    Type=oneshot
    ExecStart=/usr/bin/lei up --all

Enable with::

    systemctl --user enable --now lei-up.timer

Further reading
---------------

- `lei overview <https://public-inbox.org/lei-overview.html>`_
- `lei-q manual <https://public-inbox.org/lei-q.html>`_
- `Xapian query syntax <https://xapian.org/docs/queryparser.html>`_
