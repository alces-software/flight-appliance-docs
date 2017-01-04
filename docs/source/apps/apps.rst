.. _apps:

Software Applications
#####################

Flight Compute operating system
-------------------------------

The current revision of Alces Flight Compute builds personal, ephemeral clusters based on a 64-bit CentOS 7.3 Linux distribution. The same operating system is installed on all login and compute nodes across the cluster. The Linux distribution includes a range of software tools and utilities, packaged by the CentOS project as RPM files. These packages are available for users to install as required on login and compute nodes using the ``yum`` command. You can also install other RPM packages on your Flight Compute cluster by copying them and installing them using the ``rpm`` command. 

Installing Linux applications on the login node
===============================================

Users who are new to Linux may find it convenient to install applications on the login node only via a graphical session from the Linux application catalogue. Follow the steps below to enable the graphical Linux package installer:

 - Start a new graphical session if you don't already have one; e.g. 
     ``alces session start gnome``
 - Set a password for your user, using the sudo command. For example, if your Flight Compute cluster was launched with the username "jane", enter the command:
     ``sudo passwd jane``
 - Connect to the session from your VNC client, using the details provided.
 - Click on the **Applications** menu in the top-right-hand corner of the desktop session, navigate to **System Tools**, and select the **Application Installer** option.
 
 .. image:: graphicalappinstaller.jpg
     :alt: Launch software packager
 
 - Choose the applications to be installed on your login node; enter your password when prompted

 .. image:: appinstaller.jpg
     :alt: Launch software packager

.. note:: Applications installed via the Linux packager are installed on the cluster login node only. We recommend using the **Alces Gridware** packaging system for installing applications to be used across multiple cluster nodes. 


Shared cluster applications
---------------------------
While RPM packages are useful for system packages, they are not designed to manage software applications that have complex dependencies, span multiple nodes or coexist with other, incompatible applications. Alces Flight Compute clusters include a mechanism to install and manage software applications on your cluster called **Alces Gridware**. Gridware packages are centrally installed on your shared cluster filesystem, making them available to all cluster login and compute nodes. New applications are installed with supporting **environment module** files, and can be dynamically optimised at installation time for specific environments. 

Shared application storage
==========================

For Flight Compute clusters launched from AWS Marketplace, your applications are automatically stored in the shared cluster filesystem, making them available to all login and compute nodes across the cluster. There are two directories used to host applications on your Flight Compute cluster:

 - ``/opt/gridware/`` - Applications managed by Alces Gridware utility
 - ``/opt/apps/`` - An empty directory for user-installed applications

.. note:: Depending on the version of Flight Compute you are using, you may have the option to choose capacity and performance characteristics of the shared applications volume at cluster launch time. Ensure that you choose a large enough storage area to suit the applications you want to install.

Installing cluster applications
===============================

Alces Flight Compute clusters include access to the online **Gridware** repository of software applications. This catalogue `includes over 850 application, library, compiler and MPI versions <http://tiny.cc/gridware>`_ which can be installed and run on your cluster. Software is installed by selecting it from the catalogue, optionally compiling it on the cluster login node along with any software package dependencies, and installing it in the shared cluster application storage space. Once installed, applications can be run on the login and compute nodes interactively, or as part of job-scripts submitted to your cluster scheduler. 

Application catalogue structure
===============================

Software applications are listed in the Alces Gridware repository with the structure ``repository/type/name/version``, which corresponds to:

 - **repository** - packages are listed in the **main** repository if available for auto-scaling clusters, and the **volatile** repository otherwise. 
 - **type** - packages are listed as **apps** (applications), **libs** (shared libraries), **compilers** or **mpi** (message-passing interface API software for parallel applications)
 - **name** - the name of the software package
 - **version** - the published version of the software package

For example, a package listed as ``main/apps/bowtie2/2.2.6`` is version 2.2.6 of the Bowtie2 application, from the stable repository. 

Finding an application to install
=================================

