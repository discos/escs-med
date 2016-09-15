.. _escsCore1:

escsCore1
----------

This is the machie which runs most of the ACS containers and performs data
storage operations.

configuration files can be found on the svn repo at::

    http://svn.med.ira.inaf.it/repos/escsconf/escsCore1

The machine main configuration parameters are the following: 

========= =====
PARAMETER VALUE
========= =====
hardware  `Fujitsu Primergy RX101 S6 <http://globalsp.ts.fujitsu.com/dmsp/Publications/public/ds-py-rx100-s6-rh.pdf>`_
CPU       Intel(R) Quad Core(TM) i3 CPU 540 @ 3.07GHz
RAM       4GB
OS        Scientific Linux 5.3 i386 (32 bit)
Kernel    2.6.18-128.1.1.el5PAE
hostname  escsCore1
eth0      192.168.1.104
eth1      192.168.51.104
========= =====

.. _escsCore1_os_installation:

OS installation
~~~~~~~~~~~~~~~

EscsCore1 is installed on a 500GB hard disk so partitioned::

    [root@escsCore1 ~]# fdisk -l

    Disk /dev/sda: 500.1 GB, 500107862016 bytes
    255 heads, 63 sectors/track, 60801 cylinders
    Units = cylinders of 16065 * 512 = 8225280 bytes

       Device Boot      Start         End      Blocks   Id  System
       /dev/sda1   *           1          13      104391   83  Linux
       /dev/sda2              14        6387    51199155   83  Linux
       /dev/sda3            6388        7407     8193150   82  Linux swap / Solaris
       /dev/sda4            7408       60801   428887305    5  Extended
       /dev/sda5            7408        8624     9775521   83  Linux
       /dev/sda6            8625        9233     4891761   83  Linux
       /dev/sda7            9234       60801   414219928+  83  Linux

and::

    [root@escsCore1 ~]# mount
    /dev/sda2 on / type ext3 (rw)
    /dev/sda7 on /data type ext3 (rw)
    /dev/sda6 on /tmp type ext3 (rw)
    /dev/sda5 on /alma type ext3 (rw)
    /dev/sda1 on /boot type ext3 (rw)

During the installation process packages are installed directly from the
installation CD and we selected **KDE desktop environment**.
We removed every accessory package related to office and productivity work.

.. _escsCore1_additional_sofware:

Additional software
~~~~~~~~~~~~~~~~~~~

Software installation is performed in the same way as the escsMaster as detailed
in :ref:`escsMaster_additional_software` and includes:

* cfitsio3100
* CCfits-2.1
* qt-X11-opensource 4.5.2
* gpfs

.. _escsCore1_temporary_data:

ACS Temporary Data
~~~~~~~~~~~~~~~~~~
Temporary data are managed as in **escsMaster** in the same way explained in :ref:`escsMaster_temporary_data`

.. _escsCore1_ntp:

NTP
~~~

Ntp service for system clock synchronization is configured via */etc/ntp.conf*
and we also define a custom */root/bin/plot_loopstats* command::

    [root@escsCore1 /]# mkdir /var/log/ntpstats
    [root@escsCore1 /]# chown ntp:ntp /var/log/ntpstats
    [root@escsCore1 /]# service ntpd start
    [root@escsCore1 /]# chkconfig ntpd on
    [root@escsCore1 /]# /root/bin/plot_loopstats

.. _escsCore1_firewall:

Firewall
~~~~~~~~

Iptables is configured in order to accept connections from the 192.168.1.0/24
and 192.168.51.0/24 subnets::

    [root@escsCore1 /]# /root/bin/firewall

The script will configure and start iptables service while saving its
configuration for sequent reboots. 

.. _escsCore1_nis:

NIS
~~~

Users and groups are imported via yellow pages service **nis**. Service
configuration is done using the files::

    /etc/yp.conf /etc/rc.local /etc/sysconfig/network /etc/idmapd.conf /etc/nsswitch.conf

For the service to run correctly it is important to well configure all domain
names, hostnames and nisdomains to *escsCore1.med.ira.inaf.it*.

.. _escsCore1_nfs_and_automount:

NFS and AUTOMOUNT
~~~~~~~~~~~~~~~~~

Home directories and the whole ACS system are imported from the **escsMaster**
node via NFS. System partition is loaded via fstab.
Note that **/system/introot** directory must exist::

    [root@escsCore1 /]# mkdir /system
    [root@escsCore1 /]# chown -R manager:escs /system
    [root@escsCore1 /]# mkdir /system/introot
    [root@escsCore1 /]# mkdir /system/configuration
    [root@escsCore1 /]# chown manager:escs /system/introot
    [root@escsCore1 /]# chown manager:escs /system/configuration

    [root@escsCore1 /]# less /etc/fstab 
    ...
    192.168.1.98:/exports/system   /system/introot nfs ro,soft,auto,intr,proto=tcp,port=2049,users,exec     0 0

While users' homes are loaded at login time via automount::

    [root@escsCore1 /]# less /etc/auto.master
    ...
    /home   /etc/auto.home  --timeout=90

    [root@escsCore1 /]# less /etc/auto.home 
    *       -fstype=nfs,rw          192.168.1.98:/exports/home/&

