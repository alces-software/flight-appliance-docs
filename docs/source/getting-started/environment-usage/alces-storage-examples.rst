.. _alces-storage-examples:

Alces Storage Usage Examples
============================

The following guide will detail some of the uses of the :ref:`Alces Storage<alces-storage-overview>` utility within your compute jobs. As the Alces Storage utility can interact with multiple storage platforms - fetching archived reference data from external sources is made simple. 

Prerequisites
-------------

The following guide assumes you have correctly set up the desired storage targets using the ``alces storage configure`` tool, as well as the ``apps/fah`` Gridware package installed.

Example usage 
-------------

The following example job script uses a previously setup S3 target, which hosts our Folding@Home configuration file. The job script will pull the configuration file, run the Folding@Home application; then upload the output data to the S3 storage target: 

.. code:: bash

    #!/bin/bash -l
    #$ -pe smp-verbose 2
    #$ -N fah -o $HOME/fah.$JOB_ID.out
    module load apps/fah
    alces storage get -n alces-s3 s3://alces-data/client.cfg $HOME/client.cfg
    fah6 -smp 2 -advmethods
    alces storage put -n alces-s3 $HOME/FAHlog.txt s3://alces-data/output/FAHlog.txt

Further usage
-------------

The Alces Storage utility can be used in conjunction with your applications to pull public datasets as well as your own reference data from multiple types of storage targets including S3, Google Cloud Storage and Dropbox. 
