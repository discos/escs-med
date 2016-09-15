.. _escsHost:

escsHost
--------

This machine is mainly used as a virtualization platform, supplying the
accessory machines **escsCore2** and **escsRemote** via VirtualBox. In the
future this node is intended to run also an http server for documentation and
application deployment.

Configuration files can be found on the svn repo at::

    http://svn.med.ira.inaf.it/repos/escsconf/escsHost

The machine main configuration parameters are the following: 

========= =====
PARAMETER VALUE
========= =====
hardware  `IBM System x3250 M4 <http://www-03.ibm.com/systems/x/hardware/rack/x3250m4/>`_
CPU       Intel(R) Xeon(R) CPU E3-1220 V2 @ 3.10GHz (Quad)
RAM       8GB
OS        Scientific Linux 5.3 i386 (32 bit)
Kernel    2.6.18-128.1.1.el5PAE
hostname  escsHost
eth0      192.167.189.101
eth1      192.168.1.101
eth2      192.168.51.101
========= =====

OS installation
~~~~~~~~~~~~~~~

EscsHost is installed on a 500GB hard disk so partitioned::

    [root@escsHost /]# fdisk -l

    Disk /dev/sda: 500.1 GB, 500107862016 bytes
    255 heads, 63 sectors/track, 60801 cylinders
    Units = cylinders of 16065 * 512 = 8225280 bytes

       Device Boot      Start         End      Blocks   Id  System
       /dev/sda1   *           1          13      104391   83  Linux
       /dev/sda2              14        2690    21503002+  83  Linux
       /dev/sda3            2691        3710     8193150   82  Linux swap / Solaris
       /dev/sda4            3711       60801   458583457+   5  Extended
       /dev/sda5            3711        6387    21502971   83  Linux
       /dev/sda6            6388        7024     5116671   83  Linux
       /dev/sda7            7025       60801   431963721   83  Linux

and::

    [root@escsHost /]# mount
    /dev/sda2 on / type ext3 (rw)
    /dev/sda7 on /data type ext3 (rw)
    /dev/sda6 on /tmp type ext3 (rw)
    /dev/sda5 on /var type ext3 (rw)
    /dev/sda1 on /boot type ext3 (rw)

During the installation process packages are installed directly from the
installation CD and we selected **KDE desktop environment**, **Developer tools**
including gcc, g++, make and kernel headers.
We removed every accessory package related to office and productivity work.

Additional software
~~~~~~~~~~~~~~~~~~~

Additional software packages are installed as described in
:ref:`escsCore1_additional_sofware` and the necessary packages are:

* gpfs
* e1000
* e1000e

Apart from these packages we need to install **VirtualBox**, we can find the
.rpm package of the installed version in /root/software directory on the svn
repo but any version should be good.

VirtualBox Services
~~~~~~~~~~~~~~~~~~~

Two virtual machines named **escsRemote** and **escsCore2** are configured for
starting up at boot time, we report escsCore2 init script as an example::

    [root@escsHost /]# less /etc/init.d/vbox_escsCore2
    #!/bin/sh
    # vbox_escsCore2   Startup script for VirtualBox Virtual Machines
    #
    # chkconfig: 345 98 02
    # description: Manages escsCore2 VirtualBox machine
    # processname: vbox_escsCore2
    #
    # pidfile: /var/run/vboxcontrol/vbox_escsCore2.pid
    #

    # Source function library.
    if [ -f /etc/init.d/functions ] ; then
    . /etc/init.d/functions
    elif [ -f /etc/rc.d/init.d/functions ] ; then
    . /etc/rc.d/init.d/functions
    else
    exit 1
    fi

    ########################################################################
    # CONFIGURATION

    NAME="escsRemote"
    VBOXMANAGE="VBoxManage -nologo"

    ########################################################################
    # FUNCTIONS

    log_action_msg() {
    echo $1
    }

    wait_for_closing() {
    ISRUNNING=`$VBOXMANAGE showvminfo $NAME |grep "^State:\s*running" > /dev/null &&
    echo "RUNNING"`
    if [ ! -z "$ISRUNNING" ]; then
    sleep 8
    wait_for_closing
    fi
    }

    ########################################################################
    # RUN
    case "$1" in
    start)
    log_action_msg "Starting VM: $NAME ..."
    $VBOXMANAGE startvm $NAME -type headless
    RETVAL=$?
    touch /var/lock/subsys/$NAME
    ;;
    stop)
    log_action_msg "Shutting down VM: $VM ..."
    $VBOXMANAGE controlvm $NAME acpipowerbutton
    rm -f /var/lock/subsys/$NAME
    wait_for_closing
    ;;
    status)
    $VBOXMANAGE showvminfo $NAME | grep "State"
    ;;
    *)
    echo "Usage: $0 {start|stop|status}"
    exit 3
    esac
    exit 0

NTP and firewall
~~~~~~~~~~~~~~~~

For NTP configuration see :ref:`escsCore1_ntp`.

For firewall configuration see :ref:`escsCore1_firewall`.
