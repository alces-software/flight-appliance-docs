.. _alces-storage-object-usage:

Alces Storage Object Storage Usage
==================================

The following section details how to use your :ref:`previously set up <alces-storage-object-config>` object storage targets. 

The Alces Storage utility provides several functions when interacting with your storage targets, including:

-  Listing files and directories within a storage target
-  Adding/putting files and directories within a storage target
-  Retrieving/getting files and directories within a storage target
-  Deleting files and directories from within a storage target

The Alces Storage utility provides seamless use of Dropbox and S3 storage targets, allowing you to use the following guide for both S3 and Dropbox storage usage. 

As the ``alces`` administrator user - select one of your previously set up object storage targets, e.g. ``alces storage use alces-dropbox``

Bucket usage
------------
Object storage does not use "folders" - instead we refer to them as buckets. The Alces Storage tool allows you to make buckets for storing data in, which may help you organise your object data. 

Adding buckets
^^^^^^^^^^^^^^
To add a "bucket" -- use the ``alces storage mkbucket <name>`` command, for example: 

.. code:: bash

    [alces@login1(cluster1) ~]$ alces storage mkbucket backup-data
    alces storage mkbucket: created bucket backup-data
    [alces@login1(cluster1) ~]$ alces storage list
    2016-02-16 14:49        DIR   backup-data
    2016-02-16 14:30     692088   Get Started with Dropbox.pdf

Removing buckets
^^^^^^^^^^^^^^^^
To remove a bucket from your object storage target, including all of its contents - use the ``alces storage rmbucket <name>`` command, for example: 

.. code:: bash

    [root@login1(cluster1) ~]# alces storage rmbucket backup-data
    alces storage rmbucket: removed bucket backup-data

Viewing files in a storage target
---------------------------------
The ``alces storage list`` command can be used to list the top-level of a storage target, in this case the ``alces-dropbox`` target - this will display all objects located in your Dropbox account:

.. code:: bash

    [alces@login1(cluster1) ~]$ alces storage list
    2016-02-16 14:30     692088   Get Started with Dropbox.pdf

The ``list`` command can also be used to navigate buckets within the target, for example: 

.. code:: bash

    [alces@login1(cluster1) ~]$ alces storage list backup-data/
    2016-02-16 15:14          0   testfile 

Adding files to an object storage target
----------------------------------------
The Alces Storage utility can be used to add files to your object storage targets - this can be used in some of the following ways: 

-  Manually via CLI tools
-  Automatically through scheduler batch scripts

The following examples will detail some of the many possible uses of the ``alces storage put`` utility. 

Manually adding files
^^^^^^^^^^^^^^^^^^^^^

The ``alces storage`` utility can be used from any of the available compute nodes within your environment. For this example - we will work from the login node of the cluster. First - generate a file to upload to your object storage target. 

An example 10MB file can be generated using the ``dd`` utility: 

.. code:: bash

    [alces@login1(cluster1) ~]$ dd if=/dev/zero of=data1.input bs=1M count=10
    10+0 records in
    10+0 records out
    10485760 bytes (10 MB) copied, 0.012279 s, 854 MB/s

We can now upload the ``data1.input`` file to our object storage target using the ``alces storage put <source>`` utility: 

.. code:: bash

    [root@login1(cluster1) ~]# alces storage put data1.input
    alces storage put: data1.input -> data1.input
    [root@login1(cluster1) ~]# alces storage list
    2016-02-16 14:49        DIR   backup-data
    2016-02-16 15:32   10485760   data1.input
    2016-02-16 14:30     692088   Get Started with Dropbox.pdf

Using Alces Storage with job scripts
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

The Alces Storage utility can also be used with cluster job scripts, auto automatically upload output data to your object storage targets for safe-keeping and archival. 

The following example uses a basic job script to demonstration the functionality - from the cluster login node, create the following job script: 

.. code:: bash

    #!/bin/bash -l
    dd if=/dev/zero of=/tmp/output.$JOB_ID bs=1M count=10
    alces storage put /tmp/output.$JOB_ID

Run the job script - once the job has finished, you should see your output data located in your object storage target: 

.. code:: bash

    [alces@login1(cluster1) ~]$ alces storage list
    2016-02-16 14:49        DIR   backup-data
    2016-02-16 15:34   10485760   data1.input
    2016-02-16 14:30     692088   Get Started with Dropbox.pdf
    2016-02-16 15:42   10485760   output.2

Retrieving files from an object storage target
----------------------------------------------

The Alces Storage utility can also retrieve files from an object storage target, this is particularly useful for fetching data-sets from other locations and using it on compute nodes local scratch storage. 

To ``get`` a dataset from your object storaget target, use the ``alces storage get`` utility to place the dataset into your current working directory: 

.. code:: bash

    [alces@login1(cluster1) ~]$ alces storage get data1.input
    alces storage get: data1.input -> /home/alces/data1.input
    [alces@login1(cluster1) ~]$ ls
    alces-storage.sh  data1.input  testfile 

.. note:: The ``alces storage get`` command can also be used in job scripts, similar to previously using the ``put`` function. 

Deleting files
--------------

The Alces Storage utility can also be used to remove files from your storage targets. To remove a file from your storage target, run the following command - using subdirectories if required: 

.. code:: bash

    [root@login1(cluster1) ~]# alces storage rm backup-data/testfile
    alces storage rm: deleted backup-data/testfile
