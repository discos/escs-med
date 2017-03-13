.. _roachserver:

roach-server-parabola
---------------------

This machine is used to perform roach setup via NFS and UBOOT using the TFTP
server. It also controls the roach via scripts and the Valon synthetizer attached
to the roach board as a clock source. 

The machine main configuration parameters are the following: 

========= =====
PARAMETER VALUE
========= =====
hardware  DELL Poweredge
CPU       Intel(R) Xeon(R) CPU E3-1220 v3 @ 3.10GHz 
RAM       4GB
OS        CentOS Linux release 7.3.1611 (Core)
Kernel    3.10.0-514.10.2.el7.x86_64 #1 SMP
hostname  roach-server-parabola
em1       192.168.1.10
em2       192.168.52.10
p1p1      192.168.10.10 (10Gbe link to storage)
========= =====

OS installation
~~~~~~~~~~~~~~~


Additional software
~~~~~~~~~~~~~~~~~~~

See `med wiki <wiki.med.ira.inaf.it/nfs:ubuntu>`_ for installation of additional software 
packages necessary for roach boards setup and control.


