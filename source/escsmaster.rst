.. _escsMaster:

escsMaster
----------

This machine is the main responsible of the running system and the most critical possible point of failure.
It is its responsibility to:

* run the acs container and services daemons
* export user accounts and groups information to the system via YP
* export user home directories via NFS
* export ESCS installation via NFS
* arbitrate and manage the GPFS storage cluster
* act as a NAT and as a firewall for the private networks .51 and .1

configuration files can be found on the svn repo at::

    http://svn.med.ira.inaf.it/repos/escsconf/escsMaster

The machine main configuration parameters are the following: 

========= =====
PARAMETER VALUE
========= =====
hardware  `Fujitsu Primergy RX100 S6 <http://globalsp.ts.fujitsu.com/dmsp/Publications/public/ds-py-rx100-s6-rh.pdf>`_
CPU       Intel(R) Quad Core(TM) i3 CPU 540 @ 3.07GHz
RAM       8GB
OS        Scientific Linux 5.3 i386 (32 bit)
Kernel    2.6.18-128.1.1.el5PAE
hostname  escsMaster
eth0      192.167.189.98
eth1      192.168.1.98
eth2      192.168.51.98
========= =====

.. _escsMaster_os_installation:

OS installation
~~~~~~~~~~~~~~~

EscsMaster is installed on a 500GB hard disk so partitioned::

   [root@escsMaster /]# fdisk -l

   Disk /dev/sda: 500.1 GB, 500107862016 bytes
   255 heads, 63 sectors/track, 60801 cylinders
   Units = cylinders of 16065 * 512 = 8225280 bytes

   Device Boot      Start         End      Blocks   Id  System
   /dev/sda1   *           1          13      104391   83  Linux
   /dev/sda2              14        2624    20972857+  83  Linux
   /dev/sda3            2625        3668     8385930   82  Linux swap / Solaris
   /dev/sda4            3669       60801   458920822+   5  Extended
   /dev/sda5            3669       55885   419433021   83  Linux
   /dev/sda6           55886       56538     5245191   83  Linux
   /dev/sda7           56539       57191     5245191   83  Linux
   /dev/sda8           57192       57583     3148708+  83  Linux
   /dev/sda9           57584       60801    25848553+  83  Linux

and::

   [root@escsMaster /]# mount

   /dev/sda2 on / type ext3 (rw)
   /dev/sda8 on /tmp type ext3 (rw)
   /dev/sda5 on /home type ext3 (rw)
   /dev/sda9 on /data type ext3 (rw)
   /dev/sda7 on /alma type ext3 (rw)
   /dev/sda1 on /boot type ext3 (rw)
   /dev/sda6 on /system type ext3 (rw)

During the installation process packages are installed directly from the
installation CD and we selected **KDE desktop environment**, **Developer tools**
including gcc, g++, make and kernel headers.
We removed every accessory package related to office and productivity work.

.. _escsMaster_additional_software:

Additional software
~~~~~~~~~~~~~~~~~~~

You can find additional software packages in the **/root/sofwtare** directory,
necessary packages are:

* e1000 NIS driver
* e1000e NIS driver
* cfitsio3100
* CCfits-2.1
* qt-X11-opensource 4.5.2

