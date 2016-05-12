.. _customisation:

Cluster customisation
#####################

An Alces Flight Compute environment contains many useful configurations and tools, however there are cases where your environment may require customisation - this may include things such as;

* Distribution package installation
* External storage mounts

The Alces customiser tool requires some setup tasks in order to appropriately work with your AWS account and deployed Alces Flight Compute environments. 

Setup tasks
-----------

To begin setting up the Alces customiser tool - log in to your Alces Flight Compute environment as the administrator user. From there, run the ``alces about node`` command - this will display information about your environment: 

.. code:: bash

    [alces@login1(vaughan) ~]$ alces about node
    Clusterware release: 2016.2
    Customizer bucket prefix: s3://nmi0ztdmyzm3ztm3.alces-flight.com/customizer/default
    Platform host name: ec2-52-51-77-141.eu-west-1.compute.amazonaws.com
    Public IP address: 52.51.77.141
    Account hash: nmi0ztdmyzm3ztm3

Using the information provided - a new S3 bucket must be created using the prefix provided. Using one of the available tools, such as ``alces storage`` or ``s3cmd`` or the S3 web console - create a new bucket, for instance in the example the bucket would be called: 

    ``nmi0ztdmyzm3ztm3.alces-flight.com``

Once the bucket is created - create a "folder" within the bucket named: 

    ``customizer``

Now, from within the ``customizer`` folder - create another folder named ``default``, this sets up the ``default`` customiser profile. 

.. note:: It is important that the bucket and folders are created with the correct names - failing to create the bucket and folders with the correct names will mean the Alces customiser will not run

Your AWS account is now ready for use with the Alces customiser tool. 

Setting up customisation scripts
################################

Customisation scripts are run on each node in your environment upon entering the cluster ring - example customisation scripts include distribution package installations and external storage mounts. The Alces customiser supports any executable file type. 

The following simple example customisation shell script would install the ``emacs`` editor on each node within your environment: 

.. code:: bash

    #!/bin/bash
    yum -y install emacs

Once the bash script has been created - upload it to your S3 bucket into the ``default`` folder, for example: 

    ``s3://<account hash>.alces-flight.com/customizer/default/emacs.sh``

You can upload multiple customisation scripts to the ``default`` folder - each of the scripts will be run. 

The output of each customiser script run is sent to ``/var/log/clusterware/instance.log`` on each of the nodes. Each subsequently deployed Alces Flight Compute environment will run each of the customise scripts included in the ``default`` folder.
