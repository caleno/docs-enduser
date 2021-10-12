.. |date| date::

Troubleshoot
============

Last changed: |date|

.. contents::

Lost access to instance
-------------------------------------------------
.. _lostaccess:

There can be multiple reasons for losing access to the instance.

- Lost SSH key 
- Disk trouble e.g. wrong mount path (Rescue instance)
- Problems with NIC/network (Rescue instance)

Don't fret, there is maybe a way (workaround) to fix this by accsessing the console/terminal.
But you need to do some "hacks" to do so if you didn't set/change a users password. 


Possible solution (workaround [1]_)
.................

Got to console, press the "Send ctrlAltDel" button then activate the console window and interrupt the boot by pressing an arrow key for example. Choose a boot entry and press :kbd:`e` for edit.
Depending on which OS is in use you can edit the boot loader in the console and boot to single user by adding single to the end of the line that start with linux on ubuntu, on centos you have to remove all ``console=ttys`` besides the ``tty0`` and add ``rd.break enforcing=0`` at the end of the line starting with linux16. 
There a several exampels and documentation on how to start your Linux server/instance in singel mode (root access) which you can find by searching the web. On centos you have to be fast to interrupt the normal boot.
Depending on if you are using Ubuntu or Centos you should now have a console and logged in as root. The keyboard layout is probably en_US.UTF8 which means you have to figure out what keys on your keyboard represent :kbd:`=`, :kbd:`/`, :kbd:`-` and :kbd:`:` etc.

On my keyboard (norwegian):

:kbd:`=` is :kbd:`\\` left key from backspace

:kbd:`/` is :kbd:`-` left key from right shift

:kbd:`-` is :kbd:`?` second left key from backspace

:kbd:`:` is :kbd:`shift` + :kbd:`ø`

Now you can issue a password change for e.g. the root account by running passwd or passwd username
If you are using Centos you have to do some additional steps as follows.
You need to mount :file:`/sysroot` by running `mount -o remount,rw /sysroot` and then change root by running `chroot /sysroot`.
Now you can run e.g `passwd`
After you've don that, reboot and log in to console again on normal boot.
Now you can fix the authorized_keys. I fetched my public ssh keys from github.

E.g
``wget https://github.com/username.keys``
or
``curl -o pub.keys https://github.com/username.keys``

Then add or replace the keys in authorized_keys

E.g
``cat username.keys >> authorized_keys``

The authorized_keys file is located in :file:`/home/username/.ssh/authorized_keys`
Now you should be able to login using ssh with the new keys.


----------

All new users will get a pass phrase to use against the API when they provision
a personal project (see :doc:`login`). There are no means of retrieving this
pass phrase at the moment. Please contact us in case it is lost.


Rescue instance
-------------
.. _rescue mode: https://docs.openstack.org/nova/latest/user/rescue.htmlhttp://docs.nrec.no/changelog.html#id1

You can find official documentation on hoe to put you instance in rescue mode_, 

.. NOTE::
   Setting a password when activating rescue mode dose not work.
   If you lost access to the SSH key take a look at lostaccess_




.. [1] Since setting a password when rescuing an instance do not work.