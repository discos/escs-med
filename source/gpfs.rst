GPFS cluster setup
------------------

The GPFS cluster is managed by **escsMaster** node and involves **escsConsole**,
**escsCore1** and **escsCore2** nodes. Physical data storage is realized ona e-sata device attached
to escsCore1 node but data are accessible from every node in the cluster and
configuration and ACL permissions are managed by the master node. 

Package installation is described in the :ref:`escsMaster` document section
about :ref:`escsMaster_additional_software` and must be performed on every node
composing the cluster. Adding a node to the existing cluster must instead be
performed as described in :ref:`escsConsole GPFS <escsConsole_gpfs>`
configuration.

The first step is to make every machine root-accessible via ssh from the master
node using ssh public key authentication::

    [root@escsMaster /]# ssh-keygen -t rsa 
    [root@escsMaster /]# cp ~/.ssh/id_rsa.pub ~/.ssh/authorized_keys
    [root@escsMaster /]# scp /root/.ssh/authorized_keys escsCore1:/root/.ssh/
    [root@escsMaster /]# scp /root/.ssh/authorized_keys escsHost:/root/.ssh/
    
Now we can instruct the master node about how to manage the gpfs cluster::

    [root@escsMaster /]# mkdir /root/escsCluster
    [root@escsMaster /]# cd /root/escsCluster
    [root@escsMaster /root/escsCluster/]# vim cluster.def
    escsMaster
    escsCore1:quorum
    escsCore2
    [root@escsMaster /root/escsCluster/]# vim storage.cfg
    /dev/sdb/:escsCore1::dataAndMetadata::escsCore1Archive
    [root@escsMaster /root/escsCluster/]# mmcrcluster -N cluster-def \
          -p escsMaster -R /usr/bin/scp -r /usr/bin/ssh -C escsCluster.med.ira.inaf.it -A
    [root@escsMaster /root/escsCluster/]# mmstartup -a
    [root@escsMaster /root/escsCluster/]# mmgetstate -a
    [root@escsMaster /root/escsCluster/]# mmcrnsd -F storage.cfg -v no
    [root@escsMaster /root/escsCluster/]# mmcrfs /archive escsArchive \
          -F storage.cfg -A yes -B 64K -m1 -n 10 -r 1 -R 2 -v no

Now **gpfs** should be up and running and the file-systems has been created on
the external storage attached to escsCore1.

