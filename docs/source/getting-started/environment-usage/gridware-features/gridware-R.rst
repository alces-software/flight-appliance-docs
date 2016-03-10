.. _gridware-R:

Working with Gridware Applications: R 
=====================================

The following guide will detail the installation of R, along with some of the advanced features of Alces Gridware when working with the R Gridware package. 

R Installation
--------------

Many different versions of R are available through the Alces Gridware utility - you can see the list of available packages with the ``alces gridware list`` function, for example: 

.. code:: bash

    [alces@login1(hpc1) ~]$ alces gridware list R
    base/apps/R/2.15.0  base/apps/R/2.15.1  base/apps/R/2.15.2  base/apps/R/2.15.3
    base/apps/R/3.0.0   base/apps/R/3.0.1   base/apps/R/3.1.1   base/apps/R/3.1.2
    base/apps/R/3.2.0   base/apps/R/3.2.1   base/apps/R/3.2.2

To install, for example - R version 3.2.2; run the following command: 

.. code:: bash

    [alces@login1(hpc1) ~]$ alces gridware install R/3.2.2
    Installing base/apps/R/3.2.2
    ERROR: Unable to satisfy compilation requirements: libs/lapack, libs/blas

The Alces Gridware tool automatically checks dependencies for you - in this case, we do not have some of the required libraries needed to compile the R application. First - install the ``libs/lapack`` and ``libs/blas`` Gridware packages, you will then be able to proceed with the ``R 3.2.2`` installation: 

.. code:: bash

    [alces@login1(hpc1) ~]$ alces gridware install R/3.2.2
    Installing base/apps/R/3.2.2
     > Preparing package sources
            Download --> R-3.2.2.tar.gz ... SKIP (Existing source file detected)
              Verify --> R-3.2.2.tar.gz ... OK
            Packaged --> Rprofile       ... OK
    
     > Preparing for installation
               Mkdir ... OK (/var/cache/gridware/src/apps/R/3.2.2/gcc-4.8.5+lapack-3.5.0+blas-20110419)
             Extract ... OK
    
     > Proceeding with installation
             Compile ... OK
               Mkdir ... OK
    (/opt/gridware/depots/2b8a9f1c/el7/pkg/apps/R/3.2.2/gcc-4.8.5+lapack-3.5.0+blas-20110419)
             Install ... OK
              Module ... OK
    
    Installation complete.

Once the compilation has finished - the R 3.2.2 Gridware package will be available for use, check its availability and load using: 

.. code:: bash

    [alces@login1(hpc1) ~]$ module avail
    ---  /opt/gridware/local/el7/etc/modules  ---
      apps/perl/5.18.0/gcc-4.8.5
      apps/perl/5.20.2/gcc-4.8.5
      apps/python/2.7.5/gcc-4.8.5
      apps/python/2.7.8/gcc-4.8.5
      apps/R/3.2.2/gcc-4.8.5+lapack-3.5.0+blas-20110419
      compilers/gcc/system
      libs/blas/20110419/gcc-4.8.5
      libs/gcc/system
      libs/lapack/3.5.0/gcc-4.8.5
      null
    ---  /opt/clusterware/etc/modules  ---
      null
      services/gridscheduler
    [alces@login1(hpc1) ~]$ module load apps/R
    apps/R/3.2.2/gcc-4.8.5+lapack-3.5.0+blas-20110419
     | -- libs/gcc/system ... SKIPPED (already loaded)
     |
     OK
    [alces@login1(hpc1) ~]$ R --version
    R version 3.2.2 (2015-08-14) -- "Fire Safety"
    Copyright (C) 2015 The R Foundation for Statistical Computing
    Platform: x86_64-pc-linux-gnu (64-bit)

Multiple versions of a package can exist at one time, however only one version of a particular application can be loaded at any one time - to load a different version of R: 

.. code:: bash

    [alces@login1(hpc1) ~]$ alces module load apps/R/3.1.2
    apps/R/3.1.2/gcc-4.8.5+lapack-3.5.0+blas-20110419 ... VARIANT (have alternative: apps/R/3.2.2/gcc-4.8.5+lapack-3.5.0+blas-20110419)
    [alces@login1(hpc1) ~]$ alces module unload apps/R/3.2.2
    apps/R/3.2.2/gcc-4.8.5+lapack-3.5.0+blas-20110419 ...
                                                 UNLOADING --> OK
    [alces@login1(hpc1) ~]$ alces module load apps/R/3.1.2
    apps/R/3.1.2/gcc-4.8.5+lapack-3.5.0+blas-20110419
     | -- libs/gcc/system ... SKIPPED (already loaded)
     |
     OK
    [alces@login1(hpc1) ~]$ R --version
    R version 3.1.2 (2014-10-31) -- "Pumpkin Helmet"

