.. _run-mpi-hpl:

Running HPL with MPI using the cluster scheduler 
================================================

The following guide will detail how to run the High Performance Linpack (HPL) benchmark, which measures floating point performance of single or multiple compute hosts working together in your environment. Together with an MPI and the cluster scheduler - this can easily be run across your compute environment to gather statistics and benchmarks for your environment.

HPL Installation
----------------

To run the High Performance Linpack benchmark, several Gridware applications are required: 

- ``mpi/openmpi/1.8.5`` or later
- ``atlas/3.10.2``
- ``apps/hpl``

These can either be installed manually, or via Depot installation: 

.. code:: bash

    alces gridware depot fetch https://s3-eu-west-1.amazonaws.com/packages.alces-software.com/depots/hpl
    alces gridware depot enable hpl 

For more information on Gridware packages and depots - please visit the :ref:`working with Gridware packages<working-with-gridware-applications>` and :ref:`working with depots<working-with-gridware-depots>` guides.

Running HPL (SGE)
-----------------

Once the MPI and HPL Gridware packages are available to each of the hosts in your environment - a job script needs to be created in order to instruct the cluster scheduler which actions to perform. 

Create an example job script in your home directory, for example ``hpl.sh``: 

.. code:: bash

    #!/bin/bash -l
    #$ -cwd -V -j y
    #$ -o $HOME/$JOB_NAME.$JOB_ID.output
    #$ -N hpl
    #$ -pe mpislots-verbose 200
    module load mpi/openmpi
    module load apps/hpl
    mpirun -np 200 xhpl

The job script instructs the scheduler to request 200 slots/cores, load the ``openmpi`` and ``hpl`` Gridware packages, then run the HPL application over the 200 requested cores using MPI. 

Change the number of slots requested to suit your environment, then submit the job script to the scheduler. Once it is running - output will be sent to the ``hpl.$JOB_ID.output`` file.

Next - in the same directory as your job script, create the HPL configuration file - named ``HPL.dat``. A HPL configuration file can be obtained from: 

- http://www.advancedclustering.com/act-kb/tune-hpl-dat-file/

*Note*: The larger the ``N`` - the longer the benchmark will take to run, try using a small ``N`` value to test before running a large value.

Once you have both the job script and HPL input file, you can now submit your job to the cluster scheduler. Output will be wrriten to the ``hpl.$JOB_ID.output`` file in your users home directory. 
