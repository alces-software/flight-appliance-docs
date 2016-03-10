.. _run-mpi-imb:

Running IMB with MPI using the cluster scheduler 
================================================

The following guide will detail how to run the Intel Messaging Benchmark (IMB) which measures bandwidth and latency between compute hosts. Together with an MPI and the cluster scheduler - this can easily be run across your compute environment to gather statistics and benchmarks for your environment.

IMB Installation
----------------

To run the Intel Messaging Benchmark applications, several Gridware applications are required: 

- ``mpi/openmpi/1.8.5`` or later
- ``apps/imb/4.0``

These can either be installed manually, or via Depot installation: 

.. code:: bash

    alces gridware depot fetch https://s3-eu-west-1.amazonaws.com/packages.alces-software.com/depots/benchmark
    alces gridware depot enable benchmark

For more information on Gridware packages and depots - please visit the :ref:`working with Gridware packages<working-with-gridware-applications>` and :ref:`working with depots<working-with-gridware-depots>` guides.

Running IMB (SGE)
-----------------

Once the MPI and IMB Gridware packages are available to each of the hosts in your environment - a job script needs to be created in order to instruct the cluster scheduler which actions to perform. 

Create an example job script in your home directory, for example ``imb.sh``: 

.. code:: bash

    #!/bin/bash -l
    #$ -cwd -V -j y
    #$ -o $HOME/$JOB_NAME.$JOB_ID.output
    #$ -N imb
    #$ -pe mpislots-verbose 200
    module load mpi/openmpi
    module load apps/imb
    mpirun -np 200 IMB-MPI1

The job script instructs the scheduler to request 200 slots/cores, load the ``openmpi`` and ``imb`` Gridware packages, then run the IMB application over the 200 requested cores using MPI. 

Change the number of slots requested to suit your environment, then submit the job script to the scheduler. Once it is running - output will be sent to the ``imb.$JOB_ID.output`` file.
