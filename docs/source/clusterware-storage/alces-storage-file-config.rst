.. _alces-storage-file-config:

Alces Storage File Storage Configuration
========================================

The following section will detail how to configure your local POSIX storage for use with the Alces Storage utility. 

Prerequisites
-------------

-  :ref:`deployment`
-  Access gained to deployed environment

Configuring your storage
------------------------
Once you have gained access to your Alces compute environment - you can begin configuring your local POSIX storage for use with the Alces Storage tool. 

To work with POSIX storage, the Alces Storage utility must first enable the POSIX storage type, as the cluster administrator user - enable the POSIX storage type: 

.. code:: bash

    [alces@login1(cluster1) ~]$ alces storage enable posix
    alces storage enable: enabled storage type: base/posix -> posix

Creating basic file storage targets
-----------------------------------

Now we can begin configuring our local POSIX storage with the Alces Storage utility. For this example, we will add a private storage configuration - using the ``alces`` users home directory. To configure your home directory as a storage target, use the ``alces storage configure`` command as follows:

.. code:: bash

    [alces@login1(cluster1) ~]$ alces storage configure cluster1-home posix
    Display name [cluster1-home]:
    Path: /home/alces/
    alces storage configure: storage configuration complete 

You will now be able to see the ``cluster1-home`` storage target available in your list of storage targets: 

.. code:: bash

    [alces@login1(cluster1) ~]$ alces storage avail
    [*] cluster1-home (posix)

The ``[*]`` box indicates that ``cluster1-home`` is the storage target currently in use, if multiple storage targets are added, you will need to switch between storage targets using the ``alces storage use <target-name>`` function.

Alternatively, a non-default storage target can be used with the ``-n <storage-target>`` option, for example: 

.. code:: bash

    [alces@login1(cluster1) ~]$ alces storage get -n storage1-data testfile

Creating cluster wide storage targets
-------------------------------------

In a shared environment -- you may wish to provide cluster wide storage targets, made available for other users of your environment. Configuring cluster wide targets requires the ``root`` account. 

To create a cluster wide storage target for all users of the environment - run the following commands, customising to your own requirements: 

.. code:: bash

    [root@login1(cluster1) ~]# mkdir -p /export/data
    [root@login1(cluster1) ~]# chmod 0755 /export/data
    [root@login1(cluster1) ~]# alces storage configure --system cluster1-data posix
    Display name [cluster1-data]:
    Path: /export/data
    alces storage configure: storage configuration complete

Once you have completed the configuration - other users in the environment will be able to see and use the shared cluster-wide storage target: 

.. code:: bash

    [alces@login1(cluster1) ~]$ alces storage avail
    [ ] cluster1-data (posix)
    [*] cluster1-home (posix)
    [wilma@login1(cluster1) ~]$ alces storage avail
    [ ] cluster1-data (posix)

Working with file storage
-------------------------

For information on working with your file storage targets, please see the following guide: 

-  :ref:`Alces Storage: File Usage <alces-storage-file-usage>`