all these packages can be installed uncompressing the tar.gz packages and doing a
*configure, make, make install* loop::

    [root@escsMaster /root/software/]# tar xzvfp *.tar.gz
    [root@escsMaster /root/software/]# cd ccfitsio
    [root@escsMaster /root/software/ccfitsio]# ./configure --prefix=/usr/local
    [root@escsMaster /root/software/ccfitsio]# make
    [root@escsMaster /root/software/ccfitsio]# make install
    [root@escsMaster /root/software/]# cd /root/software/CCfits
    [root@escsMaster /root/software/CCfits]# ./configure --prefix=/usr/local
    [root@escsMaster /root/software/CCfits]# make
    [root@escsMaster /root/software/CCfits]# make install
    [root@escsMaster /root/software/]# cd /root/software/qt-4.5.2
    [root@escsMaster /root/software/qt-4.5.2]# ./configure --prefix=/usr/local/qt-4.5.2
    [root@escsMaster /root/software/qt-4.5.2]# make 
    [root@escsMaster /root/software/qt-4.5.2]# make install
    [root@escsMaster /root/software/qt-4.5.2]# cd /root/software/e1000-xxx/src
    [root@escsMaster /root/software/e1000-xxx/src]# make install
    [root@escsMaster /root/software/e1000-xxx/src]# modprobe e1000
    [root@escsMaster /root/software/e1000-xxx/src]# cd /root/software/e1000e-xxx/src
    [root@escsMaster /root/software/e1000e-xxx/src]# make install
    [root@escsMaster /root/software/e1000e-xxx/src]# modprobe e1000e
    
We than need to install the proper GPFS packages::

    [root@escsMaster /]# cd /root/software/gpfs/
    [root@escsMaster /root/software/gpfs/]# yum install kernel-devel kernel-headers
    [root@escsMaster /root/software/gpfs/]# rpm -vi gpfs.base-3.2.0.i386.rpm
    [root@escsMaster /root/software/gpfs/]# rpm -vi gpfs.docs-3.2.0.noarch.rpm
    [root@escsMaster /root/software/gpfs/]# rpm -vi gpfs.gpl-3.2.0.noarch.rpm
    [root@escsMaster /root/software/gpfs/]# rpm -vi gpfs.msg.en_US-3.2.0.noarch.rpm
    [root@escsMaster /root/software/gpfs/]# rpm -Uv gpfs.base-3.2.1-29.i386.update.rpm 
    [root@escsMaster /root/software/gpfs/]# rpm -Uv gpfs.gpl-3.2.1-29.noarch.rpm 
    [root@escsMaster /root/software/gpfs/]# rpm -Uv gpfs.msg.en_US-3.2.1-29.noarch.rpm 
    [root@escsMaster /root/software/gpfs/]# rpm -Uv gpfs.docs-3.2.1-29.noarch.rpm 
    [root@escsMaster /root/software/gpfs/]# rpm -vi gpfs.gui-3.2.1-29.i386.rpm
    [root@escsMaster /root/software/gpfs/]# echo "Red Hat Enterprise Linux Server release 5 (Tikanga)" > /etc/redhat-release
    [root@escsMaster /root/software/gpfs/]# cd /usr/lpp/mmfs/src
    [root@escsMaster /usr/lpp/mmfs/src/]# make Autoconfig
    [root@escsMaster /usr/lpp/mmfs/src/]# make World
    [root@escsMaster /usr/lpp/mmfs/src/]# make InstallImages

Users and Groups
~~~~~~~~~~~~~~~~

Users are defined on the escsMaster machine and then exported via yellow pages
to all other devices in the system, so it is particoularly important to pay
great attention during this process. 

Defined groups are:

========== === ==========
group name gid group role
========== === ==========
escs       335 owns ACS system files and processes
observers  336 final user accounts which run observation tools
========== === ==========

While necessary users are:

========= ====  ============== =========
user name uid   groups         user role
========= ====  ============== =========
manager   3060  observers,escs Run the ACS system
observer  3061  escs           Executes the observations
========= ====  ============== =========

Create those with::

    [root@escsMaster /]# groupadd -g 335 escs
    [root@escsMaster /]# groupadd -g 336 observers
    [root@escsMaster /]# useradd -g observers -G escs -n -u 3060 manager
    [root@escsMaster /]# useradd -g escs -n -u 3061 escs

You can find permissions and configuration files on the repository, at
http://svn.med.ira.inaf.it/repos/escsconf/escsMaster/ , user related files are::

    /etc/sudoers /etc/shutodwn.allow /etc/inittab /etc/pam.d/login
    /etc/pam.d/sshd /etc/security/access.conf /etc/skel/.bashrc
    /etc/skel/.bash_profile /etc/skel/.idl

