.. _working-with-gridware-depots:

Working with Gridware Depots 
============================

The Alces Gridware utility includes a feature called *depots*. Depots enable sets of applications, compilers and accelerated libraries to be packaged up - ready for use and easy installation on Alces Compute environments.

Many Gridware depots are publicly available for usage - a few examples include: 

-  ``benchmark`` - a set of popular benchmarking applications and utilities including CPU, disk and network performance tests
-  ``bio`` - a set of popular bioinformatics tools, libraries and compilers 
-  ``chem`` - a set of popular chemistry applications and tools

Optionally - you can create your own Gridware Depots for later use.

Prerequisites
-------------

-  Alces Flight Compute environment deployed

Depot Usage
-----------
Using ``alces gridware depot --help`` from your cluster login node will provide a usage page, however the basic Gridware Depot functions available include: 

-  ``install`` - install a Gridware Depot from the list of available Depots
-  ``enable`` - enable the previously depot you previously used ``fetch`` on for global usage
-  ``disable`` - mark a depot as unavailable for use

Fetching a Depot
----------------
From the login node of your environment, as an administrator user - fetch one of the publicly available Gridware Depots - such as the ``benchmark`` depot: 

.. code:: bash

    [alces@login1(flight-cluster) ~]$ alces gridware depot install benchmark
    Installing depot: benchmark
    
     > Initializing depot: benchmark
          Initialize ... OK
    
    Importing mpi-openmpi-1.8.5-el7.tar.gz
    
     > Fetching archive
            Download ... OK
    
     > Preparing import
             Extract ... OK
              Verify ... OK
    
     > Processing mpi/openmpi/1.8.5/gcc-4.8.5
           Preparing ... OK
           Importing ... OK
         Permissions ... OK
    
     > Finalizing import
              Update ... OK
        Dependencies ... OK
    
    Importing libs-atlas-3.10.2-el7.tar.gz
    
     > Fetching archive
            Download ... OK
    
     > Preparing import
             Extract ... OK
              Verify ... OK
    
     > Processing libs/atlas/3.10.2/gcc-4.8.5
           Preparing ... OK
           Importing ... OK
         Permissions ... OK
    
     > Finalizing import
              Update ... OK
        Dependencies ... OK
    
    Importing apps-memtester-4.3.0-el7.tar.gz
    
     > Fetching archive
            Download ... OK
    
     > Preparing import
             Extract ... OK
              Verify ... OK
    
     > Processing apps/memtester/4.3.0/gcc-4.8.5
           Preparing ... OK
           Importing ... OK
         Permissions ... OK
    
     > Finalizing import
              Update ... OK
        Dependencies ... OK
    
    Importing apps-iozone-3.420-el7.tar.gz
    
     > Fetching archive
            Download ... OK
    
     > Preparing import
             Extract ... OK
              Verify ... OK
    
     > Processing apps/iozone/3.420/gcc-4.8.5
           Preparing ... OK
           Importing ... OK
         Permissions ... OK
    
     > Finalizing import
              Update ... OK
        Dependencies ... OK
    
    Importing apps-imb-4.0-el7.tar.gz
    
     > Fetching archive
            Download ... OK
    
     > Preparing import
             Extract ... OK
              Verify ... OK
    
     > Processing apps/imb/4.0/gcc-4.8.5+openmpi-1.8.5
           Preparing ... OK
           Importing ... OK
         Permissions ... OK
    
     > Finalizing import
              Update ... OK
        Dependencies ... OK
    
    Importing apps-hpl-2.1-el7.tar.gz
    
     > Fetching archive
            Download ... OK
    
     > Preparing import
             Extract ... OK
              Verify ... OK
    
     > Processing apps/hpl/2.1/gcc-4.8.5+openmpi-1.8.5+atlas-3.10.2
           Preparing ... OK
           Importing ... OK
         Permissions ... OK
    
     > Finalizing import
              Update ... OK
        Dependencies ... OK
    
Once the Depot has been installed, it will be immediately available for use throughout your environment: 

.. code:: bash

    [alces@login1(flight-cluster) ~]$ module avail
    ---  /opt/gridware/benchmark/el7/etc/modules  ---
      apps/hpl/2.1/gcc-4.8.5+openmpi-1.8.5+atlas-3.10.2
      apps/imb/4.0/gcc-4.8.5+openmpi-1.8.5
      apps/iozone/3.420/gcc-4.8.5
      apps/memtester/4.3.0/gcc-4.8.5
      compilers/gcc/system
      libs/atlas/3.10.2/gcc-4.8.5
      libs/gcc/system
      mpi/openmpi/1.8.5/gcc-4.8.5
      null
    ---  /opt/gridware/local/el7/etc/modules  ---
      compilers/gcc/system
      libs/gcc/system
      null
    ---  /opt/clusterware/etc/modules  ---
      null
      services/aws
      services/gridscheduler
