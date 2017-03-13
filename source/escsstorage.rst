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

Lustre server has been installed, and software RPMs are available in */root/software/lustre* . 
Notes about the installation procedure: 

.. code:: bash 

  #il RAID areca esegue operazioni in background se alternativamente:
  #        1) si e' in interfaccia da BIOS
  #        2) sul sistema operativo e' caricato il driver del RAID (http://www.areca.com.tw/support/s_linux/linux.htm)
  # (http://faq.areca.com.tw/index.php?action=artikel&cat=8&id=487&artlang=en)

  # da https://downloads.hpdd.intel.com/public/e2fsprogs/1.42.13.wc5/el7/RPMS/x86_64/

  rpm -ivh --force libcom_err-1.42.13.wc5-7.el7.x86_64.rpm libss-1.42.13.wc5-7.el7.x86_64.rpm 
  rpm -ivh --force e2fsprogs-libs-1.42.13.wc5-7.el7.x86_64.rpm 
  rpm -ivh --force e2fsprogs-1.42.13.wc5-7.el7.x86_64.rpm

  # da https://downloads.hpdd.intel.com/public/lustre/latest-release/el7/server/RPMS/x86_64/:

  rpm -ivh kernel-3.10.0-327.3.1.el7_lustre.x86_64.rpm
  rpm -ivh lustre-modules-2.8.0-3.10.0_327.3.1.el7_lustre.x86_64.x86_64.rpm 

  rpm -iv lustre-osd-ldiskfs-mount-2.8.0-3.10.0_327.3.1.el7_lustre.x86_64.x86_64.rpm 
  rpm -iv lustre-osd-ldiskfs-2.8.0-3.10.0_327.3.1.el7_lustre.x86_64.x86_64.rpm 


  # va bloccato il boot sul kernel lustre:
  # per vedere le entry
  awk -F\' '$1=="menuentry " {print i++ " : " $2}' /etc/grub2.cfg
  # editare GRUB_DEFAULT
  nano /etc/default/grub


  e2label /dev/md126p2 mdt
  mkfs.lustre --reformat --fsname=escs --mgs --mdt --index=0 /dev/disk/by-label/mdt


