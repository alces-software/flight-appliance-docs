.. _using-namd-with-alces-flight-compute:

====================================
Using NAMD with Alces Flight Compute
====================================

NAMD is a parallel molecular dynamics code designed for high-performance simulation of large biomolecular systems [1]_. The following page details how to install the NAMD application to your Alces Flight Compute environment, as well as how to get started using the NAMD application.

Prerequisites
-------------

-  Alces Flight Compute environment deployed [2]_
-  Administrator access to the Alces Flight Compute environment
-  ``volatile`` Gridware repository enabled [3]_

Installation
------------

From your Alces Flight Compute environment as a valid administrator user, you can begin installing the NAMD Gridware package. The ``tcl-devel`` package must be installed on all hosts, this can be done via ``pdsh`` [4]_ or through the Alces customiser tool [5]_. Using the PDSH command, ``tcl-devel`` can be installed on all hosts in your environment using the following example command:

.. code:: bash
   
    pdsh -g cluster 'sudo yum -y install tcl-devel'

Once the ``tcl-devel`` package is installed, you can begin installing the Gridware packages required to successfully install NAMD. 

The below Gridware packages should be installed in the following order to correctly install the NAMD package:

.. code:: bash

    alces gridware install --binary main/mpi/openmpi/1.8.5
    alces gridware install volatile/libs/fftw2/2.1.5 --variant all
    alces gridware install volatile/apps/namd --variant all
    alces gridware install volatile/apps/vmd

Using NAMD
----------

Download and unpack the following tutorial files to your home directory: 

  -   https://s3-eu-west-1.amazonaws.com/alces-public/namd-tutorial-files.tgz

The tutorial files contain the appropriate files to get started with NAMD - examining the minimisation and equilibration of ubiquitin protein in a water sphere placed in vacuum. 

The tutorial files can be downloaded and unpacked to your cluster using the following commands from the login node: 

.. code:: bash

    wget https://s3-eu-west-1.amazonaws.com/alces-public/namd-tutorial-files.tgz
    tar -zxvf namd-tutorial-files.tgz
    cd namd-tutorial-files/1-2-sphere

Next, start a GNOME desktop session and connect to it to begin using NAMD/VMD [6]_. This can be done from the login node with the following command:

.. code:: bash

    alces session start gnome

Once you are connected to your graphical desktp session, open the Terminal application and load the following modules [7]_:

.. code:: bash

    module load apps/namd_mpi

Next, navigate to the previously unpacked tutorials directory then navigate to the ``1-2-sphere`` directory. Open the ``ubq_ws_eq.conf`` configuration file for editing in your favourite text editor, then check that the ``structure`` and ``coordinates`` paths are correct. 

Next, run the simulation - this should take about 20 minutes to complete. You can either run this through an interactive scheduler session on a dedicated compute node or create a job script and run through the cluster scheduler. 

To run the NAMD simulation in an interactive session using the SGE scheduler - run the following commands: 

.. code:: bash

    qrsh -pe smp-verbose 2 -l v_hmem=1G
    cd namd-tutorial-files/1-2-sphere
    module load apps/namd_mpi
    namd2 ubq_ws_eq.conf > ubq_ws_eq.log &

Or to run through the scheduler, you could use the following example job script for the SGE scheduler: 

.. code:: bash

    #!/bin/bash -l
    #$ -pe smp-verbose 2
    #$ -l h_vmem=1G
    #$ -N NAMD -o $HOME/namd.$JOB_ID.out
    module load apps/namd_mpi
    cd $HOME/namd-tutorial-files/1-2-sphere
    namd2 ubq_ws_eq.conf

Once the task has finished, your output file will contain lots of output data. The end of your output file should contain the following if the job has successfully completed: 

.. code:: bash

    WRITING EXTENDED SYSTEM TO OUTPUT FILE AT STEP 2600
    WRITING COORDINATES TO OUTPUT FILE AT STEP 2600
    CLOSING COORDINATE DCD FILE
    The last position output (seq=-2) takes 0.046 seconds, 309.516 MB of memory in use
    WRITING VELOCITIES TO OUTPUT FILE AT STEP 2600
    The last velocity output (seq=-2) takes 0.015 seconds, 309.516 MB of memory in use
    ====================================================
    
    WallClock: 269.963684  CPUTime: 269.963684  Memory: 309.515625 MB
    End of program

.. [1] http://www.ks.uiuc.edu/Research/namd/
.. [2] :ref:`Launch an Alces Flight Compute environment <launching_on_aws>`
.. [3] `How to enable volatile repository <http://docs.alces-flight.com/en/latest/apps/apps.html?highlight=volatile#volatile-gridware-repositories>`_
.. [4] :ref:`PDSH usage <basic_cluster_operation>`
.. [5] :ref:`Cluster customisation <customisation>`
.. [6] :ref:`Starting desktop sessions <graphicaldesktop>`
.. [7] `Loading environment modules <http://docs.alces-flight.com/en/latest/apps/apps.html#modules-environment-management>`_
