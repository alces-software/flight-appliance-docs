.. _alces-sync:

.. warning:: The documentation here may not directly apply to your version of Flight, locate your version of Flight :ref:`here <index>`


Home Directory Synchronization
==============================

Alces Flight Compute is designed to provide users with a personal, ephemeral environment - allowing you to work when you need to, and shut down your cluster to free up resources when you're done. Flight includes a range of tools and utilities to help you setup your cluster quickly and easily, minimising the time taken to launch a new cluster and make it ready for use. The ``alces-sync`` utility helps users to manage the files and data you may typically store in your home directory such as job scripts and output data. The tool allows users to synchronise new clusters using data stored online in an S3 bucket, and to quickly and easily store their data when they've finished using a cluster before shutting it down. 


Getting started
---------------

When logging in to your Alces Flight Compute for the first time - the Alces synchronization tool will pull any data stored in the default location in your S3 bucket. For an AWS account that has either not used the Alces synchronization utility or not created an Alces Flight Compute environment before - no files will need to be synchronized.

To view the synchronization location of your current user, you can use the following command to displays both the local directory to be synced and its remote S3 location:

.. code:: bash

  [alces@login1(scooby) ~]$ alces sync list
  default: /home/alces <-> s3://alces-flight-nmi0ztdmyzm3ztm3/sync/alces/default

.. note:: The ``alces sync`` utility *does not* automatically push files and data back to the remote synchronization target, please view the documentation on how to ``push`` data back to the remote synchronization target.


Pushing data back to the remote location
----------------------------------------

The ``alces sync`` utility includes a tool to push any new data back to the remote synchronization target. The following example shows the process of adding some files and directories in your home directory, then pushing them back to the remote location. The command to use is ``alces sync push`` - which synchronizes any data in your home directory back to the remote synchronization target.

.. code:: bash

  [alces@login1(scooby) ~]$ mkdir job-scripts
  [alces@login1(scooby) ~]$ touch job-scripts/imb_2node.sh
  [alces@login1(scooby) ~]$ mkdir outputs
  [alces@login1(scooby) ~]$ for i in `seq 1 5`; do touch outputs/$i; done
  [alces@login1(scooby) ~]$ alces sync push

   > Synchronizing directory '/home/alces' to s3://alces-flight-nmi0ztdmyzm3ztm3
      Permissions ... OK
      Encryption passphrase (CTRL+C to skip): Skipping.
                 Sync ... OK

You can then verify that the synchronization was successful either using the S3 web console, or via the ``alces storage`` utility when configured for your S3 target:

.. code:: bash

  [alces@login1(scooby) ~]$ alces storage ls s3://alces-flight-nmi0ztdmyzm3ztm3/sync/alces/default/
  
  DIR   s3://alces-flight-nmi0ztdmyzm3ztm3/sync/alces/default/.config/
  DIR   s3://alces-flight-nmi0ztdmyzm3ztm3/sync/alces/default/.ssh/
  DIR   s3://alces-flight-nmi0ztdmyzm3ztm3/sync/alces/default/job-scripts/
  DIR   s3://alces-flight-nmi0ztdmyzm3ztm3/sync/alces/default/outputs/
  2016-09-06 13:52       358   s3://alces-flight-nmi0ztdmyzm3ztm3/sync/alces/default/.bash_history
  2016-09-06 13:47        18   s3://alces-flight-nmi0ztdmyzm3ztm3/sync/alces/default/.bash_logout
  2016-09-06 13:47       193   s3://alces-flight-nmi0ztdmyzm3ztm3/sync/alces/default/.bash_profile
  2016-09-06 13:47       231   s3://alces-flight-nmi0ztdmyzm3ztm3/sync/alces/default/.bashrc
  2016-09-06 13:47       334   s3://alces-flight-nmi0ztdmyzm3ztm3/sync/alces/default/.emacs
  2016-09-06 13:47       887   s3://alces-flight-nmi0ztdmyzm3ztm3/sync/alces/default/.modulerc
  2016-09-06 13:47       740   s3://alces-flight-nmi0ztdmyzm3ztm3/sync/alces/default/.modules
  2016-09-06 13:47       118   s3://alces-flight-nmi0ztdmyzm3ztm3/sync/alces/default/clusterware-setup-sshkey.log

.. hint:: When performing the ``alces sync push`` command - you may choose to set an encryption passphrase for additional protection.


Managing pushed files and data
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

You may not wish to push *all* of the files and data from a local synchronization target, particularly when working with your home directory which contains many one-time-use configuration files which may cause issues with any later deployed environments. 

Your home directory contains a Flight Compute configuration management folder, located at:

    ``~/.config/clusterware/``

Inside the configuration directory - there will be a configuration file for each synchronization target - for example: 

    ``sync.data.yml``       Configuration file for the ``data`` synchronization target

    ``sync.default.yml``    Configuration file for home directory synchronization

