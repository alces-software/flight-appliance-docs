.. _run-an-mpi-job:

Run an MPI Job
##############

How to run an MPI job over 2 compute nodes using a ClusterWare SGE
compute environment. The following guide will detail how to perform a
2-node MPI job using the Intel Messaging Benchmark Gridware application,
which measures latency and bandwidth metrics between multiple compute
nodes.

Prerequisites
-------------

-  Alces Flight Compute environment deployed
-  2 compute nodes provisioned
-  ``benchmark`` Gridware depot installed

Running IMB
-----------

From the Alces Flight Compute environment login node, as either the administrator
(``alces``) user - or a regular cluster user account - check the ``imb``
application is available from the Gridware depot:

.. code:: bash

    [alces-cluster@login1(hpc1) ~]$ module avail
    ---  /opt/gridware/benchmark/el7/etc/modules  ---
      apps/imb/4.0/gcc-4.8.5+openmpi-1.8.5
      apps/iozone/3.420/gcc-4.8.5
      apps/memtester/4.3.0/gcc-4.8.5
      compilers/gcc/system
      libs/gcc/system
      mpi/openmpi/1.8.5/gcc-4.8.5
      null
    ---  /opt/gridware/local/el7/etc/modules  ---
      compilers/gcc/system
      libs/gcc/system
      null
    ---  /opt/clusterware/etc/modules  ---
      services/gridscheduler

Next - prepare a job script for the cluster scheduler, which will
instruct it to run a two-node MPI IMB job.

.. code:: bash

    #!/bin/bash
    #$ -j y -N imb -o $HOME/imb_out.$JOB_ID
    #$ -pe mpinodes-verbose 2 -cwd -V
    module load mpi/openmpi
    module load apps/imb
    mpirun IMB-MPI1

Save the job script in your home directory, e.g.
``/home/alces-cluster/imb-2node.sh``

Next - submit the job script to the cluster scheduler:

.. code:: bash

    [alces-cluster@login1(hpc1) ~]$ qsub imb-2node.sh 
    Your job 1 ("imb") has been submitted
    [alces-cluster@login1(hpc1) ~]$ qstat
    job-ID  prior   name       user         state submit/start at     queue                          slots ja-task-ID 
    -----------------------------------------------------------------------------------------------------------------
          1 11.02734 imb        alces-cluste r     01/18/2016 11:45:22 bynode.q@cluster-group-7ulle2n     2        

Once the IMB application has finished running, the output file can be
seen in ``$HOME/imb_out.$JOB_SCRIPT``. The job script output displays
the cluster compute nodes to run the application, together with
performance output:

::

    =======================================================
    SGE job submitted on Mon Jan 18 11:45:22 GMT 2016
    2 hosts used
    JOB ID: 1
    JOB NAME: imb
    PE: mpinodes-verbose
    QUEUE: bynode.q
    MASTER cluster-group-7ulle2nlo52d-f4yxu5qc5n7q-pdben42dhbav.novalocal
    Nodes used:
    cluster-group-7ulle2nlo52d-f4yxu5qc5n7q-pdben42dhbav
    cluster-group-7ulle2nlo52d-wpz7bmg5k6wo-jqzxx5ewsehp
    =======================================================

    ** A machine file has been written to /tmp/sge.machines.1 on cluster-group-7ulle2nlo52d-f4yxu5qc5n7q-pdben42dhbav.novalocal **

    =======================================================
    If an output file was specified on job submission
    Job Output Follows:
    =======================================================
    #------------------------------------------------------------
    #    Intel (R) MPI Benchmarks 4.0, MPI-1 part    
    #------------------------------------------------------------
    # Date                  : Mon Jan 18 11:58:11 2016
    # Machine               : x86_64
    # System                : Linux
    # Release               : 3.10.0-327.4.4.el7.x86_64
    # Version               : #1 SMP Tue Jan 5 16:07:00 UTC 2016
    # MPI Version           : 3.0
    # MPI Thread Environment: 

    # New default behavior from Version 3.2 on:

    # the number of iterations per message size is cut down 
    # dynamically when a certain run time (per message size sample) 
    # is expected to be exceeded. Time limit is defined by variable 
    # "SECS_PER_SAMPLE" (=> IMB_settings.h) 
    # or through the flag => -time 
      


    # Calling sequence was: 

    # IMB-MPI1

    # Minimum message length in bytes:   0
    # Maximum message length in bytes:   4194304
    #
    # MPI_Datatype                   :   MPI_BYTE 
    # MPI_Datatype for reductions    :   MPI_FLOAT
    # MPI_Op                         :   MPI_SUM  
    #
    #

    # List of Benchmarks to run:

    # PingPong
    # PingPing
    # Sendrecv
    # Exchange
    # Allreduce
    # Reduce
    # Reduce_scatter
    # Allgather
    # Allgatherv
    # Gather
    # Gatherv
    # Scatter
    # Scatterv
    # Alltoall
    # Alltoallv
    # Bcast
    # Barrier
    ...

