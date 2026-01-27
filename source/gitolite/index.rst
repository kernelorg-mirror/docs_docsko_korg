How to use gitolite
===================
If you have not yet set up your ssh access, first see :doc:`../access`.

Quick start guide to gitolite
------------------------------
You can find the upstream documentation for gitolite at their website:

* https://github.com/sitaramc/gitolite

You probably want to read their user doc section:

* http://gitolite.com/gitolite/user.html

The basics however are straightforward. You have been automatically
granted a wildcard space under ``pub/scm/linux/kernel/git/[username]/``

To create a repository under there either push to the path you want or
perform a git clone. Either of these will create the repository for you.

Example using "git clone"::

    git clone git@gitolite.kernel.org:pub/scm/linux/kernel/git/[username]/foobar

When cloning a tree that already exists on kernel.org, you should use the
gitolite built in cloning system::

    ssh git@gitolite.kernel.org fork pub/scm/from-repo pub/scm/to-repo

This will take care of the git clone, and uses Shared (-s), and Linked
(-l) git options.

E.g. for Linux kernel you should start by forking Linus's repository::

  ssh git@gitolite.kernel.org fork \
      pub/scm/linux/kernel/git/torvalds/linux \
      pub/scm/linux/kernel/git/[username]/linux

.. important:: No leading "/" or trailing ".git"

   - Do not start repository paths with '/', just "pub/scm"
   - Do not end repository paths with ".git", just "kernel/git/[username]/linux"

Some commands may be forgiving and work anyway, but many will not.

Additional commands for gitolite.kernel.org
-------------------------------------------
Note: This list may be incomplete. You can find out more up-to-date info
about available commands if you run::

  ssh git@gitolite.kernel.org help

D
~
Delete a repository. You will need to run ``D unlock`` first, and then
``D rm``. E.g.::

  ssh git@gitolite.kernel.org D unlock pub/scm/linux/kernel/git/[username]/linux
  ssh git@gitolite.kernel.org D rm pub/scm/linux/kernel/git/[username]/linux

desc
~~~~
Sets the repository description. E.g.::

  ssh git@gitolite.kernel.org desc pub/scm/linux/kernel/git/[username]/linux "[username] kernel tree"

.. note:: The **desc** command does not like special characters such as
   quotes, ampersands, brackets, etc. Another way to set your repository
   description is via a cgitrc file in the special __meta__ branch of
   the repo. See :doc:`../cgit-meta-data` for more details.

fork
~~~~
Clone a repository that is already hosted by kernel.org, E.g.::

  ssh git@gitolite.kernel.org fork pub/scm/repo1 pub/scm/repo2

help
~~~~
Displays a help message for what commands are enabled

info
~~~~
Displays access permission you may have to various repos (warning: wall
of text).

getsmtppass
~~~~~~~~~~~
Sets up a random password you can use to authenticate against
mail.kernel.org in order to send outgoing mail. The command output will
also give you some configuration examples for mutt and git.

mailforward
~~~~~~~~~~~
Allows you to modify your mail forwarding address. See :doc:`../mail`
for more details.

2fa
~~~
Allows you to set up your 2-factor authentication token. See
:doc:`2fa` for more details.

symbolic-ref
~~~~~~~~~~~~
Allows you to repoint HEAD to other branches instead of the default.
E.g.::

  ssh git@gitolite.kernel.org symbolic-ref \
    pub/scm/your/repo HEAD refs/heads/dev

.. note:: The command is fully compatible with other ``git-symbolic-ref``
   operations, but only changes to ``HEAD`` will be propagated to the
   public mirrors.

track
~~~~~
Allows you to set up a remote to another repository hosted at
kernel.org, which is useful when you want to avoid sending a lot of
objects during ``git push``. See :doc:`track_set` for
more details.

Using gitolite instead of public mirrors
----------------------------------------
The public mirrors, while generally trusted, don't offer the same level
of protection as the gitolite master, so it is advisable to make sure
that you use the `gitolite.kernel.org` master to apply pull requests.
There is a simple way to instruct git to automatically use the master
whenever a git.kernel.org pull URL is provided.

Just add the following to your ``~/.gitconfig``::

    [url "ssh://git@gitolite.kernel.org"]
        insteadOf = https://git.kernel.org
        insteadOf = http://git.kernel.org
        insteadOf = git://git.kernel.org

Git will now automatically rewrite all ``git.kernel.org`` requests to be
going to the master instead.

