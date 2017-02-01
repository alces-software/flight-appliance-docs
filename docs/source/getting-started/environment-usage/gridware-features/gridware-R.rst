.. _gridware-R:

Working with Gridware Applications: R 
=====================================

The following guide will detail the installation of R, along with some of the advanced features of Alces Gridware when working with the R Gridware package. 

R Installation
--------------

Many different versions of R are available through the Alces Gridware utility - you can see the list of available packages with the ``alces gridware list`` function, for example: 

.. code:: bash

    [alces@login1(scooby) ~]$ alces gridware list R
    main/apps/R/3.2.3  main/apps/R/3.2.5  main/apps/R/3.3.0  main/apps/R/3.3.1

To install, for example - R version 3.3.1; run the following command: 

.. code:: bash

    [alces@login1(scooby) ~]$ alces gridware install apps/R/3.3.1
    Preparing to install main/apps/R/3.3.1
    Installing main/apps/R/3.3.1
    Importing apps-R-3.3.1-el7.tar.gz
    
     > Fetching archive
            Download ... OK
    
     > Preparing import
             Extract ... OK
              Verify ... OK
    
     > Processing apps/R/3.3.1/gcc-4.8.5+lapack-3.5.0+blas-3.6.0
           Preparing ... OK
           Importing ... OK
         Permissions ... OK
    
     > Finalizing import
              Update ... OK
        Dependencies ... OK
    
    Installation complete.

Once the compilation has finished - the R 3.3.1 Gridware package will be available for use; check its availability and load using: 

.. code:: bash

    [alces@login1(scooby) ~]$ module avail
    ---  /opt/gridware/local/el7/etc/modules  ---
      apps/R/3.3.1/gcc-4.8.5+lapack-3.5.0+blas-3.6.0
      apps/samtools/0.1.19/gcc-4.8.5
      compilers/gcc/system
      libs/gcc/system
      null
    ---  /opt/clusterware/etc/modules  ---
      null
      services/gridscheduler
      services/pdsh

    [alces@login1(scooby) ~]$ module load apps/R
    apps/R/3.3.1/gcc-4.8.5+lapack-3.5.0+blas-3.6.0
     | -- libs/gcc/system
     |    * --> OK
     |
     OK

    [alces@login1(scooby) ~]$ R --version
    R version 3.3.1 (2016-06-21) -- "Bug in Your Hair"
    Copyright (C) 2016 The R Foundation for Statistical Computing
    Platform: x86_64-pc-linux-gnu (64-bit)


Multiple versions of a package can exist at one time, however only one version of a particular application can be loaded at any one time - to load a different version of R: 

.. code:: bash

    [alces@login1(scooby) ~]$ alces module load apps/R/3.3.1
    apps/R/3.3.1/gcc-4.8.5+lapack-3.5.0+blas-3.6.0
     | -- libs/gcc/system ... SKIPPED (already loaded)
     |
     OK
    [alces@login1(scooby) ~]$ alces module unload apps/R
    apps/R/3.3.1/gcc-4.8.5+lapack-3.5.0+blas-3.6.0 ...
                                                 UNLOADING --> OK
    [alces@login1(scooby) ~]$ alces module load apps/R/3.2.3
    apps/R/3.2.3/gcc-4.8.5+lapack-3.5.0+blas-3.6.0
     | -- libs/gcc/system ... SKIPPED (already loaded)
     |
     OK
    [alces@login1(scooby) ~]$ R --version
    R version 3.2.3 (2015-12-10) -- "Wooden Christmas-Tree"
    Copyright (C) 2015 The R Foundation for Statistical Computing
    Platform: x86_64-pc-linux-gnu (64-bit)


Installation of language libraries
----------------------------------

Through the Alces Gridware utility, installation of lanaguage libraries is possible both on a system-wide level, and also on a per-user basis. The following section details both system-wide language library installation, as well as user-level language library installation.

System-wide language libraries: R
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

As the ``alces`` administrator user, or any other sudo enabled user that can switch to root - change to the ``root`` user account. 

To add R packages, first load the version of R you wish to install packages to - for example ``apps/R/3.2.3``: 

