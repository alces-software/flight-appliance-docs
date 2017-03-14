.. _customisation:

Cluster customisation: AWS
##########################

An Alces Flight Compute environment contains many useful tool and utilities designed to assist users in running a diverse range of jobs and workflows. Also included are tools to help users customise their cluster environment further, allowing automated setup of a range of features including:

* Distribution package installation
* External storage mounts
* Custom script execution at launch time
 

The Alces customiser tool requires some setup tasks in order to appropriately work with your AWS account and deployed Alces Flight Compute environments. Follow the steps below to enable the customiser for your AWS account. 

.. _customisation-setup-tasks:

Setup tasks
-----------

To begin setting up the Alces customiser tool - log in to your Alces Flight Compute environment as the administrator user. From there, run the ``alces about node`` command - this will display information about your environment; e.g.

.. code:: bash

    [alces@login1(scooby) ~]$ alces about node
    Clusterware release: 2016.4.1
    Customizer bucket prefix: s3://alces-flight-a1i0ytdmvzv3ztv3/customizer/default
    Platform host name: ec2-52-51-77-141.eu-west-1.compute.amazonaws.com
    Public IP address: 52.51.77.141
    Account hash: a1i0ytdmvzv3ztv3

A new S3 bucket must be created using the prefix provided in the information above. Using one of the available tools, such as ``alces storage``, ``s3cmd`` or the S3 web console - create a new bucket with an appropriate name; for instance, in the example above the bucket would be called: 

    ``alces-flight-a1i0ytdmvzv3ztv3``

Once the bucket is created - create a "folder" within the bucket named: 

    ``customizer``

Now, from within the ``customizer`` folder - create another folder named ``default``, this sets up the ``default`` customiser profile. 

From within the ``default`` folder, you must create a folder for each customisation event you would like to add customisation scripts for; customisation scripts for this event will then be placed in this folder. See :ref:`customisation-events` for the different events available.

If you are unsure which event you should use, for most simple customisations such as installing additional system packages or creating new users you will probably want the ``configure`` event. In this case you would create a new folder within the ``default`` folder named:

    ``configure.d``

.. note:: It is important that the bucket and folders are created with the correct names - failing to create the bucket and folders with the correct names will mean that the Alces customiser will not be able to locate customisation information. 

Your AWS account is now ready for use with the Alces customiser tool. 

Using custom S3 buckets
-----------------------

You may also wish to use a custom S3 bucket rather than the automatically generated Flight bucket name. To do so, simply follow the above steps to create a bucket in the same location, changing ``default`` for a different identifier. For example, the following location could be created to hold customisation scripts for a specific environment:

  ``s3://alces-flight-bluecluster/customizer/default``
  

To use custom S3 buckets with Alces Flight Compute, enter your S3 bucket URL in the ``FlightCustomBuckets`` CloudFormation parameter, without the S3 prefix. For example, to launch a cluster using customisation scripts from the bucket in the above example, a user could specify the following value at launch time:

  ``FlightCustomBuckets: alces-flight-a1i0ytdmvzv3ztv3/customizer/default``

Setting up customisation scripts
--------------------------------

Customisation scripts are run on each node in your environment when the particular customisation event occurs - example customisation scripts include distribution package installations and external storage mounts. The Alces customiser supports any Linux executable file type.

The following simple example customisation shell script would install the ``emacs`` editor on each node within your environment: 

.. code:: bash

    #!/bin/bash
    yum -y install emacs

.. note:: Customisation scripts are run in unattended mode, and should be written to complete without interactive input.

Once the bash script has been created - upload it to your S3 bucket into the ``configure.d`` folder previously created, for example: 

    ``s3://alces-flight-<account hash>/customizer/default/configure.d/emacs.sh``

You can upload multiple customisation scripts to each event folder within the profile folder - each of the scripts will be run whenever the given event occurs.

The output of each customiser script run is sent to the file ``/var/log/clusterware/instance.log`` on each of the nodes; each output line will be prefixed with ``[cluster-customizer:<event>]``, identifying the event which produced it.

.. _customisation-events:

Customisation script environment
--------------------------------

Customisation scripts are run in the standard environment for whichever interpreter they are specified to be executed with, without loading any additional configuration. In particular this means that, in the case of customisation scripts intended to be run using Bash, configuration files for login shells are not loaded.

One consequence of this is that the ``alces`` command, which is defined as a shell function for cluster node login shells, is not available by default within customisation scripts. However, you can still run ``alces`` commands within your customisation scripts in either of the following ways:

1. Run the ``alces`` binary directly, which can be done like this:

.. code:: bash

  /opt/clusterware/bin/alces gridware depot install benchmark

2. Alternatively you can make the ``alces`` command available on your ``PATH``, which you may prefer if you want to run several ``alces`` commands. This can be done like this:

.. code:: bash

  PATH="/opt/clusterware/bin/:$PATH"
  alces gridware depot install benchmark

Customisation events
--------------------

A number of different customisation hooks are available to Flight Compute nodes when different events occur:

- ``initialize``: occurs at boot;
- ``configure``: occurs once the cluster configuration file ``/opt/clusterware/etc/config.yml`` file is detected (this is usually immediately available unless you are manually launching a cluster without using an Alces Flight CloudFormation template);
- ``start``: occurs once configure phase has completed (this event often starts services);
- ``node-started``: occurs once start complete (the node is ready);
- ``fail``: occurs should the cluster configuration file not be detected after 300 seconds;
- ``member-join``: occurs when a new node has joined the cluster (note: this event will also occur on the joining node itself);
- ``member-leave``: occurs when a node has left the cluster.

Customisation scripts can be added for each of these events by placing scripts within an appropriately named folder for the event (e.g. ``configure.d``, for scripts to run on the ``configure`` event), within a profile folder (e.g. ``default``, or see :ref:`customisation-alternate-profiles`), within the ``customizer`` folder of your S3 customisation bucket. See :ref:`customisation-setup-tasks` for full details of setting up customisation scripts.

Customisation script parameters
--------------------------------

Customisation scripts for each of the customisation events receive particular additional parameters, providing more information on the event and node, so that you can modify your script's behaviour based on these. These are as follows:

``initialize``
^^^^^^^^^^^^^^

If the node has not yet been configured (i.e. it is a clean boot, not a reboot), then the only parameter received is a literal ``once``. Otherwise no parameters are supplied.

``configure``, ``start``, ``fail``, ``node-started``
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

- 1: The name of the event being run (allowing a single script to be reused for multiple events), e.g. ``configure``.
- 2: The role of the instance, i.e. ``master`` (login/head node) or ``slave`` (compute node).
- 3: The name of the cluster (allowing a single script to behave differently for particularly named clusters).

``member-join``, ``member-leave``
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

As above, plus:

- 4: Path to a file that contains information about the member.
- 5: Hostname of the member.
- 6: IP address of the member.

.. _customisation-alternate-profiles:

Using alternate customisation profiles
--------------------------------------

Alternate customisation profiles can be set up and used with the Alces customiser tool. To set up another profile, from your S3 bucket in the ``customizer`` folder - create another profile folder, for example ``foo``

Within the ``foo`` folder - create folders for the customisation events you want to handle, e.g. create a ``configure.d`` folder. Place any ``configure`` customisation scripts for the ``foo`` profile within the ``configure.d`` folder.

To use custom profiles when launching the Alces Flight Compute CloudFormation templates, enter the profile name(s) in the ``FlightCustomProfiles`` parameter - the customiser tool will then run each of the scripts in the ``foo`` profile. 
