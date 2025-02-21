Repo appearance on git.kernel.org
=================================

You can change how your repository is shown on git.kernel.org using a
special ``refs/meta/cgit`` ref in each repository.

Creating the special ref
------------------------

To create the special ``refs/meta/cgit`` ref, use the following
commands::

    git symbolic-ref HEAD refs/meta/cgit
    git reset --hard
    # add and commit any files you need, see below
    git push origin HEAD:refs/meta/cgit
    git checkout master

Editing files in the special ref
--------------------------------

It is a bit trickier to edit files in the special ref as opposed to a
normal branch::

    git fetch origin refs/meta/cgit
    git checkout FETCH_HEAD
    # make any changes and commit them
    git push origin HEAD:refs/meta/cgit
    git checkout master

Cgit configuration options
--------------------------

You can add a ``cgitrc`` file to the ``refs/meta/cgit`` location, for
example::

    owner=Tux Penguino
    desc=Frobble module development

Many ``repo.*`` configuration parameters are supported (see allowlist
below). For in-depth description of each option, see
`man cgitrc <https://git.zx2c4.com/cgit/tree/cgitrc.5.txt>`_.

Full list of allowed options (make sure there are no spaces around
``=``)::

    # repository owner (e.g. your name)
    owner=Tux Penguino
    # repository description
    desc=Frobble module development
    # generate tarballs with this prefix instead of repo name
    # e.g. instead of linux-stable-x.x.x.tar.gz, do linux-x.x.x.tar.gz
    snapshot-prefix=linux
    # Link to the project home page
    homepage=https://frobblemod.io
    # Default branch to show, if it's not master
    defbranch=frobble
    # turn off .tar.gz snapshots for this repository
    snapshots=0
    # hide the repository from the listing, but leave it accessible via direct link
    hide=1
    # don't make this repository accessible via cgit (but make it still clonable)
    ignore=1

.. note:: Keep in mind that ``ignore=1`` still allows that repository to
   be cloned via ``git:`` or ``https:`` endpoints if someone knows the
   full path to it.

Preventing auto-hiding of repositories
--------------------------------------

Repositories that haven't seen any updates in over 3 years will be
automatically hidden from the main index on git.kernel.org in order to
make that page more navigable and avoid aggressive over-crawling by
bots. If you would like to prevent this from happening, create a
``refs/meta/cgit:cgitrc`` file following instructions above and set
``hide=0``.

Repo-specific about tab
-----------------------

Your repository is probably going to be a clone of linux.git, so if you
want to display different info in the "about" tab of the repo instead of
the default linux README, you can add a separate README file in the
``refs/meta/cgit`` location.

We will look for README, README.md and README.rst files, so if you want
Markdown or ReST formatting in the about tab, you can use the
appropriate file extension.

Note on caching
---------------

CGit heavily relies on caching, so it can take up to a few hours for the
changes you made to the ``refs/meta/cgit`` ref to show up on
git.kernel.org.