.. code:: bash

    [alces@login1(scooby) ~]$ sudo -s
    [root@login1(scooby) alces]# module load apps/R/3.2.3
    apps/R/3.2.3/gcc-4.8.5+lapack-3.5.0+blas-20110419
     | -- libs/gcc/system
     |    * --> OK
     |
     OK

Next, load the ``R`` application - and use the ``install.packages`` command to install your desired system-wide packages: 

.. code:: bash

    [root@login1(scooby) alces]# R
    
    R version 3.2.3 (2015-12-10) -- "Wooden Christmas-Tree"
    Copyright (C) 2015 The R Foundation for Statistical Computing
    Platform: x86_64-pc-linux-gnu (64-bit)
    
    Type 'q()' to quit R.
    
    > install.packages("randomForest", repos="http://cran.cnr.berkeley.edu")
    Installing package into ‘/opt/gridware/depots/22072cfc/el7/share/R/3.2.3’
    (as ‘lib’ is unspecified)
    trying URL 'http://cran.cnr.berkeley.edu/src/contrib/randomForest_4.6-12.tar.gz'
    Content type 'application/x-gzip' length 79566 bytes (77 KB)
    ==================================================
    downloaded 77 KB
    
    * installing *source* package ‘randomForest’ ...
    <snip>
    ** testing if installed package can be loaded
    * DONE (randomForest)
        
    > library(randomForest)
    randomForest 4.6-12
    Type rfNews() to see new features/changes/bug fixes.

Once the installation is complete and you have verified the package works as intended, you can check the package is available to other users on the system: 

.. code:: bash

    [alces@login1(scooby) ~]$ module load apps/R/3.2.3
    apps/R/3.2.3/gcc-4.8.5+lapack-3.5.0+blas-3.6.0
     | -- libs/gcc/system ... SKIPPED (already loaded)
     |
     OK
    
    [alces@login1(scooby) ~]$ R
    
    R version 3.2.3 (2015-12-10) -- "Wooden Christmas-Tree"
    Copyright (C) 2015 The R Foundation for Statistical Computing
    Platform: x86_64-pc-linux-gnu (64-bit)
    
    Type 'q()' to quit R.
    
    > library(randomForest)
    randomForest 4.6-12
    Type rfNews() to see new features/changes/bug fixes.


User-specific language libraries: R
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Users may also wish to install their own language libraries; these will be unavailable to other users of the environment. 

As the user you wish to install an R package for, load the version of R you wish to install the packages for (e.g. ``apps/R/3.2.3``). 

After the R application is loaded, use the ``install.packages("packagename")`` function to install packages you require - for example: 

.. code:: bash

    [alces@login1(scooby) ~]$ module load apps/R/3.2.3
    apps/R/3.2.3/gcc-4.8.5+lapack-3.5.0+blas-3.6.0
     | -- libs/gcc/system ... SKIPPED (already loaded)
     |
     OK
    
    [alces@login1(scooby) ~]$ R
    
    R version 3.2.3 (2015-12-10) -- "Wooden Christmas-Tree"
    Copyright (C) 2015 The R Foundation for Statistical Computing
    Platform: x86_64-pc-linux-gnu (64-bit)
    
    Type 'q()' to quit R.
    
    > install.packages("snow")
    Installing package into ‘/home/alces/gridware/share/R/3.2.3’
    (as ‘lib’ is unspecified)
    * installing *source* package ‘snow’ ...
    * DONE (snow)
    
    > packageVersion("snow")
    [1] ‘0.4.2’


The ``snow`` package installation was successful - and we can now use it as the ``alces`` user. Switching to another user will confirm the user-level installation success, the ``root`` user will not be able to use the ``snow`` R package: 

.. code:: bash

    [alces@login1(scooby) ~]$ sudo -s
    [root@login1(scooby) alces]# module load apps/R/3.2.3
    [root@login1(scooby) alces]# R
    
    R version 3.2.3 (2015-12-10) -- "Wooden Christmas-Tree"
    Copyright (C) 2015 The R Foundation for Statistical Computing
    Platform: x86_64-pc-linux-gnu (64-bit)
    
    Type 'q()' to quit R.
    
    > library(snow)
    Error in library(snow) : there is no package called ‘snow’

