Flight Appliance Documentation - 2016.2r3
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
   launch-aws/launching_on_aws
   launch-os/*
   basics/*
   databasics/*
   graphicaldesktop/*
   apps/*
   mpiapps/*
   jobschedulers/*
   sge/*
   
.. toctree::
   :maxdepth: 1
   :glob:
   :caption: Customising your cluster
   :name: customising
        
   customisation/*
   launch-aws/manual-launch
   launch-aws/template_launch

.. toctree::
   :maxdepth: 1
   :glob:
   :caption: Example workflows 
   :name: exampleworkflows

   getting-started/environment-usage/gridware-features/*
   getting-started/environment-usage/using-openfoam-with-alces-flight-compute.rst


