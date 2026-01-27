The kernel.org keysigning map
=============================
.. note:: Keysigning via video conference is an acceptable alternative to
   in-person meetings. If you need help organizing a video conference,
   please send an email to keys@kernel.org.

If you need help getting signatures from fellow kernel developers to
apply for a :doc:`kernel.org account <accounts>`, you can look on the
keysigning map to see who is available nearby to sign your key:

* https://www.kernel.org/doc/ksmap/

Each placemark should have the person's email address you can use to
contact them, and their PGP key information.

Adding yourself to the map
--------------------------
If you are already have a kernel.org account, we encourage you to add
your information to the map, so other developers can get into the
kernel.org web of trust. Here's how you should do it:

  - ``git clone git@gitolite.kernel.org:pub/scm/docs/kernel/ksmap``
  - Read the ``Readme`` file
  - Add or edit your ``users/[username].yaml``
  - Test the results by running the ``yaml-to-kml.py`` script per
    ``Readme`` instructions
  - Push back to the repository (all valid users can push)
  - A couple of minutes later, we will autobuild a new
    ``developers.kml`` and sync it to the website. You may need to
    Ctrl-Reload to see your changes.

The picture you see on the placemark is provided by libravatar based on
the email you list in the yaml file. If you want to change it or add
your own, go to https://libravatar.org.

.. note:: Please be mindful of your privacy when putting in your
   coordinates.
