lore.kernel.org
===============

lore.kernel.org is the kernel.org mailing list archive service. It provides
searchable archives of Linux kernel development mailing lists using
`public-inbox`_, a git-based archival system.

.. _public-inbox: https://public-inbox.org/

Accessing archives
------------------

Archives are available through multiple interfaces:

- **Web interface**: https://lore.kernel.org/
- **NNTP**: nntp://nntp.lore.kernel.org/
- **Git clone**: Each list can be cloned as a git repository for local access

The web interface supports searching across all archived lists and provides
multiple export formats including mbox and git bundles.

Importing messages into your inbox
----------------------------------

To import lore.kernel.org messages directly into your mail client, use
:doc:`korgalore`. This is the recommended approach for following mailing list
traffic without traditional subscriptions, especially for users of Gmail or
other providers that throttle high-volume mailing list delivery.

Tracking a subsystem
~~~~~~~~~~~~~~~~~~~~

Maintainers can use korgalore's ``track-subsystem`` command to automatically
follow their subsystem's mailing list traffic and patches. This parses the
MAINTAINERS file and creates queries for messages to the subsystem's mailing
lists and patches touching subsystem files:

.. code-block:: bash

   # Track from a kernel source tree
   cd ~/linux && kgl track-subsystem 'BTRFS'

   # Track from anywhere (fetches MAINTAINERS automatically)
   kgl track-subsystem 'DRM'

See :doc:`korgalore` for more details.

Local email interface (lei)
---------------------------

For advanced users, public-inbox provides ``lei`` (local email interface), a
tool for searching lore archives and creating saved queries that deliver to
local storage. See :doc:`lore-lei` for a quick start guide.

Adding a new list
-----------------

Most kernel-related mailing lists are already archived on lore.kernel.org. If
you need to request archival of a new list, see :doc:`lore-archives`.
