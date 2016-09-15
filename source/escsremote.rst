.. _escsRemote:

escsRemote
----------

This machines is mainly used to give users remote access to the system via the
VNC server it exposes.
configuration files can be found on the svn repo at::

    http://svn.med.ira.inaf.it/repos/escsconf/escsRemote

The machine main configuration parameters are the following: 

========= =====
PARAMETER VALUE
========= =====
hardware  Virtual Machine on VirtualBox running on :ref:`escsHost`
Kernel    2.6.18-128.1.1.el5
hostname  escsRemote
eth0      192.167.189.57
eth1      192.168.1.57
========= =====

.. _escsRemote_os_installation:

OS installation
~~~~~~~~~~~~~~~

EscsRemote is installed and configured exactly as :ref:`escsCore1` in most of its
services:

* users and groups are imported via **NIS yellow pages** from :ref:`escsMaster` as described in :ref:`escsCore1_nis` 
* user homes are imported via **NFS** from :ref:`escsMaster` as in
  :ref:`escsCore1_nfs_and_automount`
* **NTP** configuration is done as in :ref:`escsCore1_ntp`


.. _escsRemote_vnc:

VNC Server
~~~~~~~~~~

Two users are defined by default for accessing remptely this machine via VNC:
manager and observer.
The configuration file is defined as::

    [root@escsRemote /]# cat /etc/sysconfig/vncservers 
    ...
    VNCSERVERS="1:manager 2:observer"
    VNCSERVERARGS[2]="-geometry 1440x900 DisconnectClients=0"

    VNCSERVERARGS[1]="-geometry 1440x900 DisconnectClients=0 NeverShared=1"

We use system service to run the VNC server and it can be accessed using a VNC
viewer on ports 1 and 2 respectively for users manager and observer. Firewall is
configured accordingly.

