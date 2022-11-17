social.kernel.org
=================
If you are looking for a fediverse-enabled microblogging platform, there
is a hosted solution offered at https://social.kernel.org/, running
`pleroma`_.

Please read the `about`_ page for terms of service and the privacy
policy.

.. _`pleroma`: https://pleroma.social/

Requesting an invite
--------------------
This service is offered to anyone with a kernel.org account, or anyone
with an entry in the MAINTAINERS file. If you do not fall under one of
these two categories, you can get someone else to "sponsor" your
request.

If you already have a kernel.org account, you can display the invite
link by running the following command::

    ssh git@gitolite.kernel.org social invite

With all other cases, please follow the instructions on the `about`_
page to submit a request to helpdesk.

.. _`about`: https://social.kernel.org/about/

If you aren't in MAINTAINERS
----------------------------
Note, that there is nothing inherently special about the
social.kernel.org instance. There are many fediverse-enabled platforms
providing free registrations, many of which are specifically dedicated
to technology and Free Software communities. Here are some examples:

* https://fosstodon.org
* https://floss.social
* https://hachyderm.io

Many others exist if these are not suitable for your needs.

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
