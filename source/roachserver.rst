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

Following the list of compatible versions of python packages to be installed for a completely working
environment, do not proceed with latest versions as you will get all sort of weird errors!

.. code-block:: bash

        # pip freeze
        Automat==0.5.0
        ProxyTypes==0.9
        Twisted==17.1.0
        ValonSynth==0.0.0
        argparse==1.4.0
        astropy==1.3
        attrs==16.3.0
        backports-abc==0.5
        backports.ssl-match-hostname==3.5.0.1
        certifi==2017.01.23
        constantly==15.1.0
        construct==2.5.5
        corr==0.7.3
        cycler==0.10.0
        discosbackend==1.2
        functools32==3.2.3-2
        futures==3.0.5
        h5py==2.6.0
        incremental==16.10.1
        iniparse==0.4
        ipython==1.0.0
        katcp==0.3.5
        linecache2==1.0.0
        matplotlib==2.0.0
        mock==1.0.1
        nose==1.3.7
        numpy==1.12.0
        ply==3.10
        pyparsing==2.2.0
        pyserial==3.3
        python-dateutil==2.6.0
        pytz==2016.10
        singledispatch==3.4.0.3
        six==1.10.0
        spead==0.5.1
        subprocess32==3.2.7
        tornado==4.4.2
        traceback2==1.4.0
        unittest2==1.1.0
        wsgiref==0.1.2
        zope.interface==4.3.3


Configuration
~~~~~~~~~~~~~

All the roach software package can be found in the directory: 

.. code::

   /root/roach_discos_backend

For this to work we had to define symbolic links pointing to the lustre filesystem at root level with different names, namely: 

.. code:: bash

   ln -s /storage /raid
   ln -s /storage /locallustre


Also, the environment variable PFILES has been defined in .bashrc to point to /root/roach_discos_backend/pfiles, for completeness we provide the whole .bashrc file

.. code:: bash

   export WORKON_HOME=~/.envs
   export PFILES=/root/roach_backend_discos/pfiles
   source /usr/bin/virtualenvwrapper.sh
   workon roach







