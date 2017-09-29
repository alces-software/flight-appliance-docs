.. _docker:

.. warning:: The documentation here may not directly apply to your version of Flight, locate your version of Flight :ref:`here <index>`


Using Gridware with Docker
##########################

Gridware can now be transported within a docker container which can be shared with the nodes in a Flight environment. This allows applications to be packaged within in a container for distribution between nodes or even to systems outside of the Flight environment. 

To install docker support, apply the feature profile (for more info on features see :ref:`feature-profiles`)::

    alces customize apply feature/configure-docker

.. note:: The feature profile will install the required docker dependencies and perform Flight specific configuration changes

Viewing Images
==============

The ``alces gridware docker`` command can be used to interact with docker images & containers once the feature has been applied to the system.

To show available docker images::

    alces gridware docker list

On a fresh build there won't be any ``Local`` images - but the ``Remote`` should display the ``docker.io/alces`` remote image repo which has the gridware base image as well as some application images that have already been built by Alces.

Building Images
===============

The image ``base`` from ``docker.io/alces`` will be used to build new images with the chosen gridware application. 

To build a gridware application as a docker image::

    alces gridware docker build apps/memtester/4.3.0
    
.. note:: Unlike standard gridware, this requires the version number to be specified even if there aren't multiple versions available

This will download the base image & run the additional commands required to install the application into the image. Once this has completed then the image will be visible in the ``Local`` image list.::

    Local:
      apps-memtester-4.3.0
      base

Running Containers
==================

When an image has been created it will only get started when it is interacted with using ``alces gridware docker run`` which launches a container built from the image. The containers can be interacted with in a couple of ways using this command, these methods are outlined below.

Running a Single Command
------------------------

Provide a single command as an argument to the run command after specifying the container to launch::

    [alces@login1(scooby) ~]$ alces gridware docker run apps-memtester-4.3.0 uptime
    Executing 'alces/gridware-apps-memtester-4.3.0' with arguments 'uptime'...

      >>>  15:46:02 up  1:40,  0 users,  load average: 0.20, 0.26, 0.23

    Job completed successfu8lly.

    Output summary:

    /home/alces/apps-memtester-4.3.0/work.6ff32278-2f4e-11e7-b988-0a6424937e45/output
      total 0
      drwx------ 2 alces alces  6 May  2 15:45 .
      drwx------ 3 alces alces 36 May  2 15:45 ..
      
.. note:: Any input files will need to be copied to ``~/app-memtester-4.3.0/input/`` to appear at ``/job/input/`` within the container. (Replace app-memtester-4.3.0 with the name of the container that gridware created)

Running a Script
----------------

Local scripts can be passed through to the container which allows for multiple commands to be ran at a time. Take the below script, ``~/script.sh``, which runs a few quick commands::

    #!/bin/bash
    uptime
    free -m
    echo $HOSTNAME 

This can be passed to a container using docker run as follows::

    [alces@login1(scooby) ~]$ alces gridware docker run apps-paraview-4.3.1 --script script.sh 
    Executing script 'script.sh' in 'alces/gridware-apps-paraview-4.3.1'...

      >>>  16:02:02 up  1:56,  0 users,  load average: 3.11, 2.40, 1.39
      >>>               total        used        free      shared  buff/cache   available
      >>> Mem:           7565         728         159          17        6676        6466
      >>> Swap:             0           0           0
      >>> 7ea913ce82a3

    Job completed successfully.

    Output summary:

    /home/alces/apps-paraview-4.3.1/work.ab7a1250-2f50-11e7-85da-0a6424937e45/output
      total 0
      drwx------ 2 alces alces  6 May  2 16:01 .
      drwx------ 3 alces alces 54 May  2 16:01 ..

Sharing Images
==============

.. important:: Sharing of images is not yet implemented in the 2017.1 Flight release!

In order for nodes to be able to use the same container that was built on the login node it will need to be shared.

Run the following command to add the local image to an NFS share that can be seen by the nodes::

    alces gridware docker share apps-memtester-4.3.0

.. note:: Any other systems that are to use the docker containers will need the docker feature enabled with ``alces customize apply feature/configure-docker``