With this files, users are prohibited from shutting down the machine or putting it
offline and reboot. SSH login is permitted only to root and observers group in
order to run observations.

Pam files are removed for preventing accidental shutdowns::

    [root@escsMaster /]# rm /etc/security/console.apps/poweroff
    [root@escsMaster /]# rm /etc/security/console.apps/halt
    [root@escsMaster /]# rm /etc/security/console.apps/reboot

Then we execute *gdmsetup* in order to disable login window actions and
configure the welcome message::

    [root@escsMaster /]# gdmsetup

NIS
~~~

Users and groups are exported via yellow pages service **nis**. Service
configuration is done using the files::

    /etc/ypserv.conf /etc/yp.conf /var/yp/securenets /var/yp/Makefile
    /etc/rc.local /etc/sysconfig/network /etc/idmapd.conf /etc/nsswitch.conf

For the service to run correctly it is important to configure all domain
names, hostnames and nisdomains to *escsMaster.med.ira.inaf.it*.
After having set the correct options in the configuration files we need to
rebuild the NIS database and permanently activate the service::

    [root@escsMaster /]# /usr/lib/yp/ypinit -m
    [root@escsMaster /]# service ypserv start
    [root@escsMaster /]# chkconfig ypserv on

.. _escsMaster_acs:

ACS
~~~

ACS software package must be configured to run on the escsMaster node. We first
extract necessary files and then configure users to load the correct environment
variables::

    [root@escsMaster /]# mkdir alma
    [root@escsMaster /]# chown manager:escs /alma
    [root@escsMaster /]# su - manager
    escsMaster manager:~ > cd /
    escsMaster manager:/ > tar xzpvf /home/manager/ACS.tar.gz
    escsMaster manager:/ > cd alma; chown manager:escs ACS-8.2/
    escsMaster manager:/alma > cp -r /alma/ACS-8.2/ACSSW/config/.acs $HOME
    escsMaster manager:/ > vi ~/.bashrc
    escsMaster manager:/ > vi ~/.bash_profile
    escsMaster manager:/ > su - observer
    escsMaster observer:/ > vi ~/.bashrc
    escsMaster observer:/ > vi ~/.bash_profile
    
