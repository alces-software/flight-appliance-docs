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
===========

.. _customisation-storage-config:

Storage Configuration
---------------------

To begin setting up the Alces customiser tool - log in to your Alces Flight Compute environment as the administrator user. From there, run the ``alces about customizer`` command - this will display information about your environment; e.g.

.. code:: bash

    [alces@login1(scooby) ~]$ alces about customizer
     Customizer bucket prefix: s3://alces-flight-a1i0ytdmvzv3ztv3/customizer

A new S3 bucket must be created using the prefix provided in the information above. Using one of the available tools, such as ``alces storage``, ``s3cmd`` or the S3 web console - create a new bucket with an appropriate name; for instance, in the example above the bucket would be called:: 

    alces-flight-a1i0ytdmvzv3ztv3

Once the bucket is created - create a "folder" within the bucket named:: 

    customizer

Now, from within the ``customizer`` folder - create another folder named ``default``, this sets up the ``default`` customiser profile. 

From within the ``default`` folder, you must create a folder for each customisation event you would like to add customisation scripts for; customisation scripts for this event will then be placed in this folder. See :ref:`customisation-events` for the different events available.

If you are unsure which event you should use, for most simple customisations such as installing additional system packages or creating new users you will probably want the ``configure`` event. In this case you would create a new folder within the ``default`` folder named::

    configure.d

.. note:: It is important that the bucket and folders are created with the correct names - failing to create the bucket and folders with the correct names will mean that the Alces customiser will not be able to locate customisation information. 

Your AWS account is now ready for use with the Alces customiser tool.

Setting up Customisation Scripts
--------------------------------

Customisation scripts are run on each node in your environment when the particular customisation event occurs - example customisation scripts include distribution package installations and external storage mounts. The Alces customiser supports any Linux executable file type.

The following simple example customisation shell script would install the ``emacs`` editor on each node within your environment: 

.. code:: bash

    #!/bin/bash
    yum -y install emacs

.. note:: Customisation scripts are run in unattended mode, and should be written to complete without interactive input.

Once the bash script has been created - upload it to your S3 bucket into the ``configure.d`` folder previously created, for example:: 

    s3://alces-flight-<account hash>/customizer/default/configure.d/emacs.sh

You can upload multiple customisation scripts to each event folder within the profile folder - each of the scripts will be run whenever the given event occurs.

The output of each customiser script run is sent to the file ``/var/log/clusterware/instance.log`` on each of the nodes; each output line will be prefixed with ``[cluster-customizer:<event>]``, identifying the event which produced it.

Customisation Script Environment
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

.. _customisation-apply-methods:

Applying Customization Profiles
===============================

There are 3 ways in which a feature profile can be applied to a system. Each method applies the profile in a slightly different way.

At Formation
------------

To apply profiles when launching the Alces Flight Compute CloudFormation templates, enter the profile name(s) in the ``Customization profiles to enable`` parameter - the customiser tool will then run each of the scripts in the ``foo`` profile. This will apply the profile to all systems (both the login & compute nodes) when they boot up.

If the profile(s) you wish to use are in a different storage container than the default, see :ref:`customisation-custom-bucket`.

.. note:: In order to use multiple profiles, separate them with a space in the ``Customization profiles to enable`` parameter. (e.g. ``foo default``)

.. _customisation-apply-manual:

Using Customize Apply
---------------------

The profile can be applied to live systems with ``alces customize apply account/foo`` which will execute the profile on the current machine.

To apply the profile to all nodes run: ::

    module load services/pdsh && pdsh -g nodes "alces customize apply account/foo"
    
.. note:: This profile will not be added to any default location so any nodes brought up with autoscaling will *not* have the profile applied

Using Customize Slave Apply
---------------------------

To set a profile to be run on all compute nodes when brought up (with autoscaling), add the profile to the slave list from the login node with: ::

    alces customize slave add account/foo

The profile can then be seen by running the `list` command from the headnode: :: 

    [alces@login1(scooby) ~]$ alces customize slave list
    account/foo

This profile can be removed from the slave list with: ::

    alces customize slave remove account/foo

.. note:: This will *not* apply the profile to any currently running compute nodes. To apply it to any running nodes see :ref:`customisation-apply-manual`

Script Events & Parameters
==========================

.. _customisation-events:

Customisation Events
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