From the login node of your Alces Flight Compute cluster, use the command ``alces gridware list`` to view the available packages for installation. To search for a particular package, use the ``alces gridware search <search-word>`` command; e.g. 

.. code:: bash

    [alces@login1(scooby) ~]$ alces gridware search bowtie
    main/apps/bowtie/1.1.0   main/apps/bowtie2/2.2.6  main/apps/tophat/2.1.0

.. note:: By default, only the ``main`` repository is enabled; please read the instructions below to enable and use packages from the ``volatile`` repository. 


Installing a Gridware application
=================================
 
Use the command ``alces gridware install <package-name>`` to install a new package; e.g.

.. code:: bash

	[alces@login1(scooby) ~]$ alces gridware install apps/memtester
	Preparing to install main/apps/memtester/4.3.0
	Installing main/apps/memtester/4.3.0
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
	
	Installation complete.

.. note:: Gridware will automatically install pre-compiled binary versions of applications from the **main** repository, if they are available. Users can optionally use the ``--no-binary`` parameter to force packages to be compiled at installation time. 

Where more than one version of the requested application exists in the repository, users will be prompted for more information when attempting to install:

.. code:: bash

    [alces@login1(scooby) ~]$ alces gridware install apps/samtools
    More than one matching package found, please choose one of:
    main/apps/samtools/0.1.18  main/apps/samtools/0.1.19  main/apps/samtools/1.3
    
    [alces@login1(scooby) ~]$ alces gridware install apps/samtools/0.1.19
    Preparing to install main/apps/samtools/0.1.19
    Installing main/apps/samtools/0.1.19
    Importing apps-samtools-0.1.19-el7.tar.gz
        
     > Fetching archive
            Download ... OK
    
     > Preparing import
             Extract ... OK
              Verify ... OK
    
     > Processing apps/samtools/0.1.19/gcc-4.8.5
           Preparing ... OK
           Importing ... OK
         Permissions ... OK
    
     > Finalizing import
              Update ... OK
        Dependencies ... OK
    
    Installation complete.


For more complex applications, Alces Gridware may need to additionally build other applications, libraries and MPIs to support the installation. Users will be prompted if multiple installations will be required to make the requested package available:

.. code:: bash

    [alces@login1(scooby) ~]$ alces gridware install apps/R
    Preparing to install main/apps/R/3.2.3
    
    WARNING: Package requires the installation of the following:
      main/apps/cmake/3.5.2, main/libs/blas/3.6.0, main/libs/lapack/3.5.0
    
    Install these dependencies first?
    
    Proceed (Y/N)?


Modules environment management
------------------------------

The `Modules environment management <http://modules.sourceforge.net/>`_ system allows simple configuration of a users' Linux environment across a HPC compute cluster. It allows multiple software applications to be installed together across a group of systems, even if the different applications are incompatible with each other. Modules can also provide basic dependency analysis and resolution for software, helping users to make sure that their applications run correctly. An Alces Flight Compute cluster user can use modules to access the application software they need for running their jobs.

.. note:: Environment modules are included with your Alces Flight Compute cluster for convenience - users are free to use standard Linux configuration methods to setup their environment variables if they prefer. 

Environment modules work by configuring three existing Linux environment variables:

.. code:: bash

    $PATH
    $LD_LIBRARY_PATH
    $MANPATH

By manipulating these variables, the modules system can application binaries in your path, ensure that compatible library files are in your library path, and setup manual pages for applications. A library of module files is included with your Flight Compute cluster, and is automatically managed by the **Alces Gridware** software packager. 


Using environment modules
=========================

Users can view the available environment modules on their Alces Flight Compute cluster by using the ``module avail`` command:

.. code:: bash

    [alces@login1(scooby) ~]$ module avail 
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
     ---  /opt/apps/etc/modules  ---
       null

To load a new module for the current session, use the ``module load <module-name>`` command; any dependant modules will also be loaded automatically:

.. code:: bash

    [alces@login1(scooby) ~]$ module load apps/memtester
    apps/memtester/4.3.0/gcc-4.8.5
     | -- libs/gcc/system
     |    * --> OK
     |
     OK

