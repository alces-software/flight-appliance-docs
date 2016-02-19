.. _gridware-python:

Working with Gridware Applications: Python
==========================================

The following guide will detail the installation of Python, along with some of the advanced features of Alces Gridware when working with the Python Gridware package. 

Python Installation
-------------------

Many different versions of Python are available through the Alces Gridware utility - you can see the list of available packages with the ``alces gridware search`` function, for example: 

.. code:: bash

    [alces@login1(hpc1) ~]$ alces gridware search --name python
    base/apps/ipython/2.3.0   base/apps/python/2.7.3    base/apps/python/2.7.5
    base/apps/python/2.7.8    base/apps/python3/3.2.3   base/apps/python3/3.3.3
    base/apps/python3/3.4.0   base/apps/python3/3.4.3   base/libs/biopython/1.61
    base/libs/biopython/1.63

To install, for example - Python version 2.7.8; run the following command: 

.. code:: bash

        [alces@login1(hpc1) ~]$ alces gridware install python/2.7.8
        Installing base/apps/python/2.7.8
    
     > Preparing package sources
            Download --> Python-2.7.8.tgz ... SKIP (Existing source file detected)
          Verify --> Python-2.7.8.tgz ... OK
    
     > Preparing for installation
               Mkdir ... OK (/var/cache/gridware/src/apps/python/2.7.8/gcc-4.8.5)
             Extract ... OK
        Dependencies ... OK
    
     > Proceeding with installation
             Compile ... OK
               Mkdir ... OK (/opt/gridware/depots/2b8a9f1c/el7/pkg/apps/python/2.7.8/gcc-4.8.5)
             Install ... OK
              Module ... OK
    
    Installation complete.

Once the compilation has finished - the Python 2.7.8 Gridware package will be available for use, check its availability and load using: 

.. code:: bash

    [alces@login1(hpc1) ~]$ alces module load apps/python
    apps/python/2.7.8/gcc-4.8.5
     | -- libs/gcc/system
     |    * --> OK
     |
     OK
    [alces@login1(hpc1) ~]$ python --version
    Python 2.7.8

Multiple versions of Python can be installed at once using Gridware and Modules - for example: 

.. code:: bash

    [alces@login1(hpc1) ~]$ alces module avail
    ---  /opt/gridware/local/el7/etc/modules  ---
      apps/python/2.7.5/gcc-4.8.5
      apps/python/2.7.8/gcc-4.8.5

Only one version of a particular application can be loaded at any one time - to load a different version of Python: 

.. code:: bash

    [alces@login1(hpc1) ~]$ alces module load apps/python/2.7.5/gcc-4.8.5
    apps/python/2.7.5/gcc-4.8.5 ... VARIANT (have alternative: apps/python/2.7.8/gcc-4.8.5)
    [alces@login1(hpc1) ~]$ alces module unload apps/python/2.7.8/gcc-4.8.5
                 apps/python/2.7.8/gcc-4.8.5 ... UNLOADING --> OK
    [alces@login1(hpc1) ~]$ alces module load apps/python/2.7.5/gcc-4.8.5
    apps/python/2.7.5/gcc-4.8.5
     | -- libs/gcc/system ... SKIPPED (already loaded)
     |
     OK
    [alces@login1(hpc1) ~]$ python --version
    Python 2.7.5
