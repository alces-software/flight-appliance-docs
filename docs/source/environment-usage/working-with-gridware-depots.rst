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

-  `SGE compute node environment deployed <cfn-deploy-sge-spot-cluster>`__
-  *Optional*: Application Manager Appliance deployed

Depot Usage
-----------
Using ``alces gridware depot --help`` from your cluster login node will provide a usage page, however the basic Gridware Depot functions available include: 

-  ``fetch`` - download a Depot from a remote URL and unpack
-  ``enable`` - enable the previously depot you previously used ``fetch`` on for global usage
-  ``disable`` - mark a depot as unavailable for use

Fetching a Depot
----------------
From the login node of your environment, as an appropriate user - fetch one of the publicly available Gridware Depots - such as the ``benchmark`` depot: 

.. code:: bash

    [alces@login1(hpc1) ~]$ alces gridware depot fetch https://s3-eu-west-1.amazonaws.com/packages.alces-software.com/depots/benchmark
    
     > Fetching depot
            Metadata ... OK
             Content ... OK
             Extract ... OK
                Link ... OK
    Depot 'benchmark' fetched successfully.

Once the Depot has been fetched, enable it for usage across your environment: 

.. code:: bash

    [alces@login1(hpc1) ~]$ alces gridware depot enable benchmark
    
     > Enabling depot: benchmark
              Enable ... OK

The ``benchmark`` depot will now be available in your available application modules: 

.. code:: bash

    [alces@login1(hpc1) ~]$ alces module avail
    ---  /opt/gridware/benchmark/el7/etc/modules  ---
      apps/imb/4.0/gcc-4.8.5+openmpi-1.8.5
      apps/iozone/3.420/gcc-4.8.5
      apps/memtester/4.3.0/gcc-4.8.5
      compilers/gcc/system
      libs/gcc/system
      mpi/openmpi/1.8.5/gcc-4.8.5
      null
    ---  /opt/gridware/local/el7/etc/modules  ---
      apps/fah/6.34/bin
      compilers/gcc/system
      libs/gcc/system
      null
    ---  /opt/clusterware/etc/modules  ---
      services/gridscheduler

