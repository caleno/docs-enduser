=========
Changelog
=========

All major changes to NREC will be listed on this page.


2021-10-08
==========

The GOLD CentOS 8 image is now retired due to the distributions imminent end of
life. We have replaced CentOS 8 with three new GOLD images for "CentOS 8 Stream",
"Rocky Linux 8" and "Alma Linux 8". The former is an upstream el8 distribution,
the last two are el8 downstream distributions modelled after the original CentOS 8
distribution.

2019-02-26
==========

The support for Windows workloads has been reworked, and we have added several
new features. First, we've added support for Windows Server 2019 Standard and
Windows Server 2019 Core in addition to the existing support for Windows Server
2016 Standard. On the new 2019 images a SSH server is automatically installed and
configured. Also, when launching Windows instances in the BGO region, those instances
will be automatically activated. The activation feature is not yet available in the
OSL region for licensing reasons. Furthermore, several fixes and improvements have
been implemented in the images, including automatic disabling of the Administrator
account. New Windows images with the latest cumulative updates from Microsoft will
be automatically created every month, and will be available for UH-IaaS users a few
days after "Microsoft Patch Tuesday". See our updated Windows documentation for more
details: :doc:`create-windows-machine`

2019-02-06
==========

We've upgraded all the services to the Queens version which includes
Compute (Nova), Network (Neutron), Volume (Cinder) and Image (Glance). If you would
like more details, please refer to the Queens release notes available here:
https://releases.openstack.org/queens/index.html
If you run into any problems you suspect are caused by the upgrade, please let
us know.

2019-01-17
==========

We've launched a new logo for UH-IaaS.

2018-11-13
==========

We've upgraded all the remaining services to the Pike version which includes
Compute (Nova), Network (Neutron), Volume (Cinder) and Image (Glance). Some of
you may have noticed that we ran into a few problems the following day, but all
those should be resolved now. If you would like more details, please refer to
the Pike release notes available here:
https://releases.openstack.org/pike/index.html
If you run into any problems you suspect are caused by the upgrade, please let
us know.

2018-10-31
==========

Dashboard (Horizon) has been upgraded to the Pike version. There are small
changes in the GUI, but none of them are very intrusive. If you run into any
problems, please let us know.

2018-10-17
==========

Identity (Keystone) has been upgraded to the Pike version.

2018-08-13
==========

Several issues fixed
--------------------

The APIs for all UH-IaaS services are now open from everywhere. Earlier there
were limitations as from where the APIs were available without the use of VPN or
similar services. The registration web page is still limited, though. This will be
fixed in the near future.

It is now possible to create a volume from an image source.

Creating a snapshot from a running instance no longer breaks network connectivity.
It is still highly recommended to turn off the instance before creating a snapshot.

As an experimental feature it is now possible to boot an instance from a volume, and
create a new instance on a volume from image. For now, however, there are several
constraints. Read more in Known Issues.


2018-07-19
==========

UH-IaaS are now running OpenStack Ocata
---------------------------------------

We've upgraded OpenStack to Ocata, which is the 15th release of the software
UH-IaaS is built on. For end users the most obvious change is the look of
OpenStack's dashboard Horizon. If you run into any problems you suspect are
caused by the upgrade, please let us know.

2018-02-28
==========

Updated Horizon dashboard login page
------------------------------------

We have updated the Horizon dashboard with links to UH-IaaS documentation and first-time login page.

2018-02-26
==========

Updated flavors for instances
-----------------------------

Flavors that define the RAM, CPU and disk capacity of instances have now been updated.

More details can be found in this documentation: http://iaas.readthedocs.io/en/latest/customer/flavors.html


2018-01-24
==========

Updated storage for instances
-----------------------------

The default storage used for instances (the disk where the operating system is
running, short OS-disk) has been updated to use a centralized storage.

This change allows us to do live migrations of instances, which means that we no
longer need to reboot instances when doing maintenance work. New instances in
the availability zone (AZ) <region>-default-1 will now use centralized storage.

We have manually moved all existing instances to a new availability zone (AZ)
called `<region>-legacy-1`. This AZ will still be an available option when
starting new instances. All instances in this AZ will continue to have scheduled
maintenance.

2017-12-01
==========

Changed
-------

Debian 9 (Stretch) and Fedora 27 are now available again with support for IPv6.

2017-10-12
==========

Changed
-------

The networks in UH-IaaS (both regions) that was named "public" are now named "dualStack" - network IDs are the same.