Customisation Script Parameters
-------------------------------

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

.. _customisation-custom-bucket:

Using Custom S3 Buckets
=======================

You may also wish to use a custom S3 bucket rather than the automatically generated Flight bucket name. To do so, simply follow the steps at  :ref:`customisation-storage-config` to create a bucket in the same location, changing the bucket name for a different identifier. For example, the following location could be created to hold customisation scripts for a specific environment:

  ``s3://alces-flight-bluecluster/customizer/default``

To use custom S3 buckets with Alces Flight Compute, enter your S3 bucket URL in the ``S3 bucket for customization profiles`` CloudFormation parameter, without the S3 prefix. For example, to launch a cluster using customisation scripts from the bucket in the above example, a user could specify the following value at launch time:

  ``S3 bucket for customization profiles: alces-flight-a1i0ytdmvzv3ztv3``

.. _customisation-alternate-profiles:

Alternate Customisation Profiles
================================

Creating Alternate Profile
--------------------------

Below are 2 different methods for setting up a custom profile. The first details the steps taken to setup a custom profile from within a live stack and pushing it to the storage repository, the second details creating the profile before bringing up a Flight stack.

From Live System
^^^^^^^^^^^^^^^^

There are few bits of trickery needed to configure a new profile from the command line. It's recommended to do this from the login node and push to the storage repository upon completion.

- Create a temporary file named after the desired profile name (``foo`` is used for this example)::

    touch foo.sh

- Create profile in the accounts repository (this will generate the ``manifest.txt`` file & ``foo/configure.d/foo.sh`` filesystem structure within the default customizer location [`s3://alces-flight-<account hash>/customizer/`])::

    alces customize push foo.sh account

- Download foo repository::

    alces customize apply account/foo

- Add new scripts & make changes to the profile in ``s3://alces-flight-<account hash>/customizer/foo/`` through AWS (for more info on different stages see :ref:`customisation-events`)

- Download new scripts from the repo and run them::

    alces customize apply account/foo

Before CloudFormation
^^^^^^^^^^^^^^^^^^^^^

