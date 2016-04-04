.. _run-a-graphical-application:

Run a graphical application
###########################

How to run a graphical application using your ClusterWare environment.

The following guide will detail how to create an Alces GNOME session,
then run a graphical application from within the graphical session.

Prerequisites
-------------

-  `ClusterWare compute environment
   deployed <heat-deploy-sgecluster>`__

Creating a graphical session
----------------------------

From the ClusterWare login node - start an Alces GNOME session:

.. code:: bash

    [alces@login1(hpc1) ~]$ alces session start gnome
    VNC server started:
        Identity: 219c31a8-bdff-11e5-b528-fa163e41c43d
            Type: gnome
            Host: 10.77.1.49
            Port: 5901
         Display: 1
        Password: 1QSZyfCw
       Websocket: 41361

    Depending on your client, you can connect to the session using:

      vnc://alces:1QSZyfCw@10.77.1.49:5901
      10.77.1.49:5901
      10.77.1.49:1

    If prompted, you should supply the following password: 1QSZyfCw

Use the information displayed above to connect to the VNC session using
your favourite VNC client.

Running a graphical application
-------------------------------

Once connected to the graphical session - open the ``Terminal``
application and run the ``glxgears`` application:

.. figure:: _images/HPC-GraphicalDesktop.png
    :alt: Graphical Desktop Session

Many popular graphical applications can be used this way - try your own
graphical application from the Alces Gridware repository.