We create the necessary directories and set the right permissions::

    [root@escsMaster /]# chown manager:escs system
    [root@escsMaster /]# chown manager:observers archive
    [root@escsMaster /]# mkdir /system/configuration
    [root@escsMaster /]# chown manager:escs /system/configuration
    [root@escsMaster /]# mkdir /system/docroot
    [root@escsMaster /]# chown manager:escs /system/docroot
    [root@escsMaster /]# mkdir /system/introot
    [root@escsMaster /]# chown manager:escs /system/introot
    [root@escsMaster /]# mkdir /system/sources
    [root@escsMaster /]# chown manager:escs /system/sources
    [root@escsMaster /]# mkdir /system/userbin
    [root@escsMaster /]# chown manager:observers /system/userbin
    [root@escsMaster /]# su - manager
    escsMaster manager:~ > cd /archive
    escsMaster manager:/archive > mkdir /archive/data
    escsMaster manager:/archive > mkdir /archive/schedules
    escsMaster manager:/archive > mkdir /archive/logs
    escsMaster manager:/archive > mkdir /archive/events
    escsMaster manager:/archive > mkdir /archive/extraData
    escsMaster manager:/archive > chmod 710 /archive/*

Then we can checkout and install the escs system::

   [root@escsMaster /]# chmod a+rw /data
   [root@escsMaster /]# cd data
   [root@escsMaster /data]# mkdir ACS
   [root@escsMaster /data]# chown manager:escs ACS
   [root@escsMaster /data]# su - manager
   escsMaster manager:/ > cd /data/ACS
   escsMaster manager:/data/ACS> svn co http://belzebu.oa-cagliari.inaf.it/repos/ACS .
   escsMaster manager:/data/ACS > cd /data/ACS/trunk/SystemMake #this will change to  ACS/tags/escs-0.3
   escsMaster manager:/data/ACS/trunk/SystemMake > make all
   escsMaster manager:/data/ACS/trunk/SystemMake > make cdb
   escsMaster manager:/data/ACS/trunk/SystemMake > escsInstall

And we can set acs to start at boot time::

   [root@escsMaster /]# vim /etc/rc.local
   su -l manager -c acsservicesdaemon &
   su -l manager -c acscontainerdaemon &

.. _escsMaster_temporary_data:

ACS Temporary Data
~~~~~~~~~~~~~~~~~~

ACS needs to store log informations for each process running inside the system.
this is true for every container, daemon, manager ecc... 
This files can be very large and sometimes they can flood the disk space
resulting in wrong ACS behaviors, so we decided to store these files into a
local directory on each machine::

    [manager@escsMaster ~] vim ~/.bashrc
    ...
    export ACS_TMP=/data/ACSTMP

And we create the necessary directory setting owner and group to the ones used
by ACS processes::

    [manager@escsMaster ~] cd /data
    [manager@escsMaster data/] mkdir ACSTMP
    [manager@escsMaster data/] chown manager:escs ACSTMP


.. _escsMaster_ntp:

NTP
~~~

Ntp service for system clock synchronization is configured via */etc/ntp.conf*
and */etc/ntp/ntpservers*
connecting to the station servers **192.167.189.69** , **labtf.med.ira.inaf.it** and **192.167.189.1**.
We also define a custom */root/bin/plot_loopstats* command::

    [root@escsMaster /]# mkdir /var/log/ntpstats
    [root@escsMaster /]# chown ntp:ntp /var/log/ntpstats
    [root@escsMaster /]# service ntpd start
    [root@escsMaster /]# chkconfig ntpd on
    [root@escsMaster /]# /root/bin/plot_loopstats

Firewall
~~~~~~~~

Iptables is configured so that it:

1. Accept connections from .189 .1 and .51 subnets on the specific interfaces
2. Act as NAT for the .51 subnet and routes incoming connection to the subnet
   for a selected number of machines based on ip-restriction.
3. Act as NAT for the .1 subnet routing all the connection from the .1 subnet to
   the outside::

    [root@escsMaster /]# less /root/bin/firewall
    ...
    iptables -A INPUT -i eth2 -s 192.168.51.0/24 -j ACCEPT
    iptables -A INPUT -i eth1 -s 192.168.1.0/24 -j ACCEPT
    ...
    iptables --table nat -A POSTROUTING -o eth0 -j MASQUERADE
    iptables --table nat -A POSTROUTING -o eth2 -j MASQUERADE
    ...
    iptables -A FORWARD -i eth1 -o eth0 -j ACCEPT
    iptables -A FORWARD -i eth0 -o eth1 -m state --state RELATED,ESTABLISHED -j ACCEPT
    ...
    iptables -A FORWARD -i eth2 -o eth0 -j ACCEPT
    iptables -A FORWARD -s 192.167.189.2 -o eth2 -j ACCEPT
    ...

The script will configure and start iptables service while saving its
configuration for sequent reboots. We also need to have ip forwarding enabled in
*/etc/sysctl.conf*
Complete file configuration can be found on the svn repo at
http://svn.med.ira.inaf.it/repos/escsconf/escsMaster/root/bin/firewall .

.. WARNING::
   Pay attention when configuring or managing the firewall as it now is the only
   access point to the .51 subnet. This means that temporarily stopping the
   firewall or disconnecting the eth2 interface will result in antenna failures
   in every software communicating with antenna apparatuses.

NFS
~~~