.. note:: Module names will auto-complete if you type the first few letters, then press the **<TAB>** button on your keyboard. 

To unload a module file for the current session, use the ``module unload <module name>`` command. To allow users to configure specific versions of applications, the ``module unload`` command does not perform dependency analysis. 

.. code:: bash

    [alces@login1(scooby) ~]$ module unload apps/memtester
              apps/memtester/4.3.0/gcc-4.8.5 ... UNLOADING --> OK
              

Module files can be loaded interactively at the command-line or graphical desktop on both login and compute nodes in your cluster. They can also be loaded as part of a job-script submitted to the cluster job-scheduler. 

Applications that have Linux distribution dependencies will trigger installation of any required packages when their module is loaded on compute nodes for the first time. This allows newly launched nodes (e.g. in an auto-scaling cluster) to automatically resolve and install any dependencies without user intervention. 

.. note:: Automatic dependency installation can occasionally cause a brief delay at module load time when an application is run on a new compute node for the first time. 


Application specific variables
==============================

As well as the default environment variables (``$PATH, $LD_LIBRARY_PATH, $MANPATH``), modules included with Alces Flight Compute clusters also provide a number of additional Linux environment variables which are specific to the application being loaded. For example, to help users locate the application installation directory, the following variables are set automatically after laoding a named module file:

 - ``{APP-NAME}DIR`` - the location of the base application directory
     e.g. for the **HPL** application, the variable ``$HPLDIR`` contains the base location of the HPL application
 - ``{APP-NAME}BIN`` - the location of the application directory holding executable binaries
     e.g. for the **HPL** application, the variable ``$HPLBIN`` contains the location of binary files for HPL
 - ``{APP-NAME}EXAMPLES`` - the location of example files packaged with the application
     e.g. for the **HPL** application, the variable ``$HPLEXAMPLES`` contains an example HPL.dat file
     
     
You can use the ``module display <module-name>`` command to view all the environment variables that will be created when loading the module file for an application. 


Viewing application license information
=======================================

The open-source community forms the life-blood of computer-aided scientific research across the world, with software developers creating and publishing their work for free in order to help others. This collaborative model relies on the kindness and dedication of individuals, public and private organisations and independent research groups in taking the time to develop and publish their software for the benefit of us all. Users of open-source software have a responsibility to obey the licensing terms, credit the original authors and follow their shining example by contributing back to the community where possible - either in the form of new software, feedback and bug-reports for the packages you use and highlighting software usage in your research papers and publications. 

Applications installed by your Alces Flight Compute cluster include a module file that details the license type and original source URL for the package. Use the ``alces display <module-name>`` command to view this information:

.. code:: bash

    [alces@login1(scooby) ~]$ module display apps/hpl
    -------------------------------------------------------------------
    /opt/gridware/benchmark/el7/etc/modules/apps/hpl/2.1/gcc-4.8.5+openmpi-1.8.5+atlas-3.10.2:
    
    module-whatis
    
                Title: HPL
              Summary: A Portable Implementation of the High-Performance Linpack Benchmark for Distributed-Memory Computers
              License: Modified Free http://www.netlib.org/benchmark/hpl/copyright.html
                Group: Benchmarks
                  URL: http://www.netlib.org/benchmark/hpl/
    
                 Name: hpl
              Version: 2.1
               Module: apps/hpl/2.1/gcc-4.8.5+openmpi-1.8.5+atlas-3.10.2
          Module path: /opt/gridware/depots/1a995914/el7/etc/modules/apps/hpl/2.1/gcc-4.8.5+openmpi-1.8.5+atlas-3.10.2
         Package path: /opt/gridware/depots/1a995914/el7/pkg/apps/hpl/2.1/gcc-4.8.5+openmpi-1.8.5+atlas-3.10.2
    
           Repository: git+https://github.com/alces-software/packager-base.git@unknown
              Package: apps/hpl/2.1@9839698b
          Last update: 2016-05-05
    
              Builder: root@9bc1b720b60a
           Build date: 2016-05-05T17:16:55
        Build modules: mpi/openmpi/1.8.5/gcc-4.8.5, libs/atlas/3.10.2/gcc-4.8.5
             Compiler: compilers/gcc/system
               System: Linux 3.19.0-30-generic x86_64
                 Arch: Intel(R) Xeon(R) CPU @ 2.30GHz, 1x1 (29028551)
         Dependencies: libs/gcc/system (using: libs/gcc/system)
                       mpi/openmpi/1.8.5/gcc-4.8.5 (using: mpi/openmpi/1.8.5/gcc-4.8.5)
    
    For further information, execute:
        module help apps/hpl/2.1/gcc-4.8.5+openmpi-1.8.5+atlas-3.10.2
    
    -------------------------------------------------------------------
    
