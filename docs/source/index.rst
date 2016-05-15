Flight Appliance Documentation - 2016.2
==========================================
.. figure:: _images/AlcesFlight.png
    :alt: Alces Computing in the Cloud

This site holds documentation designed to help users create a simple research compute environment using popular public and private cloud platforms. As well as launching and accessing the environment, guides and tutorials are included to help end-users install software applications, manage their data and run different workloads. 

License
-------
This documentation is released under the `Creative-Commons: Attribution-ShareAlike 4.0 International <http://creativecommons.org/licenses/by-sa/4.0/>`_ license. The Alces Flight Compute software is released under the terms of the `Alces Flight EULA <https://s3-eu-west-1.amazonaws.com/flight-aws-marketplace/2016.1/EULA.txt>`_. Please read the relevant license text before proceeding to use. 

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
   launch-aws/*   
   launch-os/*
   basics/*
   databasics/*
   graphicaldesktop/*
   apps/*
   mpiapps/*
   jobschedulers/*
   sge/*
   
.. toctree::
   :maxdepth: 3
   :glob:
   :caption: Customising your Flight Compute cluster
   :name: customising
        
   customisation/*

.. toctree::
   :maxdepth: 3
   :glob:
   :caption: Example workflows 
   :name: exampleworkflows

   getting-started/environment-usage/run-an-interactive-application
   getting-started/environment-usage/run-a-graphical-application
   getting-started/environment-usage/gridware-features/*
   getting-started/environment-usage/gridware-howto/*
   getting-started/environment-usage/run-an-mpi-job
   getting-started/environment-usage/galaxy/*
   clusterware-storage/alces-storage-overview
   clusterware-storage/alces-storage-file-config
   clusterware-storage/alces-storage-file-usage
   clusterware-storage/alces-storage-object-config
   clusterware-storage/alces-storage-object-usage
   environment-usage/alces-storage-examples

.. toctree::
   :maxdepth: 3
   :glob:
   :caption: Alces Flight Roadmap and future Appliances
   :name: flightappliances

   about/alces-flight-compute
   about/alces-flight-storage-access
   about/alces-flight-application-manager
   about/alces-flight-galaxy-appliance
