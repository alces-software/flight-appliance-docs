.. _alces-storage-file-usage:

Alces Storage File Storage Usage
========================

The following section details how to use your :ref:`previously set up <alces-storage-file-config>` file storage targets. 

The Alces Storage utility provides several functions when interacting with your storage targets, including: 

-  Listing files and directories within a storage target
-  Adding/putting files and directories within a storage target
-  Retrieving/getting files and directories within a storage target
-  Deleting files and directories from within a storage target

As the ``alces`` administrator user - select your previously set up home directory target, e.g. ``alces storage use cluster1-home``

Viewing files in a storage target
---------------------------------
The ``alces storage list`` command can be used to list the top-level of a storage target, in this case the ``/home/alces`` directory - this will display all files and folders: 

.. code:: bash

    [alces@login1(cluster1) ~]$ alces storage list
    2016-02-16 12:02       DIR   .config
    2016-02-16 12:40       DIR   job-scripts
    2014-06-09 20:37       DIR   .mozilla
    2016-02-16 11:28       DIR   .ssh
    2016-02-16 12:32        20   .bash_history
    2016-02-16 12:18        18   .bash_logout
    2016-02-16 11:28       193   .bash_profile
    2016-02-16 11:28       231   .bashrc
    2016-02-16 11:28       118   clusterware-setup-sshkey.log
    2016-02-16 11:28       865   .modulerc
    2016-02-16 11:28       719   .modules

The ``list`` command can also be used to navigate directories within a storage target, for example you can look inside the ``job-scripts`` directory: 

.. code:: bash

    [alces@login1(cluster1) ~]$ alces storage list job-scripts
    2016-02-16 12:42         0   hello-world.sh
    2016-02-16 12:42         0   hpl-1node.sh
    2016-02-16 12:42         0   mpi-2nodes.sh

The ``list`` command can be used to view directories of any depth, for example: 

.. code:: bash

    [alces@login1(cluster1) ~]$ alces storage list job-scripts
    2016-02-16 12:44       DIR   results
    2016-02-16 12:42         0   hello-world.sh
    2016-02-16 12:42         0   hpl-1node.sh
    2016-02-16 12:42         0   mpi-2nodes.sh
    [alces@login1(cluster1) ~]$ alces storage list job-scripts/results
    2016-02-16 12:44         0   mpi-2nodes.output.1

Adding files to a POSIX storage target
--------------------------------------

The Alces Storage command can be used to add files to your POSIX storage targets - this can be used in some of the following ways: 

-  Manually via CLI tools
-  Automatically through scheduler batch scripts

The following examples will detail some of the many possible uses of the ``alces storage put`` utility. 

Manually adding files
^^^^^^^^^^^^^^^^^^^^^

This can be used from any of the available nodes within your environment. To demonstratie, SSH to one of your available compute hosts - then add a test file to the ``/tmp`` directory of your compute node, e.g.: 

.. code:: bash

    [alces@node00(cluster1) ~]$ touch /tmp/mytestfile
    [alces@node00(cluster1) ~]$ ls /tmp/
    mytestfile 

Using the ``alces storage put`` utility - we can easily add the ``mytestfile`` file to our ``/home/alces`` storage target using the following command:

.. code:: bash

    [alces@node00(cluster1) ~]$ alces storage put /tmp/mytestfile
    alces storage put: /tmp/mytestfile -> cluster1-home:mytestfile

Files can also be ``put`` to sub-directories as required.

The ``mytestfile`` file will now be available in the ``cluster1-home`` storage target: 

.. code:: bash

    [alces@node00(cluster1) ~]$ alces storage list
    2016-02-16 12:51       DIR   .config
    2016-02-16 12:44       DIR   job-scripts
    2014-06-09 20:37       DIR   .mozilla
    2016-02-16 11:28       DIR   .ssh
    2016-02-16 12:32        20   .bash_history
    2016-02-16 12:18        18   .bash_logout
    2016-02-16 11:28       193   .bash_profile
    2016-02-16 11:28       231   .bashrc
    2016-02-16 11:28       118   clusterware-setup-sshkey.log
    2016-02-16 11:28       865   .modulerc
    2016-02-16 11:28       719   .modules
    2016-02-16 12:52         0   mytestfile

Using Alces Storage with job scripts
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

The Alces Storage utility can also be used with cluster job scripts, to automatically place output data in the storage target and location of your choice. 

The following example uses a basic job script to demonstrate the functionality - from the cluster login node, create the following job script: 

.. code:: bash

    #!/bin/bash -l
    echo "Output of job $JOB_ID" > /tmp/output.$JOB_ID
    alces storage put /tmp/output.$JOB_ID job-scripts/results/

Run the job script - once complete you can see the output data of your job script using the ``alces storage list`` command, e.g.: 

.. code:: bash

    [alces@login1(cluster1) ~]$ qsub job-scripts/alces-storage.sh
    Your job 2 ("alces-storage.sh") has been submitted
    [alces@login1(cluster1) ~]$ alces storage list job-scripts/results
    2016-02-16 12:44         0   mpi-2nodes.output.1
    2016-02-16 13:18        16   output.2

Retrieving files from a POSIX storage target
--------------------------------------------

The Alces Storage utility can also retrieve files from a storage target, this is particularly useful for fetching data-sets from other locations and using it on compute nodes local scratch storage. 

To ``get`` a dataset from your ``/home/alces`` storaget target, use the ``alces storage get`` utility to place the dataset into the ``/tmp`` directory of the compute node. 

.. code:: bash

    [alces@node00(cluster1) tmp]$ alces storage list
    2016-02-16 12:51       DIR   .config
    2016-02-16 13:18       DIR   job-scripts
    2014-06-09 20:37       DIR   .mozilla
    2016-02-16 11:28       DIR   .ssh
    2016-02-16 12:32        20   .bash_history
    2016-02-16 12:18        18   .bash_logout
    2016-02-16 11:28       193   .bash_profile
    2016-02-16 11:28       231   .bashrc
    2016-02-16 11:28       118   clusterware-setup-sshkey.log
    2016-02-16 13:29 1073741824   dataset1
    2016-02-16 11:28       865   .modulerc
    2016-02-16 11:28       719   .modules
    2016-02-16 12:52         0   mytestfile
    2016-02-16 13:18       722   .viminfo
    [alces@node00(cluster1) tmp]$ alces storage get dataset1 /tmp/dataset1
    alces storage get: cluster1-home:dataset1 -> /tmp/dataset1

Once you have finished working with the ``dataset1`` file - it can be ``put`` back into a location of your choice, again using the Alces Storage command.

Deleting files
--------------

The Alces Storage utility can also be used to remove files from your storage targets. To remove a file from your storage target, run the following command - using subdirectories if required: 

.. code:: bash

    [alces@node00(cluster1) ~]$ alces storage list job-scripts/results
    2016-02-16 12:44         0   mpi-2nodes.output.1
    2016-02-16 13:18        16   output.2
    [alces@node00(cluster1) ~]$ alces storage rm job-scripts/results/output.2
    alces storage rm: deleted cluster1-home:job-scripts/results/output.2
