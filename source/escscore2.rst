.. _escsCore2:

escsCore2
----------

This machine runs some particular ACS container.

Configuration files can be found on the svn repo at::

    http://svn.med.ira.inaf.it/repos/escsconf/escsCore2

The machine main configuration parameters are the following: 

========= =====
PARAMETER VALUE
========= =====
hardware  Virtual Machine on VirtualBox running on :ref:`escsHost`
Kernel    2.6.18-128.1.1.el5
hostname  escsRemote
eth0      192.168.1.105
eth1      192.168.51.105
========= =====

.. _escsCore2_os_installation:

OS installation
~~~~~~~~~~~~~~~

EscsRemote is installed and configured exactly as :ref:`escsCore1` in most of its
services:

* users and groups are imported via **NIS yellow pages** from :ref:`escsMaster` as described in :ref:`escsCore1_nis` 
* additional software is installed as in :ref:`escsMaster_additional_software`
* user homes and **ACS** installation are imported via **NFS** from :ref:`escsMaster` as in
  :ref:`escsCore1_nfs_and_automount`
* **NTP** configuration is done as in :ref:`escsCore1_ntp`

