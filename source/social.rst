social.kernel.org
=================
If you are looking for a fediverse-enabled microblogging platform, there
is a hosted solution offered at https://social.kernel.org/, running
`Akkoma`_.

Please read the `about`_ page for terms of service and the privacy
policy.

.. _`Akkoma`: https://akkoma.social/
.. _`about`: https://social.kernel.org/about/

If you have a kernel.org account
--------------------------------
If you already have a kernel.org account and would like to register on
social.kernel.org, you can display the invite link by running the
following command::

    ssh git@gitolite.kernel.org social invite

.. note::

   Please only create an account if you actually intend to use it. If
   you already have an established presence on the Fediverse elsewhere,
   we encourage you to continue using it and just set the webfinger
   pointer as described below.

If you DON'T have a kernel.org account
--------------------------------------
This service is offered to anyone with an entry in the `MAINTAINERS`_
file, so you can qualify even if you don't have a kernel.org account.

Note, that there is nothing inherently special about the
social.kernel.org instance. There are many fediverse-enabled platforms
providing free registrations, many of which are specifically dedicated
to technology and Free Software communities. Here are some examples:

* https://fosstodon.org
* https://floss.social
* https://hachyderm.io

Many others exist if these are not suitable for your needs.

Nonetheless, if you are convinced that social.kernel.org is the best
place for your account, please send a request to helpdesk@kernel.org,
using the following template::

    To: helpdesk@kernel.org
    Subject: social.kernel.org account request for [Your Name]

    I would like a social.kernel.org account because I am the
    maintainer of the [kernel subsbystem].

.. _`MAINTAINERS`: https://git.kernel.org/pub/scm/linux/kernel/git/torvalds/linux.git/tree/MAINTAINERS

Setting your webfinger
----------------------
If you have a kernel.org account and would like to be "discoverable" via
your @username@kernel.org address, you will need to set up a webfinger
service to point at your home in the fediverse.

To do that, run the following command::

    ssh git@gitolite.kernel.org social set @youraddr@social.service.foo

This will perform the webfinger query to the fediverse service you
specify and set the webfinger response on the kernel.org side of things.

Note, that it takes about 5 minutes for the change to replicate to all
frontends.

If you would like to remove your webfinger info::

    ssh git@gitolite.kernel.org social rm

To see what it is currently set to (if anything)::

    ssh git@gitolite.kernel.org social show

With all other questions, please contact helpdesk@kernel.org.

Recommended client apps
-----------------------
If you are looking for a client application to use with the
social.kernel.org instance, we can recommend the following options.

.. note::

  Since social.kernel.org is running on Akkoma, not Mastodon, some apps
  may not be fully compatible. The ones listed below have been tried and
  found working.

Android
~~~~~~~

* Husky_
* Fedilab_

Another perfectly suitable option is to access it via your regular web
browser or to set up a lite app via Hermit_.

.. _Husky: https://f-droid.org/en/packages/su.xash.husky/
.. _Fedilab: https://f-droid.org/en/packages/fr.gouv.etalab.mastodon/
.. _Hermit: https://play.google.com/store/apps/details?id=com.chimbori.hermitcrab

GUI
~~~

* Whalebird_

Another popular GUI application is Tootle, but it is known not to work
with Akkoma (and is unmaintained).

.. _Whalebird: https://flathub.org/apps/details/social.whalebird.WhalebirdDesktop

Console
~~~~~~~

* Toot_
* Tut_

.. _Toot: https://github.com/ihabunek/toot
.. _Tut: https://github.com/RasmusLindroth/tut
