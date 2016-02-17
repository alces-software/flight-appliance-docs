.. _alces-storage-object-config:

Alces Storage Object Storage Configuration 
==========================================

The following section will detail how to configure object storage for use with the Alces Storage utility. 

Prerequisites
-------------

-  :ref:`Alces compute environment deployed <deployment>`
-  Access gained to deployed environment

Configuring your storage
------------------------
Once you have gained access to your Alces compute environment - you can begin configuring your object storage for use with the Alces Storage tool. 

To work with object storage, the Alces Storage utility must first enable the object storage type you wish to use, for example: 

-  For Dropbox storage, enable the ``dropbox`` storage type
-  For S3 compatible object storage including Ceph RadOS Gateway and Google Cloud Storage, enable the ``s3`` storage type

To enable storage types -- use the ``alces storage enable <type>`` command, e.g.: 

.. code:: bash

    [alces@login1(cluster1) ~]$ alces storage enable s3

Configuring S3 storage
----------------------
The following section will assist you to configure your S3-compatible object storage targets with the Alces Storage tool. 

The ``s3`` storage type will need to be enabled in order to configure object storage targets, enable the ``s3`` storage type with ``alces storage enable s3``. 

.. code:: bash

    [alces@login1(cluster1) ~]$ alces storage enable s3
    ######################################################################## 100.0%
      >>> alces service install: installed serviceware: base/s3cmd -> s3cmd
    alces storage enable: enabled storage type: base/s3 -> s3

S3 compatible object storage targets will usually have two keys associated with them: 

-  Access Key
-  Secret Key 
-  Service address (if using RadOS gateway/Google Cloud storage)

To configure an Amazon S3 storage target, run the ``alces storage configure`` utility - using your access key and secret key: 

.. code:: bash

    [alces@login1(cluster1) ~]$ alces storage configure alces-s3 s3
    Display name [alces-s3]:
    Access key: 123456789
    Secret key: 123456789
    Service address [s3.amazonaws.com]:
    alces storage configure: storage configuration complete

The newly configured S3 storage target will be available to your user: 

.. code:: bash

    [alces@login1(cluster1) ~]$ alces storage avail
    [ ] cluster1-data (posix)
    [ ] alces-s3 (s3)
    [*] cluster1-home (posix)

.. note:: You can set your default storage target to the ``alces-s3`` storage target using the ``alces storage use <config-name>`` command.


Configuring Dropbox storage
---------------------------
The following section will assist you to configure your Dropbox storage targets with the Alces Storage tool. 

The Dropbox storage type will need to be enabled in order to configure object storage targets, enable the ``dropbox`` storage type with ``alces storage enable dropbox``. 

To configure your Dropbox storage target, use the ``alces storage configure <name> dropbox`` command. The ``configure`` command will generate a URL for you to visit - and authorise access to your Dropbox account with. Once you have visited the URL, press Enter in your terminal prompt to finish configuration: 

.. code:: bash

    [alces@login1(cluster1) ~]$ alces storage configure alces-dropbox dropbox
    Display name [alces-dropbox]:
    Please visit the following URL in your browser and click 'Authorize':
    
      https://www.dropbox.com/1/oauth/authorize?oauth_token=Fm6tgisK5e7oJbDz
    
    Once you have completed authorization, please press ENTER to continue...
        
    Authorization complete.
    alces storage configure: storage configuration complete


The ``dropbox`` configuration will now be available for use: 

.. code:: bash

    [alces@login1(cluster1) ~]$ alces storage avail
    [ ] cluster1-data (posix)
    [*] alces-s3 (s3)
    [ ] cluster1-home (posix)
    [ ] alces-dropbox (dropbox)
    [alces@login1(cluster1) ~]$ alces storage -n alces-dropbox list
    2016-02-16 14:30     692088   Get Started with Dropbox.pdf

Working with object storage
---------------------------

For information on working with your object storage targets, please see the following guide: 

-  :ref:`alces-storage-object-usage`
