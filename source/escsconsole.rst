.. _escsConsole:

escsConsole
-----------

The console machine is directly connected to a monitor in the control room and
is intended to be used by observers in order to run auxiliary observation tools
such as **quicklook** the **Imager** and the **ScheduleCreator**.
Configuration files can be found on the svn repo at::

    http://svn.med.ira.inaf.it/repos/escsconf/escsConsole

The machine main configuration parameters are the following: 

========= =====
PARAMETER VALUE
========= =====
hardware  ERack E4 computer engineering
Kernel    2.6.18-128.1.1.el5 x86_64
hostname  escsConsole
eth0      192.168.1.54
eth1      192.167.189.54
========= =====

.. _escsConsole_os_installation:

OS installation
~~~~~~~~~~~~~~~

IDL
~~~

This is the only machine running **IDL 8.2**  in this setup. IDL is installed via
original CD-ROM available at Medicina station and install location is the one
suggested by default::

    /usr/local/exelis

We then created a directory for IDL third party libraries in:: 

    /usr/local/idllib

and installed there **Coyote** and **astron** libraries. You can find original
versions of the libraries at the following links:

    * http://www.idlcoyote.com/programs/zip_files/coyoteprograms.zip
    * http://idlastro.gsfc.nasa.gov/ftp/astron.tar.gz

Remember to add th edirectory to the IDL path and to include all subdirectories
recursively, the easiest way to do that is via idlde->window->preferences.


SDI and Quicklook
~~~~~~~~~~~~~~~~~

The *Single dish imager* can be downloaded from the SRT scicom wiki page at
http://scicomsrt.pbworks.com/w/page/54294508/IMAGING%20ANALYSIS and has been
extracted to the same location in::

    /usr/local/idllib/SDI

SDI has **ds9** as a dependency, which can be download from
http://hea-www.harvard.edu/RD/ds9/site/Download.html or you can find in
/root/software/ directory. Extract ds9 and copy it in::

    /usr/local/bin

You can find the **fits_look.pro** idl procedure in /root/software directory. 
The procedure has been copied in */usr/local/idllib/* as the rest of custom idl
programs.

Python
~~~~~~

EscsConsole is provided with Python2.7 installed. This allow users and
astronomers to use and develop data analysis programs based on more recent
technologies then the default Python2.4. 
Installing is easy, just remember to do a **make altinstall** instead of the
usual make install::

    [root@escsConsole /root/software/] wget http://www.python.org/ftp/python/2.7.6/Python-2.7.6.tgz
    [root@escsConsole /root/software/] tar xzvf Python-2.7.6.tgz
    [root@escsConsole /root/software/] cd Python-2.7.6
    [root@escsConsole /root/software/] ./configure
    [root@escsConsole /root/software/] make
    [root@escsConsole /root/software/] make altinstall

After this step you can invoke python2.7 from command line. Using this python
version Download and install the latest version of python-setuptools from
https://pypi.python.org/pypi/setuptools and then::
    
    [root@escsConsole /root/software/] easy_install-2.7 pip
    [root@escsConsole /root/software/] pip-2.7 install ipython numpy astropy

.. WARNING::
   Installing with normal *make install* will override system python
   installation causing serious problems. Beware!

.. _escsConsole_gpfs:

GPFS
~~~~

Installing GPFS on escsConsole requires some additional work due to the 64 bit
OS, this leads to different dependencies and configuration parameters::
    
    [root@escsConsole /root/software/gpfs/]# yum install kernel-devel kernel-headers
    [root@escsConsole ~]# cd /root/software/gpfs
    [root@escsConsole gpfs]# yum install compat-libstdc++-33-3.2.3-61.i386
    [root@escsConsole gpfs]# yum install compat-libstdc++-33.x86_64
    [root@escsConsole gpfs]# rpm -vi gpfs.base-3.2.0-0.x86_64.rpm 
    Preparing packages for installation...
    gpfs.base-3.2.0-0
    [root@escsConsole gpfs]# rpm -vi gpfs.docs-3.2.0-0.noarch.rpm 
    Preparing packages for installation...
    gpfs.docs-3.2.0-0
    [root@escsConsole gpfs]# rpm -vi gpfs.gpl-3.2.0-0.noarch.rpm 
    Preparing packages for installation...
    gpfs.gpl-3.2.0-0
    [root@escsConsole gpfs]# rpm -vi gpfs.msg.en_US-3.2.0-0.noarch.rpm 
    Preparing packages for installation...
    gpfs.msg.en_US-3.2.0-0
    [root@escsConsole gpfs]# rpm -Uv gpfs.base-3.2.1-26.x86_64.update.rpm 
    Preparing packages for installation...
    gpfs.base-3.2.1-26
    [root@escsConsole gpfs]# rpm -Uv gpfs.gpl-3.2.1-26.noarch.rpm 
    Preparing packages for installation...
    gpfs.gpl-3.2.1-26
    [root@escsConsole gpfs]# rpm -Uv gpfs.msg.en_US-3.2.1-26.noarch.rpm 
    Preparing packages for installation...
    gpfs.msg.en_US-3.2.1-26
    [root@escsConsole gpfs]# rpm -Uv gpfs.docs-3.2.1-26.noarch.rpm 
    Preparing packages for installation...
    gpfs.docs-3.2.1-26
    [root@escsConsole gpfs]# rpm -vi gpfs.gui-3.2.1-26.x86_64.rpm 
    Preparing packages for installation...
    gpfs.gui-3.2.1-26
    You may start the GPFS GUI now by typing : /etc/init.d/gpfsgui start
    Alternatively, the GPFS GUI will start on reboot.

    [root@escsConsole gpfs]# cd /usr/lpp/mmfs/src/config
    [root@escsConsole config]# cp site.mrc.proto site.mrc
    [root@escsConsole config]# vim site.mrc

    //SET THE FOLLOWING PARAMETERS IN THE FILE
    #define GPFS_ARCH_X86_64
    LINUX_DISTRIBUTION = REDHAT_AS_LINUX
    #define LINUX_KERNEL_VERSION 2061899
    KERNEL_HEADER_DIR = /usr/src/kernels/2.6.18-371.1.2.el5-x86_64/include
    KERNEL_BUILD_DIR = /usr/src/kernels/2.6.18-371.1.2.el5-x86_64

    [root@escsConsole config]# cd ..
    [root@escsConsole src]# make World
    [root@escsConsole src]# make InstallImages

We added this node to the gpfs cluster after the cluster itself had been created
and initialized, so we follow here with the necessary instructions used to add a
node to the escsCluster. So login into the master node and::

    [root@escsMaster escsCluster]# mmaddnode escsConsole
    Fri Oct 25 10:44:08 CEST 2013: mmaddnode: Processing node
    escsConsole.med.ira.inaf.it
    The authenticity of host 'escsconsole.med.ira.inaf.it (192.168.1.45)' can't be
    established.
    RSA key fingerprint is 74:e5:89:41:4f:8f:ad:86:3c:e7:e1:f5:98:b6:77:4e.
    Are you sure you want to continue connecting (yes/no)? yes
    mmaddnode: Command successfully completed
    mmaddnode: Propagating the cluster configuration data to all
      affected nodes.  This is an asynchronous process.

