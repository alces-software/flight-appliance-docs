Flight Appliance Documentation - 2016.4r1
==========================================
.. figure:: _images/AlcesFlight.png
    :alt: Alces Computing in the Cloud

This site holds documentation designed to help users create a simple research compute environment using popular public and private cloud platforms. As well as launching and accessing the environment, guides and tutorials are included to help end-users install software applications, manage their data and run different workloads. 

License
-------
This documentation is released under the `Creative-Commons: Attribution-ShareAlike 4.0 International <http://creativecommons.org/licenses/by-sa/4.0/>`_ license. The Alces Flight Compute software is released under the terms of the `Alces Flight EULA <https://s3-eu-west-1.amazonaws.com/flight-aws-marketplace/2016.4/AlcesFlight_2016.4_EULA.txt>`_. Please read the relevant license text before proceeding to use. 

Prerequisites
---------------
We recommend that users wishing to use Flight Appliances have basic Linux skills. The ability to move about in a filesystem, copy and delete files, read and edit files on the command-line will be needed in order to get the best out of the Flight software. 

.. Navigation/TOC

.. toctree::
   :maxdepth: 1
   :glob:
   :caption: Using your Flight Compute cluster
   :name: using

   quickstart/*
   overview/*

.. toctree::
   :maxdepth: 1
   :glob:
   :caption: Launching your cluster
   :name: launching

   launch-aws/launching_on_aws
   launch-os/*

.. toctree::
   :maxdepth: 1
   :glob:
   :caption: Using your cluster
   :name: cluster-usage

   basics/*
   databasics/*
   alces-sync/*
   graphicaldesktop/*
   vpn/vpn

.. toctree::
   :maxdepth: 1
   :glob:
   :caption: Software applications
   :name: clusterapps

   apps/apps
   mpiapps/*

.. toctree::
   :maxdepth: 1
   :glob:
   :caption: Cluster job schedulers
   :name: schedulers

   jobschedulers/*
   sge/sge
   slurm/slurm
   openlava/openlava
   torque/torque
   pbspro/pbspro
   
.. toctree::
   :maxdepth: 1
   :glob:
   :caption: Customising your cluster
   :name: customising
        
   customisation/*
   launch-aws/manual-launch
   launch-aws/template_launch
   sge/disable_sge_limits

.. toctree::
   :maxdepth: 1
   :glob:
   :caption: Available software packages
   :name: gridware
   
   apps/gridware

.. toctree::
   :maxdepth: 1
   :glob:
   :caption: Example workflows 
   :name: exampleworkflows

   getting-started/environment-usage/gridware-features/*
   getting-started/environment-usage/using-openfoam-with-alces-flight-compute.rst
   getting-started/environment-usage/namd_on_flight.rst