.. note:: Please remember to credit open-source contributors by providing a URL to the supporting project along with your research papers and publications.


Configuring modules for your default session
============================================

The ``module load`` command configures your current session only - when a user logs out of the cluster or starts a new session, they are returned to their initial set of modules. This is often preferable for users wanting to include ``module load`` commands in their cluster job-scripts, but it is also possible to instruct environment modules to configure the default login environment so modules are automatically loaded at every login.

Use the ``module initadd <module-file>`` command to add a software package to the list of automatically loaded modules. The ``module initrm <module-file`` command will remove an application from the list of automatically loaded modules; the ``module initlist`` command will display what applications are currently set to automatically load on login.

.. note:: Commands to submit jobs to your cluster job-scheduler are automatically included in your users' **$PATH** via a ``services/`` module. If you unload this module or remove it from your list of automatically-loaded modules, you may not be able to submit jobs to the cluster scheduler.




Volatile Gridware repositories
------------------------------

Applications packaged in the ``main`` repository are tested to support automatic dependancy resolution, enabling support for auto-scaling clusters where compute nodes may be sourced from the AWS spot market. This allows Linux distribution dependancies to be satisfied dynamically at ``module load`` time, ensuring that software applications execute correctly whenever they are run. For access to a larger catalogue of software, users can additionally enable the ``volatile`` software repository. Once enabled, advanced users can access the full list of available applications by choosing software along with any dependencies to install from the combined package list. 

.. note:: Users installing applications from the ``volatile`` repo should either ensure that auto-scaling is disabled for their user environment, or make use of Flight customization features to ensure that software package dependancies are resolved for new compute nodes joining the cluster after applications have been installed. 

To enable volatile repositories, edit the ``/opt/gridware/etc/gridware.yml`` YAML file and un-comment the volatile repository by removing the ``#`` symbol at the start of line 11. Alternatively, users can enable the repository by using the following command:

.. code:: bash

   sed -i 's?^# - /opt/clusterware/var/lib/gridware/repos/volatile? - /opt/clusterware/var/lib/gridware/repos/volatile?g' /opt/gridware/etc/gridware.yml

Finally, run the ``alces gridware update`` command to refresh the application catalogue. 

When installing packages from the volatile repo, users must resolve any dependencies before applications can be successfully installed. The Gridware packager will report any issues when attempting to install software from the volatile repo. The example below shows installation of the "beast" bioinformatics tool, which requires a Java Development Kit (JDK) to build:

.. code:: bash

    [alces@login1(scooby) ~]$ alces gridware install volatile/apps/beast/1.7.5
    Preparing to install volatile/apps/beast/1.7.5
    Installing volatile/apps/beast/1.7.5
    
     > Preparing package sources
            Download --> beast-1.7.5.tgz ... OK
              Verify --> beast-1.7.5.tgz ... OK
    
     > Preparing for installation
               Mkdir ... OK (/var/cache/gridware/src/apps/beast/1.7.5/gcc-4.8.5)
             Extract ... OK
    
     > Proceeding with installation
             Compile ... ERROR: Package compilation failed
    
       Extract of compilation script error output:
       > In file included from NucleotideLikelihoodCore.c:2:0:
       > NucleotideLikelihoodCore.h:7:17: fatal error: jni.h: No such file or directory
       > #include <jni.h>
       > ^
       > compilation terminated.
       > make: *** [NucleotideLikelihoodCore.o] Error 1
    [alces@login1(scooby) ~]$ 
    
