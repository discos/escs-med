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
OS        Ubuntu Server 16.04
Kernel    4.4.0-31-generic #50-Ubuntu SMP
hostname  roach-server-parabola
eno1      192.168.1.10
eno2      192.168.52.10
========= =====

OS installation
~~~~~~~~~~~~~~~


Additional software
~~~~~~~~~~~~~~~~~~~

See `med wiki <wiki.med.ira.inaf.it/nfs:ubuntu>`_ for installation of additional software 
packages necessary for roach boards setup and control.