Alternate customisation profiles can be set up from the S3 customizer bucket (e.g. ``s3://alces-flight-a1i0ytdmvzv3ztv3/customizer/``). To set up another profile, from your S3 bucket in the ``customizer`` folder - create another profile folder, for example ``foo``.

Within the ``foo`` folder:

- Create folders for the customisation events you want to handle (e.g. create a ``configure.d`` folder. Place any ``configure`` customisation scripts for the ``foo`` profile within the ``configure.d`` folder)

- Create a file called ``manifest.txt`` (in the ``foo`` directory) which lists all of your customisation scripts as below::

    start.d/script.sh
    configure.d/emacs.sh
    configure.d/test.sh

Using Alternate Profile
-----------------------

See :ref:`customisation-apply-methods`

.. _feature-profiles:

Feature Profiles
================

Feature profiles are available to add further functionality to a Flight environment. These features can be schedulers, applications or environment tweaks. One of the main benefits of feature profiles is that it allows for the automatic installation of commercial software that cannot be distributed with Alces Flight & gridware. Feature profiles also allow for the automation of complex installations which are out of the scope of gridware.

To show available features profiles::

    [alces@login1(scooby) ~]$ alces customize avail
    feature/ansys-fluent-v170          software
    feature/configure-beegfs           software
    feature/configure-docker           software
    feature/configure-ephemeral-disks  config
    feature/configure-users            config
    feature/disable-hyperthreading     config
    feature/ellexusmistral             software
    feature/enginframe-2015.1          software
    feature/enginframe-2017.0          software

.. note:: The above output has been shortened to save space on the documentation, there are many more features available on a running Flight instance

Installing a Feature Profile
----------------------------

Installing a new feature profile is done in much the same way as applying an alternate customisation profile. 

Apply customisation feature::

    alces customize apply feature/ansys-fluent-v170
    
If any of the files for the installation are missing then an error message similar to the following will be displayed::

    Running event hooks for ansys-fluent-v170
    Running configure hook: /opt/clusterware/var/lib/customizer/feature-ansys-fluent-v170/configure.d/install.sh
    download: 's3://alces-flight-<account hash>/apps/ansys-fluent/FLUIDS_170_LINX64.tar' -> '/tmp/FLUIDS_170_LINX64.tar'  [1 of 1]
    download: 's3://alces-flight-<account hash>/apps/ansys-fluent/FLUIDS_170_LINX64.tar' -> '/tmp/FLUIDS_170_LINX64.tar'  [1 of 1]
    ERROR: S3 error: 404 (Not Found)
    download: 's3://alces-flight-<account hash>/apps/ansys-fluent/fluent-license.lic' -> '/tmp/fluent-license.lic'  [1 of 1]
    download: 's3://alces-flight-<account hash>/apps/ansys-fluent/fluent-license.lic' -> '/tmp/fluent-license.lic'  [1 of 1]
    ERROR: S3 error: 404 (Not Found)
    ******************************************************************************

      Please download the Fluent installation file(s) from the
      download link provided by Ansys.

      If you do not have a download link, please contact Ansys support at
      info@ansys.com.

      Copy the installation archive and license file to /tmp on
      this machine:

        /tmp/FLUIDS_170_LINX64.tar
        /tmp/fluent-license.lic

      For repeatable installation, upload to your Alces Flight S3
      customization bucket:

        s3://alces-flight-<account hash>/apps/ansys-fluent/FLUIDS_170_LINX64.tar
        s3://alces-flight-<account hash>/apps/ansys-fluent/fluent-license.lic

      Once you have placed the files in one of these locations, run the
      following command:

        alces customize trigger configure feature-ansys-fluent

      Installation will then continue.

    ******************************************************************************
    No start hooks found in /opt/clusterware/var/lib/customizer/feature-ansys-fluent-v170
    No node-started hooks found in /opt/clusterware/var/lib/customizer/feature-ansys-fluent-v170
    No member-join hooks found in /opt/clusterware/var/lib/customizer/feature-ansys-fluent-v170
    No member-join hooks found in /opt/clusterware/var/lib/customizer/feature-ansys-fluent-v170

.. tip:: To make the installation files available for any Flight instances on the same AWS account - save them to ``s3://alces-flight-<account hash>/apps/app-name/`` instead of ``/tmp``. Where ``alces-flight-<account-hash>`` is the name of the bucket in ``alces about customizer`` and ``app-name`` is the name of the app feature minus the version number (in the example above, ``ansys-fluent-v170`` would become ``ansys-fluent``). The names of the files should match those mentioned in the error output.

Once the required files are in place the installation will run through (this example shows the feature profile being applied after the files have been added to S3)::

    Running event hooks for ansys-fluent-v170
    Running configure hook: /opt/clusterware/var/lib/customizer/feature-ansys-fluent-v170/configure.d/install.sh
    download: 's3://alces-flight-<account-hash>/apps/ansys-fluent/FLUIDS_170_LINX64.tar' -> '/tmp/FLUIDS_170_LINX64.tar'  [1 of 1]
    download: 's3://alces-flight-<account-hash>/apps/ansys-fluent/FLUIDS_170_LINX64.tar' -> '/tmp/FLUIDS_170_LINX64.tar'  [1 of 1]
     7125760000 of 7125760000   100% in   82s    82.21 MB/s  done
    download: 's3://alces-flight-<account-hash>/apps/ansys-fluent/fluent-license.lic' -> '/tmp/fluent-license.lic'  [1 of 1]
    download: 's3://alces-flight-<account-hash>/apps/ansys-fluent/fluent-license.lic' -> '/tmp/fluent-license.lic'  [1 of 1]
     26 of 26   100% in    0s   925.40 B/s  done
    Unpacking tarball FLUIDS_170_LINX64.tar ...
    Starting Ansys Fluent installer...
    cp: cannot stat ‘/tmp/fluent-license.lic’: No such file or directory
    Installing and Configuring modulefiles...
    Ansys Fluent installation completed.

    No start hooks found in /opt/clusterware/var/lib/customizer/feature-ansys-fluent-v170
    No node-started hooks found in /opt/clusterware/var/lib/customizer/feature-ansys-fluent-v170
    No member-join hooks found in /opt/clusterware/var/lib/customizer/feature-ansys-fluent-v170
    No member-join hooks found in /opt/clusterware/var/lib/customizer/feature-ansys-fluent-v170

.. note:: If the required files have only been uploaded to S3 then the installation will take a little longer while it copies the data locally to the login node
