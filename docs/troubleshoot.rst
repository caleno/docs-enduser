.. |date| date::

Troubleshoot
============

Last changed: |date|

.. contents::

Lost access to instance
-------------------------------------------------

There can be multiple reasons for losing access to the instance.

- lost SSH key 
- wrong disk mount path 
- problems with NIC/network  
- 

Dont fret, there is maybe a way to fix this by accsing the console/terminal.
But you need to do some "hacks" to do so if you didnt set/change a users password. 


Possible solution
.................

Got to console, press the "Send ctrlAltDel" button then activate the console window and interrupt the boot by pressing an arrow key for example. Choose a boot entry and press 'e' for edit.
Depending on which OS is in use you can edit the boot loader in the console and boot to single user by adding single to the end of the line that start with linux on ubuntu, on centos you have to remove all console=ttys besides the tty0 and add rd.break enforcing=0 at the end of the line starting with linux16. There a several documentations on this you can find searching the web. On centos you have to be fast to interrupt the normal boot.
Depending on if you are using Ubuntu or Centos you should now have a console and logged in as root.
The keyboard layout is probably en_US.UTF8 which means you have to figure out what keys on your keyboard represent =, /, - and : etc.

On my keyboard (norwegian):
= is \ left key from backspace
/ is - left key from right shift
- is ? second left key from backspace
: is shift+ø

Now you can issue a password change for e.g. the root account by running passwd or passwd username
If you are using Centos you have to do some additional steps as follows.
You need to mount /sysroot by running mount -o remount,rw /sysroot and then change root by running chroot /sysroot.
Now you can run e.g passwd
After you've don that, reboot and log in to console again on normal boot.
Now you can fix the authorized_keys. I fetched my public ssh keys from github.

E.g
     wget https://github.com/username.keys
or
     curl -o pub.keys https://github.com/username.keys

Then add or replace the keys in authorized_keys

E.g
     cat username.keys >> authorized_keys
The authorized_keys file is located in /home/username/.ssh/authorized_keys
Now you should be able to login using ssh with the new keys.


----------

All new users will get a pass phrase to use against the API when they provision
a personal project (see :doc:`login`). There are no means of retrieving this
pass phrase at the moment. Please contact us in case it is lost.


Outdated size
-------------
.. _flavors: http://docs.nrec.no/changelog.html#id1

As we have updated flavors_, the users that have had access to the larger machines may now notice new size status "Outdated" on the Horizon dashboard. Those flavors are not available anymore, but it will not affect the running instances.


Missing network when provisioning from snapshot
-----------------------------------------------

Debian 9, 10
''''''''''''

IPv6 is broken in an instance started from a snapshot, and this can also affect
the original instance. If the resolver addresses is configured using their IPv6
addresses, even IPv4 is affected. This issue appears regardless of which network
is selected for the instance. Here is a workaround:

1. Log in to the instance as the **debian** user

#. Remove the IPv6 dhclient leases file::

     rm /var/lib/dhcp/dhclient6.eth0.leases

#. Log out and shut down the system

#. Create a snapshot

#. The original instance might be restarted at this point

You should now be able to create new machines based upon this snapshot and get
fully functional networks.


CentOS 7
''''''''

.. NOTE::
   This issue only affects CentOS 7 instances provisioned from our
   GOLD image before 2019-01-01. As of January 1, 2019 the GOLD image
   for CentOS 7 is upgraded to CentOS 7.6, and the networking setup
   has been fixed.

There is an issue with CentOS and provisioning instances from a
snapshot. This is due to a local workaround we have added to mitigate
a bug in the CentOS cloud-init package. This bug is fixed in CentOS
7.6 onwards. However, for instances originally provisioned with CentOS
7.5 or older this is a problem. Here is how to fix this:

#. Log in to your instance as the **centos** user

#. Make sure that the instance is fully updated::

     sudo yum upgrade -y

#. Make sure that the instance is running at least CentOS 7.6
   (example)::

     [centos@centos ~]$ cat /etc/centos-release
     CentOS Linux release 7.6.1810 (Core)

#. Install the **NetworkManager** package::

     sudo yum -y install NetworkManager

#. Enable the **NetworkManager** service::

     sudo systemctl enable NetworkManager

#. Remove the file
   ``/etc/cloud/cloud.cfg.d/99-disable-network-config.cfg``::

     sudo rm /etc/cloud/cloud.cfg.d/99-disable-network-config.cfg

#. Create a file ``/etc/cloud/cloud.cfg.d/custom-networking.cfg``
   with the following contents::

     network:
       version: 2
       ethernets:
         eth0:
           dhcp4: true
           dhcp6: true

After this change, you should be able to take a snapshot from the
instance, and use that snapshot to provision other
instances. Networking should just work. Note that we have introduced a
significant change to the original instance. This instance should be
rebooted after the changes, if possible.


Cannot delete DNS zones or records in dashboard
-----------------------------------------------

.. _Deleting records in CLI: dns.html#id7
.. _Deleting a zone in CLI: dns.html#id8

Currently, the GUI module for the DNS service has a Javascript bug
which prevents deletion of zones and records from the GUI. Preliminary
testing suggests that thus bug is fixed in the next release of
Openstack (the "Rocky" release). An upgrade to the "Rocky" release is
planned later this year. For now, zones and records can be deleted
using the API, for example via the command line (CLI):

* `Deleting records in CLI`_
* `Deleting a zone in CLI`_

Instance name
------------
We recommend you to name your instances only with [a-zA-Z0-9]
characters to avoid any maintenance issues.


Security Groups caution
-----------------------
When creating security groups via the API (e.g. Terraform), be as explicit as
possible when setting parameters. In one case we discovered that opening a port
range for all IPs without explicitly setting 0.0.0.0/0 for the remote-ip
parameter (which is default) opened all ports for all IPs. We routinely report
bugs to Openstack developers, however, this is how to work around the problem
for now.

Security group rules created in the dashboard are not affected by this bug,
however, make sure your CIDR notation is correct and make sense to avoid having
Openstack correcting it by guessing what your intentions are. Use a CIDR
calculator if you're unsure.

Users are always advised to ensure their security group rules work as intended
in regards to both IP and port filtering.


Can choose both IPv6 and dualStack
----------------------------------

.. _IPv6 or dualStack: networking.html#ipv6-or-dualstack

It is possible, when creating an instance, to select more than one
network on a single host. As described in `IPv6 or dualStack`_ you
should only select one network.

If an instance has more than one network enabled, it will most likely
not work correctly. In order to fix this issue, do the following:

#. Shut down the instance

#. In the GUI, select **Detach Interface** and select the network you
   wish to remove

#. Start the instance

It may take a few minutes for the instance to become available with
the fixed networking setup.


GOLD image may vary between regions
-----------------------------------

As our GOLD images are built separately for each region, and not necessarily on
the same day, the base upstream image may be altered between the builds. Thus
there may be some differencies between instances started at the same time in our
two regions, even though they may seem to be started from the same GOLD image.