The YUM utility can be used to identify any system packages which may satisfy build dependencies; e.g. 

.. code:: bash

    [alces@login1(scooby) ~]$ yum provides */jni.h
    Loaded plugins: fastestmirror
    Loading mirror speeds from cached hostfile
     * base: ftp.heanet.ie
     * extras: ftp.heanet.ie
     * updates: ftp.heanet.ie
    extras/7/x86_64/filelists_db                                                           | 296 kB  00:00:00
    updates/7/x86_64/filelists_db                                                          | 3.1 MB  00:00:00
    1:java-1.6.0-openjdk-devel-1.6.0.36-1.13.8.1.el7_1.x86_64 : OpenJDK Development Environment
    Repo        : base
    Matched from:
    Filename    : /usr/lib/jvm/java-1.6.0-openjdk-1.6.0.36.x86_64/include/jni.h

    [alces@login1(scooby) ~]$
    
Installing any dependencies may allow the software application to be installed as desired; e.g.

.. code:: bash

    [alces@login1(scooby) ~]$ pdsh -g cluster 'sudo yum -y -e0 install java-1.8.0-openjdk-devel'
    Resolving Dependencies
    --> Running transaction check
    ---> Package java-1.8.0-openjdk-devel.x86_64 1:1.8.0.91-0.b14.el7_2 will be installed
    --> Processing Dependency: java-1.8.0-openjdk = 1:1.8.0.91-0.b14.el7_2 for package: 1:java-1.8.0-openjdk-devel-1.8.0.91-0.b14.el7_2.x86_64
    --> Processing Dependency: libawt_xawt.so(SUNWprivate_1.1)(64bit) for package: 1:java-1.8.0-openjdk-devel-1.8.0.91-0.b14.el7_2.x86_64
    --> Processing Dependency: libawt_xawt.so()(64bit) for package: 1:java-1.8.0-openjdk-devel-1.8.0.91-0.b14.el7_2.x86_64
    --> Finished Dependency Resolution
    
    Dependencies Resolved
    
    ==============================================================================================================
     Package                            Arch             Version                          Repository         Size
    ==============================================================================================================
    Installing:
     java-1.8.0-openjdk-devel           x86_64           1:1.8.0.91-0.b14.el7_2           updates           9.7 M
    Installing for dependencies:
     java-1.8.0-openjdk                 x86_64           1:1.8.0.91-0.b14.el7_2           updates           219 k
     ttmkfdir                           x86_64           3.0.9-42.el7                     base               48 k
     xorg-x11-fonts-Type1               noarch           7.5-9.el7                        base              521 k
    
    Transaction Summary
    ==============================================================================================================
    Install  1 Package (+3 Dependent packages)
    
    Total download size: 11 M
    Installed size: 42 M
    Is this ok [y/d/N]: y
    Running transaction
    Installed:
      java-1.8.0-openjdk-devel.x86_64 1:1.8.0.91-0.b14.el7_2
    
    Dependency Installed:
      java-1.8.0-openjdk.x86_64 1:1.8.0.91-0.b14.el7_2               ttmkfdir.x86_64 0:3.0.9-42.el7
      xorg-x11-fonts-Type1.noarch 0:7.5-9.el7
    
    Complete!

    [alces@login1(scooby) ~]$ alces gridware install volatile/apps/beast/1.7.5
    Preparing to install volatile/apps/beast/1.7.5
    Installing volatile/apps/beast/1.7.5
    
    WARNING: Build directory already exists:
      /var/cache/gridware/src/apps/beast/1.7.5/gcc-4.8.5
    
    Proceed with a clean?
    
    Proceed (Y/N)? y
               Clean ... OK
    
     > Preparing package sources
            Download --> beast-1.7.5.tgz ... SKIP (Existing source file detected)
              Verify --> beast-1.7.5.tgz ... OK
    
     > Preparing for installation
               Mkdir ... OK (/var/cache/gridware/src/apps/beast/1.7.5/gcc-4.8.5)
             Extract ... OK
    
     > Proceeding with installation
             Compile ... OK
               Mkdir ... OK (/opt/gridware/depots/b7e5f115/el7/pkg/apps/beast/1.7.5/gcc-4.8.5)
             Install ... OK
              Module ... OK
    
    Installation complete.


