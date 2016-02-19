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

Installation of language libraries
----------------------------------

Through the Alces Gridware utility, installation of lanaguage libraries is possible both on a system-wide level, and also on a per-user basis. The following section details both system-wide language library installation, as well as user-level language library installation.

System-wide language libraries: Python
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

As the ``alces`` administrator user, or any other sudo enabled user that can switch to root - change to the ``root`` user account.

To add Python packages, the ``setuptools`` Gridware application is required - this can be installed using ``alces gridware install setuptools/15.1 --variant default``. Once the ``setuptools`` module is available, load it as the ``root`` user: 

.. code:: bash

    [root@login1(hpc1) ~]# module load apps/setuptools
    apps/setuptools/15.1/python-2.7.8
     | -- apps/python/2.7.8/gcc-4.8.5
     |    | -- libs/gcc/system
     |    |    * --> OK
     |    * --> OK
     |
     OK

Next, using ``easy_install`` - install the Python libraries required, for example: 

.. code:: bash

    [root@login1(hpc1) ~]# easy_install numpy
    Creating /opt/gridware/share/python/2.7.8/lib/python2.7/site-packages/site.py
    Searching for numpy
    Reading https://pypi.python.org/simple/numpy/
    Best match: numpy 1.11.0b3
    <-- snip -->
    Installed /opt/gridware/share/python/2.7.8/lib/python2.7/site-packages/numpy-1.11.0b3-py2.7-linux-x86_64.egg
    Processing dependencies for numpy
    Finished processing dependencies for numpy

Once the installation is complete - you can check the library is available to other users on the system: 

.. code:: bash

    [barney@login1(hpc1) ~]$ module load apps/python/2.7.8
    apps/python/2.7.8/gcc-4.8.5
     | -- libs/gcc/system
     |    * --> OK
     |
     OK
    [barney@login1(hpc1) ~]$ python
    Python 2.7.8 (default, Feb 19 2016, 10:02:41)
    [GCC 4.8.5 20150623 (Red Hat 4.8.5-4)] on linux2
    Type "help", "copyright", "credits" or "license" for more information.
    >>> import numpy
    >>> numpy.version.version
    '1.11.0b3'

User-specific language libraries: Python
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Users may also wish to install their own language libraries, these will be unavailable to other users of the environment. 

As the user you wish to install a Python library for, load the ``setuptools`` Gridware application for the version of Python you wish to install libraries for (e.g. ``apps/setuptools/15.1/python-2.7.8``), then use ``easy_install`` to install the required module: 

.. code:: bash

    [barney@login1(hpc1) ~]$ easy_install htseq
    Searching for htseq
    Reading https://pypi.python.org/simple/htseq/
    Best match: HTSeq 0.6.1
    <-- snip -->
    Installed /home/barney/gridware/share/python/2.7.8/lib/python2.7/site-packages/HTSeq-0.6.1-py2.7-linux-x86_64.egg
    Processing dependencies for htseq
    Finished processing dependencies for htseq
    [barney@login1(hpc1) ~]$ python
    Python 2.7.8 (default, Feb 19 2016, 10:02:41)
    [GCC 4.8.5 20150623 (Red Hat 4.8.5-4)] on linux2
    Type "help", "copyright", "credits" or "license" for more information.
    >>> import HTSeq
    >>> HTSeq.__version__
    '0.6.0'

The ``htseq`` installation was successful - and we can now use it as the ``barney`` user. Switching to another user will confirm the user-level installation success, the ``alces`` user will not be able to user the ``HTSeq`` Python library: 

.. code:: bash

    [alces@login1(hpc1) ~]$ module load apps/python
    apps/python/2.7.8/gcc-4.8.5
     | -- libs/gcc/system
     |    * --> OK
     |
     OK
    [alces@login1(hpc1) ~]$ python
    Python 2.7.8 (default, Feb 19 2016, 10:02:41)
    [GCC 4.8.5 20150623 (Red Hat 4.8.5-4)] on linux2
    Type "help", "copyright", "credits" or "license" for more information.
    >>> import HTSeq
    Traceback (most recent call last):
      File "<stdin>", line 1, in <module>
    ImportError: No module named HTSeq

