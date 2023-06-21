Convenient URL shorteners for git.kernel.org
============================================

There is a simple framework of convenient URL shortcuts for
git.kernel.org, for repositories hosted in pub/scm/linux/kernel/git
(where most Linux trees are).

/c/ for commits
---------------

::

    /[name]/c/[sha1]        -> /pub/scm/linux/kernel/git/[name]/linux.git/commit/?id=[sha1]
    /[name]/[repo]/c/[sha1] -> /pub/scm/linux/kernel/git/[name]/[repo].git/commit/?id=[sha1]

If *[repo]* is omitted, "linux.git" is assumed.

Examples
~~~~~~~~

* https://git.kernel.org/torvalds/c/4f27395
* https://git.kernel.org/tip/tip/c/2923b27

The commit can be a full sha1 or at least 7 chars long; it can also be a
tag name or any other tree object, e.g.::

    https://git.kernel.org/torvalds/c/v4.2-rc1

/p/ for patches
---------------

::

    /[name]/p/[sha1]        -> /pub/scm/linux/kernel/git/[name]/linux.git/patch/?id=[sha1]
    /[name]/[repo]/p/[sha1] -> /pub/scm/linux/kernel/git/[name]/[repo].git/patch/?id=[sha1]

If *[repo]* is omitted, "linux.git" is assumed.

Examples
~~~~~~~~

* https://git.kernel.org/torvalds/p/4f27395
* https://git.kernel.org/tip/tip/p/2923b27

/h/ for Head Trees
------------------

::

    /[name]/h/[head]        -> /pub/scm/linux/kernel/git/[name]/linux.git/tree/?h=[head]
    /[name]/[repo]/h/[head] -> /pub/scm/linux/kernel/git/[name]/[repo].git/tree/?h=[head]

If *[repo]* is omitted, "linux.git" is assumed.

Examples
~~~~~~~~

* https://git.kernel.org/clk/h/clk-next
* https://git.kernel.org/next/linux-next/h/stable

/l/ for Head Logs
-----------------

::

    /[name]/l/[head]        -> /pub/scm/linux/kernel/git/[name]/linux.git/log/?h=[head]
    /[name]/[repo]/l/[head] -> /pub/scm/linux/kernel/git/[name]/[repo].git/log/?h=[head]

If *[repo]* is omitted, "linux.git" is assumed.

Examples
~~~~~~~~

* https://git.kernel.org/clk/l/clk-next
* https://git.kernel.org/next/linux-next/l/stable

/d/ and /ds/ for diff and diffstat
----------------------------------

::

    /[name]/d/[id1]/[id2]        -> /pub/scm/linux/kernel/git/[name]/linux.git/diff/id=[id1]&id2=[id2]
    /[name]/[repo]/d/[id1]/[id2] -> /pub/scm/linux/kernel/git/[name]/[repo].git/diff/id=[id1]&id2=[id2]

Both *[id1]* and *[id2]* would normally be tags, but can be a full sha1
or at least 7 chars long. Use ``/ds/`` for "stat only" view (e.g. when
full diff is way too huge to be reasonably useful for anything).

If *[repo]* is omitted, "linux.git" is assumed.

Examples
~~~~~~~~

* https://git.kernel.org/torvalds/ds/v4.5-rc1/v4.4
* https://git.kernel.org/tip/tip/d/v4.18-rc2/v4.18-rc1
