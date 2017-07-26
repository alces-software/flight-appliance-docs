.. _using-namd-with-alces-flight-compute:

====================================
Using NAMD with Alces Flight Compute
====================================

NAMD is a parallel molecular dynamics code designed for high-performance simulation of large biomolecular systems [1]_. The following page details how to install the NAMD application to your Alces Flight Compute environment, as well as how to get started using the NAMD application.

Prerequisites
-------------

-  Alces Flight Compute environment deployed [2]_
-  A VNC graphical desktop client

Installation
------------

From your Alces Flight Compute environment, you can begin by installing the NAMD Gridware package. Use the command below to install NAMD:

.. code:: bash

    alces gridware install apps/namd --variant all --yes

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

Next, start a GNOME desktop session and connect to it to begin using NAMD/VMD [3]_. This can be done from the login node with the following command:

.. code:: bash

    alces session start gnome

Once you are connected to your graphical desktp session, open the Terminal application and load the following modules [4]_:

.. code:: bash

    module load apps/namd_mpi

Next, navigate to the previously unpacked tutorials directory then navigate to the ``1-2-sphere`` directory. Open the ``ubq_ws_eq.conf`` configuration file for editing in your favourite text editor, then check that the ``structure`` and ``coordinates`` paths are correct. 

Next, run the simulation - this should take about 20 minutes to complete. You can either run this through an interactive scheduler session on a dedicated compute node or create a job script and run through the cluster scheduler. 

To run the NAMD simulation in an interactive session using the SLURM scheduler - run the following commands: 

.. code:: bash

    srun -n2 --mem-per-cpu=1024 --pty /bin/bash
    cd ~/namd-tutorial-files/1-2-sphere
    module load apps/namd_mpi
    mpirun -np 2 namd2 ubq_ws_eq.conf > ubq_ws_eq.log &

Or to run through the job scheduler, you could use the following example job script for the SLURM scheduler: 

.. code:: bash

    #!/bin/bash -l
    #SBATCH -n 4
    #SBATCH --mem-per-cpu=1024
    #SBATCH -J NAMD
    #SBATCH -o /home/%u/namd.%j.out
    module load apps/namd_mpi
    cd $HOME/namd-tutorial-files/1-2-sphere
    mpirun namd2 ubq_ws_eq.conf

.. note:: The output directory is set to ``/home/%u/`` instead of ``$HOME`` due to the SLURM script terminating before launch when using shell variables in ``#SBATCH`` arguments.

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
.. [3] :ref:`Starting desktop sessions <graphicaldesktop>`
.. [4] :ref:`Loading environment modules <modules-environment-management>`
