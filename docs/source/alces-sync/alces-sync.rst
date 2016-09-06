.. alces-sync:

Home Directory Synchronization
==============================

When interacting with multiple Alces Flight Compute clusters in the Cloud, it can be tricky manage your data - particularly files and data you may typically store in your home directory such as job scripts and output data.

In order to facilitate the management and ease of access over your most used files and data - your Alces Flight Compute environment includes a utility to aid you with automatically synchronizing data from an S3 bucket.

Getting started
---------------

When logging in to your Alces Flight Compute for the first time - the Alces synchronization tool will pull any data stored in the default location in your S3 bucket, however for an AWS account that has either not used the Alces synchronization utility or not created an Alces Flight Compute environment before - no files will need to be synchronized.

To view the synchronization location of your current user - you can use the following command, which displays the local directory to be synced - as well as its remote S3 location:

.. code:: bash

  [alces@login1(vlj) ~]$ alces sync list
  default: /home/alces <-> s3://alces-flight-nmi0ztdmyzm3ztm3/sync/alces/default

.. note:: The ``alces sync`` utility *does not* automatically push files and data back to the remote synchronization target, please view the documentation on how to ``push`` data back to the remote synchronization target

Pushing data back to the remote location
----------------------------------------

The ``alces sync`` utility includes a tool to push any and all new data back to the remote synchronization target. The following example shows the process of adding some files and directories in your home directory, then pushing them back to the remote synchronization target. The command to use is ``alces sync push`` - which synchronizes any data in your home directory back to the remote synchronization target.

.. code:: bash

  [alces@login1(vlj) ~]$ mkdir job-scripts
  [alces@login1(vlj) ~]$ touch job-scripts/imb_2node.sh
  [alces@login1(vlj) ~]$ mkdir outputs
  [alces@login1(vlj) ~]$ for i in `seq 1 5`; do touch outputs/$i; done
  [alces@login1(vlj) ~]$ alces sync push

   > Synchronizing directory '/home/alces' to s3://alces-flight-nmi0ztdmyzm3ztm3
      Permissions ... OK
      Encryption passphrase (CTRL+C to skip): Skipping.
                 Sync ... OK

You can then verify that the synchronization was successful either using the S3 web console, or via the ``s3cmd`` utility with appropriate authentication present:

.. code:: bash

  s3cmd ls s3://alces-flight-nmi0ztdmyzm3ztm3/sync/alces/default/
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

.. hint:: When performing the ``alces sync push`` command - you may choose to set an encryption passphrase for additional protection

Pulling files and data
----------------------

If you have pushed any new data from another running Alces Flight Compute environment, or manually uploaded files to the remote synchronization target - you may wish to pull those files to your current Alces Flight Compute environment. The following command displays the ``alces sync pull`` command retrieving remote data from the synchronization target:

.. code:: bash

  [alces@login1(vlj) ~]$ ls
  clusterware-setup-sshkey.log
  [alces@login1(vlj) ~]$ alces sync pull
   > Synchronizing directory '/home/alces' from s3://alces-flight-nmi0ztdmyzm3ztm3
              Sync ... OK
       Permissions ... OK
  [alces@login1(vlj) ~]$ ls
  clusterware-setup-sshkey.log  job-scripts  outputs

Adding and removing synchronization targets
-------------------------------------------

The following section details the process of adding and removing additional storage synchronization configurations.

Adding a synchronization configuration
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

In addition to the default home-directory synchronization configuration, you may wish to set up additional configurations to assist you with commonly used data directories.

The following example demonstrates how to set up a synchronization target for the local ``/data`` directory:

.. code:: bash

  [alces@login1(vlj) ~]$ alces sync add data /data
  alces sync add: created 'data' to sync '/data'
  [alces@login1(vlj) ~]$ alces sync list
  data: /data <-> s3://alces-flight-nmi0ztdmyzm3ztm3/sync/alces/data
  default: /home/alces <-> s3://alces-flight-nmi0ztdmyzm3ztm3/sync/alces/default

You can then synchronize any data stored in the ``/data`` directory using the ``alces sync push <name>`` command.

.. note:: By default, the ``alces sync push`` command will only push data to the default storage configuration - typically the users home directory. It is important to ensure you specify the storage configuration name when using the ``alces sync push`` command to manage additionally set up storage configurations

Removing a synchronization configuration
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

You may wish to remove a storage synchronization configuration from your Alces Flight Compute environment - this can be achieved using the ``alces sync remove`` command - as demonstrated below:

.. code:: bash

  [alces@login1(vlj) ~]$ alces sync list
  data: /data <-> s3://alces-flight-nmi0ztdmyzm3ztm3/sync/alces/data
  default: /home/alces <-> s3://alces-flight-nmi0ztdmyzm3ztm3/sync/alces/default
  [alces@login1(vlj) ~]$ alces sync remove data
  Remove sync configuration for 'data' (Y/N)? y
  alces sync remove: removed 'data'

Wiping remote storage configuration targets
-------------------------------------------

In order to assist you with data management, the ``alces sync`` utility provides an easy method of removing all files and data stored within a remote synchronization target. To clear the contents of a remote storage target, use the ``alces sync purge <name>`` command as demonstrated below: 

.. code:: bash

  [alces@login1(vlj) ~]$ alces sync purge data
  Purge all files for 'data' at 's3://alces-flight-nmi0ztdmyzm3ztm3/sync/alces/data' (Y/N)? y
  alces sync purge: purged 'data'

.. warning:: Double check the remote location you are planning to wipe **does not** contain any important data before running the ``alces sync purge`` command