Installation of language libraries
----------------------------------

Through the Alces Gridware utility, installation of lanaguage libraries is possible both on a system-wide level, and also on a per-user basis. The following section details both system-wide language library installation, as well as user-level language library installation.

System-wide language libraries: R
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

As the ``alces`` administrator user, or any other sudo enabled user that can switch to root - change to the ``root`` user account. 

To add R packages, first load the version of R you wish to install packages to - for example ``apps/R/3.2.2``: 

.. code:: bash

    [root@login1(hpc1) ~]# module load apps/R/3.2.2
    apps/R/3.2.2/gcc-4.8.5+lapack-3.5.0+blas-20110419
     | -- libs/gcc/system
     |    * --> OK
     |
     OK

Next, load the ``R`` application - and use the ``install.packages`` command to install your desired system-wide packages: 

.. code:: bash

    [root@login1(hpc1) ~]# R
    R version 3.2.2 (2015-08-14) -- "Fire Safety"
    Copyright (C) 2015 The R Foundation for Statistical Computing
    Platform: x86_64-pc-linux-gnu (64-bit)
    
    > install.packages("randomForest", repos="http://cran.cnr.berkeley.edu")
    Installing package into ‘/opt/gridware/share/R/3.2.2’
    (as ‘lib’ is unspecified)
    trying URL 'http://cran.cnr.berkeley.edu/src/contrib/randomForest_4.6-12.tar.gz'
    <-- snip -->
    > library(randomForest)
    randomForest 4.6-12
    Type rfNews() to see new features/changes/bug fixes.

Once the installation is complete and you have verified the package works as intended, yo ucan check the package is available to other users on the system: 

.. code:: bash

    [barney@login1(hpc1) ~]$ module load apps/R/3.2.2
    apps/R/3.2.2/gcc-4.8.5+lapack-3.5.0+blas-20110419
     | -- libs/gcc/system
     |    * --> OK
     |
     OK
    [barney@login1(hpc1) ~]$ R
    
    R version 3.2.2 (2015-08-14) -- "Fire Safety"
    Copyright (C) 2015 The R Foundation for Statistical Computing
    Platform: x86_64-pc-linux-gnu (64-bit)
    <-- snip -->
    > library(randomForest)
    randomForest 4.6-12
    Type rfNews() to see new features/changes/bug fixes.

User-specific language libraries: R
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Users may also wish to install their own language libraries, these will be unavailable to other users of the environment. 

As the user you wish to install an R package for, load the version of R you wish to install the packages for (e.g. ``apps/R/3.2.2``). 

After the R application is loaded, use the ``install.packages("packagename")`` function to install packages you require - for example: 

.. code:: bash

    [barney@login1(hpc1) ~]$ module load apps/R/3.2.2
    apps/R/3.2.2/gcc-4.8.5+lapack-3.5.0+blas-20110419
     | -- libs/gcc/system ... SKIPPED (already loaded)
     |
     OK
    [barney@login1(hpc1) ~]$ R
    
    R version 3.2.2 (2015-08-14) -- "Fire Safety"
    Copyright (C) 2015 The R Foundation for Statistical Computing
    Platform: x86_64-pc-linux-gnu (64-bit)
    <-- snip -->
    > install.packages("snow")
    Installing package into ‘/home/barney/gridware/share/R/3.2.2’
    (as ‘lib’ is unspecified)
    <-- snip -->
    > library(snow)
    > packageVersion("snow")
    [1] ‘0.4.1’

The ``snow`` package installation was successful - and we can now use it as the ``barney`` user. Switching to another user will confirm the user-level installation success, the ``alces`` user will not be able to use the ``snow`` R package: 

.. code:: bash

    [alces@login1(hpc1) ~]$ module load apps/R/3.2.2
    apps/R/3.2.2/gcc-4.8.5+lapack-3.5.0+blas-20110419
     | -- libs/gcc/system
     |    * --> OK
     |
     OK
    [alces@login1(hpc1) ~]$ R
    
    R version 3.2.2 (2015-08-14) -- "Fire Safety"
    Copyright (C) 2015 The R Foundation for Statistical Computing
    Platform: x86_64-pc-linux-gnu (64-bit)
    <-- snip -->
    > library(snow)
    Error in library(snow) : there is no package called ‘snow’
