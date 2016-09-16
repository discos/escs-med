.. _escsStorage:

escsStorage
-----------

This machine is the high performarnce storage attached to the 10GBE network
and sharing tis filesystem via LUSTRE services to others ESCS nodes for experiments
data storage.

The machine main configuration parameters are the following: 

========= =====
PARAMETER VALUE
========= =====
hardware  Supermicro with ARECA raid controller
CPU       Intel(R) Quad Core(TM) i3 CPU 540 @ 3.07GHz
RAM       4GB
OS        Scientific Linux release 6.5 (Carbon) 
Kernel    2.6.32-431.el6.x86_64 #1 SMP
hostname  escsStorage
eth0      192.168.1.9
eth1(10G)      192.168.10.9
raid      192.168.1.8
========= =====

OS installation
~~~~~~~~~~~~~~~

Additional software
~~~~~~~~~~~~~~~~~~~

