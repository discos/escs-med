ESCS 0.3 CONFIGURATION AND SETUP
================================

As ESCS development goes, on a new release, namely **escs 0.3**, has been
installed at the Medicina Radiotelescope. 
This internal report describes how the escs version is configured in terms
of hardware and software, enabling future users to install a specular system.

ESCS Version
------------

The installed version of the control system is **escs-0.3** and can be downloaded
from belzebu svn repository at:: 

    svn+ssh://belzebu.oa-cagliari.inaf.it/repos/ACS/tags/escs-0.3

Subversion repository
---------------------

The configuration of each single machine composing the system is fully detailed
in its own chapter, as well as the network topology. Every peculiar configuration file is 
stored in a private subversion repository hosted on:: 

    http://svn.med.ira.inaf.it/repos/escsconf/

and is referenced throughout this document without being fully copied on paper. You can ask the
authors for repository credentials whenever needed.
Each subfolder is mapped to the **/ (root)** directory of the machine naming the
folder i.e.::

    http://svn.med.ira.inaf.it/repos/escsconf/escsMaster/

maps the **/** directory of the **escsMaster** node. As a result of this
operation you can checkout configuration files directly on the escs nodes and
vice versa.

Documentation on Github
-----------------------

Documentation of the whole installation configuration in a human readable form is hosted on github 
under the DISCOS organization page at::
    
    http://github.com/discos/escs-med/

..    Machines
    ========

    .. include:: machines.rst

    Configurations
    ==============

    .. include:: escsmaster.rst
    .. include:: escscore1.rst
    .. include:: escshost.rst
    .. include:: gpfs.rst
    .. include:: cabling.rst

    .. _svnrepo: http://svn.med.ira.inaf.it/repos/escsconf
    .. _our svn server: svnrepo_
