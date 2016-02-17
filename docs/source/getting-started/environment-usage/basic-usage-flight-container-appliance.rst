.. _basic-usage-flight-container-appliance:

Overview
========
The Alces Flight Container Appliance allows you to easily get started with containerised computing in an Alces Flight environment. 

Prerequisites
-------------

-  `Alces Flight Container Appliance deployed <manual-deploy-flight-container-appliance>`__
-  Access to Alces Flight Container Appliance gained

Basic Docker Usage
------------------
The ``alces`` administrator user belongs to the ``docker`` system group, enabling sudo-less Docker commands. Once access has been gained to your Alces Flight Container Appliance, use the ``docker ps -a`` command to verify that Docker is successfully working: 

.. code:: bash

    [alces@master1(docker) ~]$ docker ps
    CONTAINER ID        IMAGE               COMMAND             CREATED             STATUS              PORTS               NAMES

The ``docker ps`` command displays a list of running, and non-running Docker containers on your current machine. 

Starting a container
--------------------
Docker containers can run interactively - similar to working with a virtual machine, run an application or run in daemonized mode. The following section will detail running both in interactive mode as well as running an example application through Docker. 

When running a Docker container, it will check locally for the Docker image first - if it does not exist locally, the image name will be searched in Docker Hub. 

Running an application
^^^^^^^^^^^^^^^^^^^^^^
The ``whalesay`` Docker image runs an application which prints the users choice of words, together with an ASCII whale. 

As the ``alces`` user on your Alces Flight Docker Appliance, run the ``whalesay`` application: 

.. code:: bash

    [alces@master1(docker) ~]$ docker run docker/whalesay cowsay Alces Software
    Unable to find image 'docker/whalesay:latest' locally
    latest: Pulling from docker/whalesay
    e190868d63f8: Pull complete
    909cd34c6fd7: Pull complete
    0b9bfabab7c1: Pull complete
    a3ed95caeb02: Pull complete
    00bf65475aba: Pull complete
    c57b6bcc83e3: Pull complete
    8978f6879e2f: Pull complete
    8eed3712d2cf: Pull complete
    Digest: sha256:178598e51a26abbc958b8a2e48825c90bc22e641de3d31e18aaf55f3258ba93b
    Status: Downloaded newer image for docker/whalesay:latest
     ________________
    < Alces Software >
     ----------------
        \
         \
           \
                         ##        .
                   ## ## ##       ==
                ## ## ## ##      ===
           /""""""""""""""""___/ ===
      ~~~ {~~ ~~~~ ~~~ ~~~~ ~~ ~ /  ===- ~~~
           \______ o          __/
            \    \        __/
              \____\______/
    
As you can see - Docker will pull the ``whalesay`` image from the remote repository and install it to your local repository. You can view your locally available images using the ``docker image`` command:

.. code:: bash

    [alces@master1(docker) ~]$ docker images
    REPOSITORY          TAG                 IMAGE ID            CREATED             SIZE
    docker/whalesay     latest              6b362a9f73eb        8 months ago        247 MB

Running a container interactively
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
Containers can also be run interactively - similar to using a Virtual Machine. To run a basic CentOS 7 Docker Image in interactive mode: 

.. code:: bash

    [alces@master1(docker) ~]$ docker run -i -t centos /bin/bash
    Unable to find image 'centos:latest' locally
    latest: Pulling from library/centos
    a3ed95caeb02: Pull complete
    3286cdf780ef: Pull complete
    Digest: sha256:8072bc7c66c3d5b633c3fddfc2bf12d5b4c2623f7004d9eed6aae70e0e99fbd7
    Status: Downloaded newer image for centos:latest
    [root@3c5ad59992bd /]# cat /etc/redhat-release
    CentOS Linux release 7.2.1511 (Core)
    [root@3c5ad59992bd /]#

You can disconnect from an interactive session and leave the Docker image running using the following key-combination: 

.. code:: bash

    ctrl+p
    ctrl+q

What's next?
------------
This guide is only a very brief introduction to using your Alces Flight Container Appliance - for advanced usage of Docker and real-world usage, view the Docker documentation: 

-  https://docs.docker.com/engine/userguide/intro/
