Machines
--------

The installation is composed of 4 physical machines and 2 virtual machines for a
total of 6 setups which will be entirely documented. Here follows a brief
description: 

    * :ref:`escsMaster`: the node running ACS daemons as well as common services
    * :ref:`escsCore1`: the node running ESCS containers and GPFS storage
    * :ref:`escsHost`: the node running virtual machines for user access, and the web
      server
    * **escsCore2**: unused at the moment, spare room for more containers
    * :ref:`escsRemote`: the node running VNC servers for accessing the system
    * :ref:`escsConsole`: the node providing access to the system by operators for
      accessing the system, will have **IDL** and **QuickLook** tools installed
    * :ref:`roachserver`: the node providing services for roach control
    * :ref:`escsStorage`: the high performance storage attached to the 10GBE network


.. toctree::
   escsmaster
   escscore1
   escshost
   escsremote
   escscore2
   escsconsole
   roachserver
   escsstorage


