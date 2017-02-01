.. _customisation:

Cluster customisation: AWS
##########################

An Alces Flight Compute environment contains many useful tool and utilities designed to assist users in running a diverse range of jobs and workflows. Also included are tools to help users customise their cluster environment further, allowing automated setup of a range of features including:

* Distribution package installation
* External storage mounts
* Custom script execution at launch time
 

The Alces customiser tool requires some setup tasks in order to appropriately work with your AWS account and deployed Alces Flight Compute environments. Follow the steps below to enable the customiser for your AWS account. 

Setup tasks
-----------

To begin setting up the Alces customiser tool - log in to your Alces Flight Compute environment as the administrator user. From there, run the ``alces about node`` command - this will display information about your environment; e.g.

.. code:: bash

    [alces@login1(scooby) ~]$ alces about node
    Clusterware release: 2016.4.1
    Customiser bucket prefix: s3://alces-flight-a1i0ytdmvzv3ztv3/customiser/default
    Platform host name: ec2-52-51-77-141.eu-west-1.compute.amazonaws.com
    Public IP address: 52.51.77.141
    Account hash: a1i0ytdmvzv3ztv3

A new S3 bucket must be created using the prefix provided in the information above. Using one of the available tools, such as ``alces storage``, ``s3cmd`` or the S3 web console - create a new bucket with an appropriate name; for instance, in the example above the bucket would be called: 

    ``alces-flight-a1i0ytdmvzv3ztv3``

Once the bucket is created - create a "folder" within the bucket named: 

    ``customiser``

Now, from within the ``customiser`` folder - create another folder named ``default``, this sets up the ``default`` customiser profile. 

From within the ``default`` folder, we must create a folder where all customisation scripts should be placed. Create a new folder within the ``default`` folder named:

    ``configure.d``

.. note:: It is important that the bucket and folders are created with the correct names - failing to create the bucket and folders with the correct names will mean that the Alces customiser will not be able to locate customisation information. 

Your AWS account is now ready for use with the Alces customiser tool. 

Using custom S3 buckets
-----------------------

You may also wish to use a custom S3 bucket rather than the automatically generated Flight bucket name. To do so, simply follow the above steps to create a bucket in the same location, changing ``default`` for a different identifier. For example, the following location could be created to hold customisation scripts for a specific environment:

  ``s3://alces-flight-bluecluster/customiser/default``
  

To use custom S3 buckets with Alces Flight Compute, enter your S3 bucket URL in the ``FlightCustomBuckets`` CloudFormation parameter, without the S3 prefix. For example, to launch a cluster using customisation scripts from the bucket in the above example, a user could specify the following value at launch time:

  ``FlightCustomBuckets: alces-flight-a1i0ytdmvzv3ztv3/customiser/default``

Setting up customisation scripts
--------------------------------

Customisation scripts are run on each node in your environment upon joining the cluster network - example customisation scripts include distribution package installations and external storage mounts. The Alces customiser supports any Linux executable file type. 

The following simple example customisation shell script would install the ``emacs`` editor on each node within your environment: 

.. code:: bash

    #!/bin/bash
    yum -y install emacs

.. note:: Customisation scripts are run in unattended mode, and should be written to complete with interactive input. 

Once the bash script has been created - upload it to your S3 bucket into the ``configure.d`` folder previously created, for example: 

    ``s3://alces-flight-<account hash>/customiser/default/configure.d/emacs.sh``

You can upload multiple customisation scripts to the ``default`` folder - each of the scripts will be run. 

The output of each customiser script run is sent to the file ``/var/log/clusterware/instance.log`` on each of the nodes. Each subsequently deployed Alces Flight Compute environment will run each of the customise scripts included in the ``default`` folder.

Using alternate customisation profiles
--------------------------------------

Alternate customisation profiles can be set up and used with the Alces customiser tool. To set up another profile, from your S3 bucket in the ``customiser`` folder - create another profile folder, for example ``foo``

Within the ``foo`` folder - create the ``configure.d`` folder. Place any customisation scripts for the ``foo`` profile within the ``configure.d`` folder. 

To use custom profiles when launching the Alces Flight Compute CloudFormation templates, enter the profile name(s) in the ``FlightCustomProfiles`` parameter - the customiser tool will then run each of the scripts in the ``foo`` profile. 
