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



Viewing files in a storage target
---------------------------------
The ``alces storage list`` command can be used to list the top-level of a storage target, in this case the ``alces-dropbox`` target - this will display all objects located in your Dropbox account:

.. code:: bash

    [alces@login1(cluster1) ~]$ alces storage list
    2016-02-16 14:30     692088   Get Started with Dropbox.pdf

The ``list`` command can also be used to navigate folders within the target, for example 