We have had some issues setting up the NFS file system exports. With this kernel
we did not manage to work with NFSv4 while everything is perfectly fine with
NFSv3 that is the one we are currently using. NFS is used in order to export
user home directories and ACS system installation while GPFS is used in order to
manage archived data.

First, we create the exported filesystem directories::

    [root@escsMaster /]# mkdir /exports
    [root@escsMaster /]# mkdir /exports/home
    [root@escsMaster /]# mkdir /exports/system
    [root@escsMaster /]# mkdir /exports/docroot
    [root@escsMaster /]# mkdir /exports/userbin

Then we bind the filesystem to the exported directories modifying the
*/etc/fstab* file adding the following lines::

   /home                  /exports/home           none    bind            0 0
   /system/introot        /exports/system         none    bind            0 0
   /system/docroot        /exports/docroot        none    bind            0 0
   /system/userbin        /exports/userbin        none    bind            0 0

Now the OS must be instructed to export the bound filesystems::

    [root@escsMaster /]# cat /etc/exports
    /exports         192.168.1.0/24(rw,fsid=0,insecure,no_subtree_check,sync,no_root_squash)
    /exports/home    192.168.1.0/24(rw,nohide,insecure,no_subtree_check,sync,no_root_squash)
    /exports/system  192.168.1.0/24(ro,nohide,insecure,no_subtree_check,sync,no_root_squash)
    /exports/docroot 192.168.1.0/24(ro,nohide,insecure,no_subtree_check,sync,no_root_squash)
    /exports/userbin 192.168.1.0/24(ro,nohide,insecure,no_subtree_check,sync,no_root_squash)
    [root@escsMaster /]# exportfs -rv

And we start the nfs server::
 
    [root@escsMaster /]# service nfs start
    [root@escsMaster /]# chkconf nfs on

DATA BACKUP
~~~~~~~~~~~

Data backup is realized on IRA-Bologna servers via rsync. we thus must authorize
IRA server to use rsync service on escsMaster which is the public access point
of the control system and enable rsync service on the machine itself::

    [root@escsMaster /]# vim /etc/rsyncd.conf
    [Area-Med-Arc]
        comment=archivio osservazioni single dish
        path=/archive/data
        read only = yes
        list = yes
        host allow =
        192.167.165.0/255.255.255.0
        uid = 3060
        gid = 335

    [root@escsMaster /]# vim /etc/xinetd.d/rsync
    # default: off
    # description: The rsync server is a good addition to an ftp server, as it \
    #       allows crc checksumming etc.
    service rsync
    {
        disable = no
        socket_type     = stream
        wait            = no
        user            = root
        server          = /usr/bin/rsync
        server_args     = --daemon
        log_on_failure  += USERID
    }

Service can be started and monitored using::

    [root@escsMaster /]# service xinted start|stop|status|restart
    

DATA REMOVAL
~~~~~~~~~~~~
Data are removed from /archive directory after 60 days and saved into
**/data/garbage** as .tar.gz archives. This is realized as a cronjob running
each night at 03:00am, and is logged in system log */var/log/messages* ::

    [root@escsMaster /]# vim /root/bin/purgeData
    #!/bin/bash
    #@author Andrea Orlati (a.orlati@ira.inaf.it)
    #@date 21/04/2011
    #modified Marco Bartolini (bartolini@ira.inaf.it)
    #@date 08/01/2014
    FILES=$(find /archive/data -mtime +60 -type f)
    if [[ $FILES ]]; then
        OUTPUT=$(date +"%Y_%m_%d")
        mkdir -p /data/garbage/$OUTPUT
        for j in $FILES
        do
            mv -f $j /data/garbage/$OUTPUT
        done
        cd /data/garbage
        gtar czpf $OUTPUT.tar.gz $OUTPUT/
        rm -rf $OUTPUT
        logger "backup data to file /data/garbage/$OUTPUT.tar.gz"
    else
        logger "no data for backup"
    fi

