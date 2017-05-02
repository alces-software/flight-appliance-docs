.. _docker:

Using Gridware with Docker
##########################

Gridware can now be transported within a docker container which can be shared with the nodes in a Flight environment. This allows applications to be packaged within in a container for distribution between nodes or even to systems outside of the Flight environment. 

To install docker support, apply the feature profile (for more info on features see :ref:`feature-profiles`)::

    alces customize apply feature/configure-docker

