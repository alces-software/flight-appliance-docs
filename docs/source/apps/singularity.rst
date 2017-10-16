.. _singularity:

Using Gridware with Singularity
###############################

Much like how :ref:`Gridware can be used within docker containers <docker>`, Singularity can import those same containers into an image file. Providing an alternative to docker for distributing applications in a platform-agnostic manner. 

To install singularity support, apply the feature profile (for more info on features see :ref:`feature-profiles`::

    alces customize apply feature/configure-singularity

.. note:: This feature compiles Singularity and installs it to ``/opt/`` on the system. Any client nodes that are to run Singularity containers will need to have the feature installed to do so

Importing Gridware Container Images
===================================

To run a gridware application as a Singularity container it must first be :ref:`built as a Docker container<docker-build-images>` and :ref:`shared <docker-share-images>`.

Once the image resides in the local registry, a container image file can be created::

    singularity create memtester.img

Then the docker image can be imported using the SSL certificate generated as part of the registry configuration::

    SSL_CERT_FILE="/opt/gridware/docker/certificates/public/login1:5000/ca.crt" singularity import memtester.img docker://login1:5000/alces/gridware-apps-memtester-4.3.0

The file ``memtester.img`` can now be run as an executable!
