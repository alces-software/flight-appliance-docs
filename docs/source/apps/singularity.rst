.. _singularity:

Using Gridware with Singularity
###############################

Much like how :ref:`Gridware can be used within docker containers <docker>`, Singularity can import those same containers into an image file. Providing an alternative to docker for distributing applications in a platform-agnostic manner. 

To install singularity support, apply the feature profile (for more info on features see :ref:`feature-profiles`::

    alces customize apply feature/configure-singularity

.. note:: This feature compiles Singularity and installs it to ``/opt/apps`` on the system. Any client nodes that are to run Singularity containers will need to have the feature installed to do so

Once the feature has been installed, the Singularity binary can be added to the path by loading the module::

    module load apps/singularity/2.4

Building Images
===============

Singularity supports a range of `build sources <http://singularity.lbl.gov/archive/docs/v2-3/user-guide#supported-uris>`_ for its images, this documentation will focus primarily on using `Singularity Hub <http://singularity-hub.org/>`_ for building container images.

To build the hello-world container from Singularity Hub::

    singularity pull shub://vsoch/hello-world

This creates an image file in the current working directory called ``vsoch-hello-world-master.simg``.

Running Containers
==================

The image can either be executed as a binary or interactively logged into. 

Executing Container
-------------------

The container can be run using the predefined entrypoint (at ``/Singularity`` in the image)::

    [alces@login1(scooby) ~]$ singularity run vsoch-hello-world-master.simg
    RaawwWWWWWRRRR!!

Or it can have a single command passed through to it to run::

    [alces@login1(mycluster) ~]$ singularity exec vsoch-hello-world-master.simg cat /etc/os-release
    NAME="Ubuntu"
    VERSION="14.04.5 LTS, Trusty Tahr"
    ID=ubuntu
    ID_LIKE=debian
    PRETTY_NAME="Ubuntu 14.04.5 LTS"
    VERSION_ID="14.04"
    HOME_URL="http://www.ubuntu.com/"
    SUPPORT_URL="http://help.ubuntu.com/"
    BUG_REPORT_URL="http://bugs.launchpad.net/ubuntu/"

.. note:: ``/home`` from the master system is automatically mounted within the container so scripts existing in the user's home directory can be executed within the container 

Logging into Container
----------------------

To connect to an interactive shell on the image::

    [alces@login1(mycluster) ~]$ singularity shell vsoch-hello-world-master.simg
    Singularity: Invoking an interactive shell within container...

    Singularity vsoch-hello-world-master.simg:~>

