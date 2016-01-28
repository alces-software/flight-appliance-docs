.. _working-with-gridware-applications:

Working with Gridware Applications
==================================

How to work with the Alces Gridware utility to install and manage Linux applications, compilers and libraries together with the Linux *modules* environment.

Prerequisites
-------------

-  `SGE compute node environment deployed <cfn-deploy-sge-spot-cluster>`_
-  At least 1 compute node deployed
-  *Optional*: Application Manager Appliance deployed

Overview
--------
Alces Gridware provides users with a simple method to install many applications, libraries and compilers - made available for use with the Linux *modules* environment. The Alces Gridware tool provides well-known instructions for installing each of the Gridware package.

Using the *modules* environment with the Alces Gridware utility enables you to install multiple versions of applications if required, traditionally this is not possible without advanced configuration.

Gridware can install be installed from any host within your environment, in the case more powerful compute hosts are available - you may wish to compile applications on compute hosts. The default configuration shares Gridware applications across the ``/opt/gridware`` shared NFS directory from either the cluster login node or the Application Manager appliance, depending on your installation type.

Viewing available Gridware
--------------------------
From any node within your environment, the ``alces gridware list`` will provide a list of available applications, compilers and libraries:

.. code:: bash

    [alces@login1(cluster) ~]$ alces gridware list
    base/apps/abacas/1.3.1                   base/apps/abra/0.96
    base/apps/abyss/1.2.6                    base/apps/abyss/1.5.1
    base/apps/abyss/1.5.2                    base/apps/ambertools/12.7
    base/apps/amos/3.0.0                     base/apps/amos/3.1.0
    base/apps/anges/1.01                     base/apps/ants/1.9.1
    base/apps/aragorn/1.2.36                 base/apps/artemis/13.0.0
    base/apps/artemis/14.0.17                base/apps/artfastqgen/20121120
    base/apps/ase/3.6.0                      base/apps/atlassnp2/1.2
    base/apps/atlassnp2/1.4.3                base/apps/augustus/2.6.1
    ...

There are many hundreds of Gridware packages available - should you wish to search for a specific package, combine the ``alces gridware list`` function with ``grep``, for example - to find the available versions of *Python*:

.. code:: bash

    [alces@login1(cluster) ~]$ alces gridware list python*
    base/apps/python/2.7.3   base/apps/python/2.7.5   base/apps/python/2.7.8   base/apps/python3/3.2.3
    base/apps/python3/3.3.3  base/apps/python3/3.4.0  base/apps/python3/3.4.3

Viewing information about Gridware packages
-------------------------------------------
The ``alces gridware info <package>`` tool provides information, and optional (or sometimes compulsary) installation arguments. For example, to view information about the ``python/2.7.8`` package:

.. code:: bash

    [alces@login1(cluster) ~]$ alces gridware info python/2.7.8
    base/apps/python/2.7.8:
      Summary
        A remarkably powerful dynamic programming language

      Version
        2.7.8

      Compatible compilers (--compiler)
        gcc

      Description
        Python is a remarkably powerful dynamic programming language that is
        used in a wide variety of application domains. Python is often
        compared to Tcl, Perl, Ruby, Scheme or Java. Some of its key
        distinguishing features include:

          * very clear, readable syntax
          * strong introspection capabilities
          * intuitive object orientation
          * natural expression of procedural code
          * full modularity, supporting hierarchical packages
          * exception-based error handling
          * very high level dynamic data types
          * extensive standard libraries and third party modules for
            virtually every task
          * extensions and modules easily written in C, C++ (or Java for
            Jython, or .NET languages for IronPython)
          * embeddable within applications as a scripting interface

Installing Gridware packages
----------------------------
Gridware packages can be installed from any node in your environment, enabling you to use potentially more powerful dedicated compute hosts to install and compile Gridware packages.

To install a Gridware application, use the ``alces gridware install`` command - for example to install the Python 2.7.8 package:

.. code:: bash

    [alces@login1(cluster) ~]$ alces gridware install python/2.7.8
    Installing base/apps/python/2.7.8

     > Preparing package sources
            Download --> Python-2.7.8.tgz ... OK
              Verify --> Python-2.7.8.tgz ... OK

     > Preparing for installation
               Mkdir ... OK (/var/cache/gridware/src/apps/python/2.7.8/gcc-4.8.5)
             Extract ... OK

     > Proceeding with installation
             Compile ... OK
               Mkdir ... OK (/opt/gridware/depots/1664fc8e/el7/pkg/apps/python/2.7.8/gcc-4.8.5)
             Install ... OK
              Module ... OK

    Installation complete.

Verifying package installations
*******************************
Once a package has installed, you can check its availability using the ``alces module`` utility, e.g. to list available Gridware applications:

.. code:: bash

    [alces@login1(cluster) ~]$ alces module avail
    ---  /opt/gridware/local/el7/etc/modules  ---
      apps/python/2.7.8/gcc-4.8.5
      compilers/gcc/system
      libs/gcc/system
      null
    ---  /opt/clusterware/etc/modules  ---
      services/gridscheduler

We can see the ``python/2.7.8`` package is now available for use after previous installation. To load the application, use the ``alces module load <package>`` command, e.g.:

.. code:: bash

    [alces@login1(cluster) ~]$ alces module load apps/python/2.7.8
    apps/python/2.7.8/gcc-4.8.5
     | -- libs/gcc/system
     |    * --> OK
     |
     OK
     [alces@login1(cluster) ~]$ echo $PYTHONBIN
     /opt/gridware/depots/1664fc8e/el7/pkg/apps/python/2.7.8/gcc-4.8.5/bin
