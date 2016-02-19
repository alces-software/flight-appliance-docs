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