Installing packages from a depot
--------------------------------

Alces Flight Compute clusters also support collated application depots which are preconfigured to include specific suites of applications for particular purposes. Depots can be used for the following purposes:

 - Creating a set of applications for a particular purpose (e.g. Bioinformatics, Engineering or Chemistry applications)
 - Collecting optimised applications together; e.g. those built with specialist accelerated compilers
 - Packaging your frequently used applications in a convenient bundle
 - Distributing your commercial applications (as permissible under the terms of the appropriate software license)
 
 To list the available depots for your environment, use the command ``alces gridware depot list``. New depots can be installed using the ``alces gridware depot install <depot-name>`` command; e.g. 
 
.. code:: bash
 
    [alces@login1(scooby) ~]$ alces gridware depot install benchmark
    Installing depot: benchmark
    
     > Initializing depot: benchmark
          Initialize ... OK
    
    Importing mpi-openmpi-1.8.5-el7.tar.gz
    
     > Fetching archive
            Download ... SKIP (Existing source file detected)
    
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
            Download ... SKIP (Existing source file detected)
    
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
    
     [alces@login1(scooby) ~]$

 
Once installed, enable a new depot using the ``alces gridware depot enable <depot-name>`` command; e.g.
 
.. code:: bash

    [alces@login1(scooby) ~]$ alces gridware depot enable benchmark
    
     > Enabling depot: benchmark
              Enable ... OK


Requesting new applications in Gridware
---------------------------------------

The list of applications available in the Gridware repository expands over time as more software is added and tested on Flight Compute clusters. Wherever possible, software is not removed from the repository, allowing users to rely on applications continuing to be available for a particular release of Alces Flight. New versions of existing applications are also added over time - newly launched Flight Compute clusters automatically use the latest revision of the Gridware repository; use the ``alces gridware update`` command to refresh any running Flight Compute clusters with the latest updates.

If you need to use an application that isn't already part of the Alces Gridware project, there are three methods you can use to get access to the application:

 1. Install the application yourself manually (see below). This is a good first step for any new software package, as it will allow you to evaluate its use on a cluster and confirm that it works as expected in  a Flight Compute cluster environment.
 2. `Request the addition of an application via the community support site <http://community.alces-flight.com>`_. Please include as much information about the application as possible in your request to help new users of the package. There is no fee for requesting software via the community support site - this service is provided to benefit users worldwide by providing convenient access to the best open-source software packages available.
 3. If you have an urgent need for a new software package, users can fund consultancy time to have packages added to Gridware repository. Please add details of your funding offer to your enhancement request ticket on the `community support site <http://community.alces-flight.com>`_, and a software engineer will contact you with more details.


Manually installing applications on your cluster
------------------------------------------------

Your Alces Flight Compute cluster also allows manual installation of software applications into the ``/opt/apps/`` directory. This is useful for commercial applications that you purchase, and for software which you've written yourself or at your business or institution. Your Flight Compute cluster runs standard CentOS7, and should be compatible with any application tested on a CentOS, Scientific Linux or RedHat Enterprise Linux 7 distribution. It is often possible to run applications designed to run on other distributions with minimal modifications. 

Install new applications into a sub-directory of the ``/opt/apps/`` directory - this location is available on both login and compute nodes, allowing software to be run across your cluster. A example environment module tree is also included for use with manually installed applications - add new modules into the ``/opt/apps/etc/modules/`` directory to be included here. Documentation on creating your own module files `is available here <http://modules.sourceforge.net/man/modulefile.html>`_. 