Open the ``sync.default.yml`` configuration file to add an example file exclusion. Below the ``:source: :home`` line - include a new section - e.g.

.. code:: yaml

  ---
  :source: :home
  :exclude:
  - ".modules"
  - ".ssh/id_*"

The above example would prevent the ``.modules`` file as well as any file matching the wildcard search ``id_*`` in the ``.ssh`` directory from being pushed to the remote synchronization target.

Encrypting data
~~~~~~~~~~~~~~~

The ``alces sync`` tool offers the choice to optionally set an encryption key on any uploaded data. This provides an extra layer of security for the data stored in your S3 buckets. A secure and most importantly memorable passphrase should be used - failing to remember your passphrase will prevent you from obtaining or using the data later on. 

When performing the ``alces sync push`` command - you will be prompted for an encryption passphrase, or optionally given the choice to skip using a passphrase by pressing **Ctrl+C**. 

A passphrase can contain any form of characters, just ensure the passphrase is either remembered or stored in a secure location for retrieval and access later on. 

The following example shows the ``alces sync push`` command being used, setting an encryption passphrase on the uploaded data: 

.. code:: bash

  [alces@login1(scooby) ~]$ alces sync push
  
   > Synchronizing directory '/home/alces' to s3://alces-flight-nmi0ztdmyzm3ztm3
       Permissions ... OK
   Encryption passphrase (CTRL+C to skip):
              Sync ... OK

When the ``alces sync`` tool retrieves data, either automatically on first login or manually via the ``alces sync pull`` command - you will be prompted to enter your encryption passphrase if one was previously set. Without the encryption passphrase, you will not be able to obtain or use your stored data.

Pulling files and data
----------------------

If you have pushed any new data from another running Alces Flight Compute environment, or manually uploaded files to the remote synchronization target - you may wish to pull those files to your current Alces Flight Compute environment. The following command displays the ``alces sync pull`` command retrieving remote data from the synchronization target:

.. code:: bash

  [alces@login1(scooby) ~]$ ls
  clusterware-setup-sshkey.log
  
  [alces@login1(scooby) ~]$ alces sync pull
   > Synchronizing directory '/home/alces' from s3://alces-flight-nmi0ztdmyzm3ztm3
              Sync ... OK
       Permissions ... OK
       
  [alces@login1(scooby) ~]$ ls
  clusterware-setup-sshkey.log  job-scripts  outputs


Adding and removing synchronization targets
-------------------------------------------

The following section details the process of adding and removing additional storage synchronization configurations.

Adding a synchronization configuration
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

In addition to the default home-directory synchronization configuration, you may wish to set up additional configurations to assist you with commonly used data directories.

The following example demonstrates how to set up a synchronization target for the local ``/data`` directory:

.. code:: bash

  [alces@login1(scooby) ~]$ alces sync add data /data
  alces sync add: created 'data' to sync '/data'
  
  [alces@login1(scooby) ~]$ alces sync list
  data: /data <-> s3://alces-flight-nmi0ztdmyzm3ztm3/sync/alces/data
  default: /home/alces <-> s3://alces-flight-nmi0ztdmyzm3ztm3/sync/alces/default

You can then synchronize any data stored in the ``/data`` directory using the ``alces sync push <name>`` command.

.. note:: By default, the ``alces sync push`` command will only push data to the default storage configuration - typically the users home directory. It is important to ensure you specify the storage configuration name when using the ``alces sync push`` command to manage additionally set up storage configurations.


Removing a synchronization configuration
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

You may wish to remove a storage synchronization configuration from your Alces Flight Compute environment - this can be achieved using the ``alces sync remove`` command - as demonstrated below:

.. code:: bash

  [alces@login1(scooby) ~]$ alces sync list
  data: /data <-> s3://alces-flight-nmi0ztdmyzm3ztm3/sync/alces/data
  default: /home/alces <-> s3://alces-flight-nmi0ztdmyzm3ztm3/sync/alces/default
  
  [alces@login1(scooby) ~]$ alces sync remove data
  Remove sync configuration for 'data' (Y/N)? y
  alces sync remove: removed 'data'

Wiping remote storage configuration targets
-------------------------------------------

In order to assist you with data management, the ``alces sync`` utility provides an easy method of removing all files and data stored within a remote synchronization target. To clear the contents of a remote storage target, use the ``alces sync purge <name>`` command as demonstrated below: 

.. code:: bash

  [alces@login1(scooby) ~]$ alces sync purge data
  Purge all files for 'data' at 's3://alces-flight-nmi0ztdmyzm3ztm3/sync/alces/data' (Y/N)? y
  alces sync purge: purged 'data'

.. warning:: Double check the remote location you are planning to wipe **does not** contain any important data before running the ``alces sync purge`` command.
