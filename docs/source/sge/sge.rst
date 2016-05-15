.. _sge:


Open Grid Scheduler (SGE)
=========================

The `Open Grid Scheduler (OGS) <http://gridscheduler.sourceforge.net/>`_ cluster job-scheduler is an open-source implementation of the popular Sun Grid-Engine (SGE) codebase, with recent patches and updates to support newer Linux distributions. The syntax and usage of commands is identical to historical SGE syntax, and users can typically migrate from one to another with no issues. This documentation provides a guide to using OGS on your Alces Flight Compute cluster to run different types of jobs. 

See :ref:`jobschedulers` for a description of the different use-cases of a cluster job-scheduler. 

Running an Interactive job
-------------------------- 

You can start a new interactive job on your Flight Compute cluster by using the ``qrsh`` command; the scheduler will search for an available compute node, and provide you with an interactive login shell on the node if one is available. 

.. code:: bash

    [alces@login1(scooby) ~]$ qrsh
    Warning: Permanently added '[ip-10-75-0-21.eu-west-1.compute.internal]:53024,[10.75.0.21]:53024' (ECDSA) to the list of known hosts.

    <<< -[ alces flight ]- >>>
    [alces@ip-10-75-0-21(scooby) ~]$ hostname -f
    ip-10-75-0-21.eu-west-1.compute.internal
    
    [alces@ip-10-75-0-21(scooby) ~]$ module load apps/R
    
    [alces@ip-10-75-0-21(scooby) ~]$ R
    
    R version 3.2.3 (2015-12-10) -- "Wooden Christmas-Tree"
    Copyright (C) 2015 The R Foundation for Statistical Computing
    Platform: x86_64-pc-linux-gnu (64-bit)
    > 

Alternatively, the ``qrsh`` command can also be executed from an interactive desktop session; the job-scheduler will automatically find an available compute node to launch the job on. Applications launched from within the qrsh session are executed on the assigned cluster compute node:

.. image:: interactivejob.jpg
     :alt: Running an interactive graphical job
     
When you've finished running your application, simply type ``logout``, or press **CTRL+D** to exit the interactive job. 

If the job-scheduler could not satisfy the resources you've requested for your interactive job (e.g. all your available compute nodes are busy running other jobs), it will report back after a few seconds with an error:

.. code:: bash

    [alces@login1(scooby) ~]$ qrsh 
    Your "qrsh" request could not be scheduled, try again later.

You can force an interactive job to queue for available resources by adding the parameter ``-now no`` to your ``qrsh`` command. 

Submitting a batch job
----------------------

Batch (or non-interactive) jobs allow users to leverage one of the main benefits of having a cluster scheduler; jobs can be queued up with instructions on how to run them and then executed across the cluster while the user `does something else <https://www.quora.com/What-do-you-do-while-youre-waiting-for-your-code-to-finish-running>`_. Users submit jobs as scripts, which include instructions on how to run the job - the output of the job (*stdout* and *stderr* in Linux terminology) is written to a file on disk for review later on. You can write a batch job that does anything that can be typed on the command-line. 

We'll start with a basic example - the following script is written in bash (the default Linux command-line interpreter). You can create the script yourself using the `Nano <http://www.howtogeek.com/howto/42980/the-beginners-guide-to-nano-the-linux-command-line-text-editor/>`_ command-line editor - use the command ``nano simplejobscript.sh`` to create a new file, then type in the contents below. The script does nothing more than print some messages to the screen (the **echo** lines), and sleeps for 120 seconds. We've saved the script to a file called ``simplejobscript.sh`` - the ``.sh`` extension helps to remind us that this is a *shell* script, but adding a filename extension isn't strictly necessary for Linux. 

.. code:: bash
    
    #!/bin/bash -l
    echo "Starting running on host $HOSTNAME"
    sleep 120
    echo "Finished running - goodbye from $HOSTNAME"
    
We can execute that script directly on the login node by using the command ``bash simplejobscript.sh`` - after a couple of minutes, we get the following output:

.. code:: bash

    Starting running on host login1
    Finished running - goodbye from login1

To submit your jobscript to the cluster job scheduler, use the command ``qsub simplejobscript.sh``. The job scheduler should immediately report the job-ID for your job; your job-ID is unique for your current Alces Flight Compute cluster - it will never be repeated once used.

.. code:: bash

    [alces@login1(scooby) ~]$ qsub simplejobscript.sh
    Your job 3 ("simplejobscript.sh") has been submitted

    [alces@login1(scooby) ~]$
    

Viewing and controlling queued jobs
-----------------------------------

Once your job has been submitted, use the ``qstat`` command to view the status of the job queue. If you have available compute nodes, your job should be shown in ``r`` (running) state; if your compute nodes are busy, or you've launched an auto-scaling cluster and currently have no running nodes, your job may be shown in ``qw`` (queuing/waiting) state until compute nodes are available to run it. 

.. code:: bash

    [alces@login1(scooby) ~]$ qstat
    job-ID  prior   name       user         state submit/start at     queue                          slots ja-task-ID
    -----------------------------------------------------------------------------------------------------------------
         3 11.02734 simplejobs alces        r     05/15/2016 09:32:54 byslot.q@ip-10-75-0-131.eu-wes     1       


You can keep running the ``qstat`` command until your job finishes running and disappears from the queue. The output of your batch job will be stored in a file for you to look at. The default location to store the output file is your home-directory - the output file will be named in the format ``<jobscript-name>.o<job-ID>``. So - in the example above, our jobscript was called ``simplejobscript.sh`` and the job-ID was ``3``, so our output file is located at ``~/simplejobscript.sh.o3``. You can use the Linux ``more`` command to view your output file:

.. code:: bash
  
    [alces@login1(scooby) ~]$ more ~/simplejobscript.sh.o3
    Starting running on host ip-10-75-0-131.eu-west-1.compute.internal
    Finished running - goodbye from ip-10-75-0-131.eu-west-1.compute.internal


Your job runs on whatever node the scheduler can find which is available for use - you can try submitting a bunch of jobs at the same time, and using the ``qstat`` command to see where they run. The scheduler is likely to spread them around over different nodes in your cluster (if you have multiple nodes). The login node is not included in your cluster for scheduling purposes - jobs submitted to the scheduler will only be run on your cluster compute nodes. You can use the ``qdel <job-ID>`` command to delete a job you've submitted, whether it's running or still in queued state.

.. code:: bash
    
    [alces@login1(scooby) ~]$ qsub simplejobscript.sh
    Your job 4 ("simplejobscript.sh") has been submitted
    [alces@login1(scooby) ~]$ qsub simplejobscript.sh
    Your job 5 ("simplejobscript.sh") has been submitted
    [alces@login1(scooby) ~]$ qsub simplejobscript.sh
    Your job 6 ("simplejobscript.sh") has been submitted
    [alces@login1(scooby) ~]$ qsub simplejobscript.sh
    Your job 7 ("simplejobscript.sh") has been submitted
    [alces@login1(scooby) ~]$ qsub simplejobscript.sh
    Your job 8 ("simplejobscript.sh") has been submitted
    [alces@login1(scooby) ~]$ qstat
    job-ID  prior   name       user         state submit/start at     queue                          slots ja-task-ID
    -----------------------------------------------------------------------------------------------------------------
          4 11.15234 simplejobs alces        r     05/15/2016 09:43:48 byslot.q@ip-10-75-0-117.eu-wes     1       
          5 11.02734 simplejobs alces        r     05/15/2016 09:43:49 byslot.q@ip-10-75-0-126.eu-wes     1       
          6 11.02734 simplejobs alces        r     05/15/2016 09:43:49 byslot.q@ip-10-75-0-131.eu-wes     1       
          7 11.02734 simplejobs alces        r     05/15/2016 09:43:49 byslot.q@ip-10-75-0-154.eu-wes     1       
          8 11.02734 simplejobs alces        r     05/15/2016 09:43:49 byslot.q@ip-10-75-0-199.eu-wes     1       
 
    [alces@login1(scooby) ~]$ qdel 8
    alces has registered the job 8 for deletion


Viewing compute host status
---------------------------

Users can use the ``qhost`` command to view the status of compute node hosts in your Flight Compute cluster. 

.. code:: bash

    [alces@login1(scooby) ~]$ qhost
    HOSTNAME                ARCH         NCPU  LOAD  MEMTOT  MEMUSE  SWAPTO  SWAPUS
    -------------------------------------------------------------------------------
    global                  -               -     -       -       -       -       -
    ip-10-75-0-117          linux-x64      36  0.01   58.6G  602.7M    2.0G     0.0
    ip-10-75-0-126          linux-x64      36  0.01   58.6G  593.6M    2.0G     0.0
    ip-10-75-0-131          linux-x64      36  0.01   58.6G  601.9M    2.0G     0.0
    ip-10-75-0-132          linux-x64      36  0.01   58.6G  589.5M    2.0G     0.0
    ip-10-75-0-154          linux-x64      36  0.01   58.6G  603.7M    2.0G     0.0
    ip-10-75-0-199          linux-x64      36  0.01   58.6G  604.9M    2.0G     0.0
    ip-10-75-0-202          linux-x64      36  0.01   58.6G  591.4M    2.0G     0.0
    ip-10-75-0-211          linux-x64      36  0.01   58.6G  586.8M    2.0G     0.0


The ``qhost`` output will show (from left-to-right):

  - The hostname of your compute nodes
  - The architecture of your compute nodes (typically 64-bit Linux for Flight Compute clusters)
  - The detected number of CPUs (including hyper-threaded cores)
  - The Linux run-queue length; e.g. for a 36-core node, a load of ``36.0`` indicates that the system is 100% loaded
  - Memory statistics; the total and used amount of physical RAM and configured swap memory
  


Default resources
=================

In order to promote efficient usage of your cluster, the job-scheduler automatically sets a number of default resources to your jobs when you submit them. These defaults must be overridden by users to help the scheduler understand how you want it to run your job - if we don't include any instructions to the scheduler, then our job will take the defaults shown below:

 - Number of CPU cores for your job: ``1``
 - Maximum job runtime (in hours): ``24``
 - Output file location: ``~/<jobscript-name>.o<jobID>``
 - Output file style: ``stdout`` and ``stderr`` merged into a single file.
 - Amount of memory for your job: the arithmetic sum of
      ``total memory per node / total cores per node``
      e.g. with 36 core nodes that have 60GB of RAM, the default memory per job is set to around 1.5GB
      
This documentation will explain how to change these limits to suit the jobs that you want to run. You can also disable these limits if you prefer to control resource allocation manually by yourself.

.. note:: Scheduler limits are automatically enforced - e.g. if your job exceeds the requested runtime or memory allocation, it will automatically be stopped. 


Providing job-scheduler instructions
====================================

Most cluster users will want to provide instructions to the job-scheduler to tell it how to run their jobs. The instructions you want to give will depend on what your job is going to do, but might include:

 - Naming your job so you can find it again
 - Controlling how job output files are written
 - Controlling when your job will be run
 - Requesting additional resources for your job
 
 
Job instructions can be provided in two ways; they are:

 1. **On the command line**, as parameters to your ``qsub`` or ``qrsh`` command. 
 
    e.g. you can set the name of your job using the ``-N <name>`` option:
    
.. code:: bash
    
    [alces@login1(scooby) ~]$ qsub -N newname simplejobscript.sh
    Your job 16 ("newname") has been submitted

    [alces@login1(scooby) ~]$ qstat
    job-ID  prior   name       user         state submit/start at     queue                          slots ja-task-ID
    -----------------------------------------------------------------------------------------------------------------
         16 11.02734 newname    alces        r     05/15/2016 10:09:13 byslot.q@ip-10-75-0-211.eu-wes     1       




 2. For batch jobs, job scheduler instructions can also **included in your job-script** on a line starting with the special identifier ``#$``. 
 
    e.g. the following job-script includes a ``-N`` instruction that sets the name of the job:
    
.. code:: bash
    
    #!/bin/bash -l
    #$ -N newname
    echo "Starting running on host $HOSTNAME"
    sleep 120
    echo "Finished running - goodbye from $HOSTNAME"


Including job scheduler instructions in your job-scripts is often the most convenient method of working for batch jobs - follow the guidelines below for the best experience:

  - Lines in your script that include job-scheduler instructions must start with ``#$`` at the beginning of the line
  - You can have multiple lines starting with ``#$`` in your job-script, with normal scripts lines in-between.
  - You can put multiple instructions separated by a space on a single line starting with ``#$``
  - The scheduler will parse the script from top to bottom and set instructions in order; if you set the same parameter twice, the second value will be used and a warning will be printed at submission time.
  - Instructions provided as parameters to ``qsub`` override values specified in job-scripts. 
  - Instructions are parsed at job submission time, before the job itself has actually run. That means you can't, for example, tell the scheduler to put your job output in a directory that you create in the job-script itself - the directory will not exist when the job starts running, and your job will fail with an error. 
  - You can use dynamic variables in your instructions (see below)
  

Dynamic scheduler variables
---------------------------

Your cluster job scheduler automatically creates a number of pseudo environment variables which are available to your job-scripts when they are running on cluster compute nodes, along with standard Linux variables. Useful values include the following:

 - ``$HOME``        The location of your home-directory
 - ``$USER``        The Linux username of the submitting user
 - ``$HOSTNAME``    The Linux hostname of the compute node running the job
 - ``$JOB_ID``      The job-ID number for the job
 - ``$JOB_NAME``    The configured job name
 - ``$SGE_TASK_ID`` For task array jobs, this variable indicates the task number. This variable is not defined for non-task-array jobs. 
 
 
Simple scheduler instruction examples
-------------------------------------

Here are some commonly used scheduler instructions, along with some examples of their usage:

Setting output file location
~~~~~~~~~~~~~~~~~~~~~~~~~~~~

To set the output file location for your job, use the ``-o <filename>`` option - both standard-out and standard-error from your job-script, including any output generated by applications launched by your script, will be saved in the filename you specify. 

By default, the scheduler stores data relative to your home-directory - but to avoid confusion, we recommend **specifying a full path to the filename** to be used. Although Linux can support several jobs writing to the same output file, the result is likely to be garbled - it's common practice to include something unique about the job (e.g. it's job-ID) in the output filename to make sure your job's output is clear and easy to read. 

.. note:: The directory used to store your job output file must exist **before** you submit your job to the scheduler. Your job may fail to run if the scheduler cannot create the output file in the directory requested. 

For example; the following job-script includes a ``-o`` instruction to set the output file location:

.. code:: bash
    
    #!/bin/bash -l
    #$ -N mytestjob -o $HOME/outputs/output.$JOB_NAME.$JOB_ID
    
    echo "Starting running on host $HOSTNAME"
    sleep 120
    echo "Finished running - goodbye from $HOSTNAME"

In the above example, assuming the job was submitted as user ``alces`` and was given job-ID number ``24``, the scheduler will save output data from the job in the filename ``/home/alces/outputs/output.mytestjob.24``. 


Setting working directory for your job
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

By default, jobs are executed from your home-directory on the cluster (i.e. ``/home/<your-user-name>``, ``$HOME`` or ``~``). You can include ``cd`` commands in your job-script to change to different directories; alternatively, you can provide an instruction to the scheduler to change to a different directory to run your job. The available options are:

  - ``-wd <directory>`` - instruct the job scheduler to move into the directory specified before starting to run the job on a compute node
  - ``-cwd`` - instruct the scheduler to move into the same directory you submitted the job from before starting to run the job on a compute node
  
.. note:: The directory specified must exist and be accessible by the compute node in order for the job you submitted to run.


Waiting for a previous job before running
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

You can instruct the scheduler to wait for an existing job to finish before starting to run the job you are submitting with the ``-hold_jid <job-ID`` instruction. This allows you to build up multi-stage jobs by ensuring jobs are executed sequentially, even if enough resources are available to run them in parallel. For example, to submit a new job that will only start running once job number 15352 has completed, use the following command:

   ``qsub -hold_jid 15352 myjobscript.sh``


Running task array jobs
~~~~~~~~~~~~~~~~~~~~~~~

A common workload is having a large number of jobs to run which basically do the same thing, aside perhaps from having different input data. You could generate a job-script for each of them and submit it, but that's not very convenient - especially if you have many hundreds or thousands of tasks to complete. Such jobs are known as **task arrays** - an `embarrassingly parallel <https://en.wikipedia.org/wiki/Embarrassingly_parallel>`_ job will often fit into this category. 

A convenient way to run such jobs on a cluster is to use a task array, using the ``-t <start>-<end>[:<step>]`` instruction to the job-scheduler. Your job-script can then use pseudo environment variables created by the scheduler to refer to data used by each task in the job. If the ``:step`` value is omitted, a step value of one will be used. For example, the following job-script uses the ``$SGE_TASK_ID`` variable to set the input data used for the ``bowtie2`` application:

.. code:: bash
    
    [alces@login1(scooby) ~]$ cat simplejobscript.sh
    #!/bin/bash -l
    #$ -N arrayjob
    #$ -o $HOME/data/outputs/output.$JOB_ID.$TASK_ID
    #$ -t 1-10:2
        
    module load apps/bowtie
    bowtie -i $HOME/data/genome342/inputdeck_split.$SGE_TASK_ID -o $HOME/data/outputs/g342.output.$SGE_TASK_ID

.. note:: The pseudo variable ``$SGE_TASK_ID`` is accessible under the name ``$TASK_ID`` at submission time (e.g. when setting output file location)
    
All tasks in a job are given the same job-ID, with the task number indicated after a ``.``; e.g. 

.. code:: bash

    [alces@login1(scooby) ~]$ qsub simplejobscript.sh
    Your job-array 27.1-10:2 ("arrayjob") has been submitted

    [alces@login1(scooby) ~]$ qstat
    job-ID  prior   name       user         state submit/start at     queue                          slots ja-task-ID
    -----------------------------------------------------------------------------------------------------------------
         27 11.0273 arrayjob      alces        r     05/15/2016 11:24:29 byslot.q@ip-10-75-0-211.eu-wes     1 1
         27 6.02734 arrayjob      alces        r     05/15/2016 11:24:29 byslot.q@ip-10-75-0-227.eu-wes     1 3
         27 4.36068 arrayjob      alces        r     05/15/2016 11:24:29 byslot.q@ip-10-75-0-201.eu-wes     1 5
         27 3.52734 arrayjob      alces        r     05/15/2016 11:24:29 byslot.q@ip-10-75-0-178.eu-wes     1 7
         27 3.02734 arrayjob      alces        r     05/15/2016 11:24:29 byslot.q@ip-10-75-0-42.eu-west     1 9


Individual tasks may be deleted by referring to them using ``<job-ID>.<task-ID>`` - e.g. to delete task 7 in the above example, you could use the command ``qdel 27.7``. Deleting the job-ID itself will delete all tasks in the job. 


Requesting more resources 
-------------------------

By default, jobs are constrained to the default set of resources (see above) - users can use scheduler instructions to request more resources for their jobs. The following documentation shows how these requests can be made. 


Running multi-threaded jobs
~~~~~~~~~~~~~~~~~~~~~~~~~~~

If users want to use multiple cores on a compute node to run a multi-threaded application, they need to inform the scheduler - this allows jobs to be efficiently spread over compute nodes to get the best possible performance. Using multiple CPU cores is achieved by requesting access to a **Parallel Environment (PE)** - the default multi-threaded PE on your Alces Flight Compute cluster is called **smp** (for `symmetric multi-processing <https://en.wikipedia.org/wiki/Symmetric_multiprocessing>`_). Users wanting to use the **smp PE** must request it with a job-scheduler instruction, along with the number of CPU cores they want to use - in the form ``-pe smp <number-of-cores>``. 

For example, to use 4 CPU cores on a single node for an application, the instruction ``-pe smp 4`` can be used. The following example shows the **smptest** binary being run on 8 CPU cores - this application uses the `OpenMP API <http://openmp.org/wp/>`_ to automatically detect the number of cores it has available, and prints a simple "hello world" message from each CPU core:

.. code:: bash

    [alces@login1(scooby) ~]$ more runsmp.sh
    #!/bin/bash -l
    #$ -pe smp 8 -o $HOME/smptest/results/smptest.out.$JOB_ID
    ~/smptest/hello
    
    [alces@login1(scooby) ~]$ qsub runsmp.sh
    Your job 30 ("runsmp") has been submitted
    
    [alces@login1(scooby) ~]$ more ~/smptest/results/smptest.out.30
    2: Hello World!
    5: Hello World!
    6: Hello World!
    1: Hello World!
    4: Hello World!
    0: Hello World!
    3: Hello World!
    7: Hello World!


.. note:: For debugging purposes, an ``smp-verbose`` PE is also provided that prints additional information in your job output file.

For the best experience, please follow these guidelines when running multi-threaded jobs:

  - Alces Flight Compute automatically configures compute nodes with one CPU **slot** per detected CPU core, including hyper-threaded cores. 
  - **Memory limits** are enforced per CPU-core-slot; for example, if your default memory request is 1.5GB and you request ``-pe smp 4``, your 4-core job will be allocated 4 x 1.5GB = **6GB of RAM** in total. 
  - **Runtime** limits are a measurement of wall-clock time and are not effected by requesting multiple CPU cores. 
  - Multi-threaded jobs can be **interactive, batch** or **array** type. 
  - If you request more CPU cores than your largest node can accommodate, your scheduler will print a warning at submission time but still allow the job to queue (in case a larger node is added to your cluster at a later date). For example:
  
.. code:: bash

    [alces@login1(scooby) ~]$ qsub -pe smp 150 simplejobscript.sh
    warning: no suitable queues
    Your job 58 ("smpjob") has been submitted

.. note:: Requesting more CPU cores in the ``smp`` parallel environment than your nodes actually have may cause your job to queue indefinitely. 


Running Parallel (MPI) jobs
~~~~~~~~~~~~~~~~~~~~~~~~~~~

If users want to use run parallel jobs via an install message passing interface (MPI), they need to inform the scheduler - this allows jobs to be efficiently spread over compute nodes to get the best possible performance. Using multiple CPU cores across multiple nodes is achieved by requesting access to a **Parallel Environment (PE)** - the default MPI PE on your Alces Flight Compute cluster is called **mpislots**. Users wanting to use the **mpislots PE** must request it with a job-scheduler instruction, along with the number of CPU cores they want to use - in the form ``-pe mpislots <number-of-cores>``. This parallel environment is configured to automatically generate an MPI hostfile, and pass it to the MPI using a scheduler integration. 

There is a second parallel environment available which allows users to request complete nodes to participate in MPI jobs - the **mpinodes PE** allows a number of complete nodes to be booked out for jobs that use complete compute nodes at once. Users wanting to use the **mpinodes** PE must request it with a job-scheduler instruction, along with the number of complete nodes they want to use - in the form ``-pe mpinodes <number-of-nodes>``. 

For example, to use 64 CPU cores on the cluster for a single application, the instruction ``-pe mpislots 64`` can be used. The following example shows launching the **Intel Message-passing** MPI benchmark across 64 cores on your cluster. This application is launched via the OpenMPI **mpirun** command - the number of threads and list of hosts to use are automatically assembled by the scheduler and passed to the MPI at runtime. This jobscript loads the **apps/imb** module before launching the application, which automatically loads the module for **OpenMPI**. 

.. code:: bash

    [alces@login1(scooby) ~]$ more runparallel.sh
    #!/bin/bash -l
    #$ -N IMBjob -pe mpislots 64 -o $HOME/imbjob.out.$JOB_ID
    
    module load apps/imb
    mpirun IMB-MPI1

    [alces@login1(scooby) ~]$ qsub runparallel.sh
    Your job 31 ("IMBjob") has been submitted

    [alces@login1(scooby) ~]$ more ~/imbjob.out.31
    #------------------------------------------------------------
    #    Intel (R) MPI Benchmarks 4.0, MPI-1 part
    #------------------------------------------------------------
    # Date                  : Mon May 16 12:54:13 2016
    # Machine               : x86_64
    # System                : Linux
    # Release               : 3.10.0-327.18.2.el7.x86_64
    # Version               : #1 SMP Thu May 12 11:03:55 UTC 2016
    # MPI Version           : 3.0
    # MPI Thread Environment:
    
    # List of Benchmarks to run:
    # PingPong, PingPing, Sendrecv, Exchange, Allreduce, Reduce, Reduce_scatter, Allgather, 
    # Allgatherv, Gather, Gatherv, Scatter, Scatterv, Alltoall, Alltoallv, Bcast, Barrier
    
    #---------------------------------------------------
    # Benchmarking PingPong
    # #processes = 2
    # ( 62 additional processes waiting in MPI_Barrier)
    #---------------------------------------------------
           #bytes #repetitions      t[usec]   Mbytes/sec
                0         1000         7.25         0.00
                1         1000         7.27         0.13
                2         1000         7.29         0.26
                4         1000         7.32         0.52
                8         1000         7.22         1.06
               16         1000         7.40         2.06
    ...
    
.. note:: For debugging purposes, an ``mpislots-verbose`` PE is also provided that prints additional information in your job output file.

For the best experience, please follow these guidelines when running parallel MPI jobs:

  - Alces Flight Compute automatically configures compute nodes with one CPU **slot** per detected CPU core, including hyper-threaded cores. 
  - **Memory limits** are enforced per CPU-core-slot; for example, if your default memory request is 1.5GB and you request ``-pe mpislots 64``, your 64-core job will be allocated ``64 x 1.5GB = 96GB`` of RAM in total, which may be spread over multiple nodes. 
  - **Runtime** limits are a measurement of wall-clock time and are not effected by requesting multiple CPU cores. 
  - Parallel jobs can be interactive, batch or array type. 
  - Parallel applications must use an MPI to handle multi-node communications; the scheduler will prepare nodes for use, but users must use an MPI to launch the application (as shown in the example above). 
  - If you request more CPU cores than your cluster can accommodate, your scheduler will print a warning at submission time but still allow the job to queue (in case more nodes are added to your cluster at a later date). For example:
  
.. code:: bash

    [alces@login1(scooby) ~]$ qsub -pe mpislots 1024 runparallel.sh
    warning: no suitable queues
    Your job 32 ("IMBjob") has been submitted


.. note:: Requesting more CPU cores in the ``mpislots`` parallel environment than your nodes actually have may cause your job to queue indefinitely. If auto-scaling is enabled, the cluster will be expanded over a few minutes to its maximum size in an attempt to add resources to allow your job to run. If you request more resources than your auto-scaling limit will allow, your job may queue indefinitely. 


Requesting more memory
----------------------

Your jobs are restricted to using a maximum amount of memory on the compute node they are executed on. The default memory allocation divides the total amount of RAM per node by the number of available CPU cores - e.g. a cluster that has node with 36 cores and 160GB of RAM will have a default memory allocation of 4.4GB. This allows the job scheduler to efficiently manage resources, ensuring all jobs get enough memory to run without the node running out of memory and crashing. 

If you need more than the default amount of memory for your job, use the ``-l h_vmem=<amount-of-RAM>`` scheduler instruction to request more. For example, to request 32GB of RAM for your single-CPU interactive job, you can use the command ``qrsh -l h_vmem=32G``:

.. code:: bash

    [alces@login1(scooby) ~]$ qrsh -l h_vmem=32G
    
    <<< -[ alces flight ]- >>>
    [alces@ip-10-75-0-128(scooby) ~]$
    

Memory allocations are performed **per scheduler slot** - i.e. per CPU core. So - if you want to request to run an 8-CPU-core multi-threaded job with a total of 64GB of memory, you would request ``-pe smp 8 -l h_vmem=8G`` (as 64GB / 8-cores = **8GB per core**). 

.. note:: Memory allocations are automatically enforced by the job scheduler. If your application exceeds it's memory request, your job will be stopped to prevent crashing the hosting compute node. 


How do I know how much memory to request?
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

It can be difficult for new users to know how much memory their job needs to run effectively. Use the method below to run your job with a high memory limit in order to identify appropriate memory limits to request:

  1. Use the ``qhost`` command to view how much memory your nodes have (**MEMTOT** column).
  2. Submit your job with the maximum memory request for your node type. 
  
     e.g. If your nodes are reported as having 58GB of RAM:
     
        - Submit a single-CPU job with the instruction ``-l h_vmem=58G``
        - Submit a 4-CPU core multi-threaded job with the instruction ``-l h_vmem=14.5G``
        - Submit an 8-CPU core multi-threaded job with the instruction ``-l h_vmem=7.25G``
        etc.
        
  3. Note the job-ID number of your job while it is running, and allow your job to finish normally. 
  4. Use the ``qacct -j <job-ID>`` command to view the scheduler accounting database entry for your job
  5. Look for the entry in the ``qacct`` output that starts **maxvmem** - this will display how much memory your job used when running
  

The next time you submit your job, you can use a smaller memory request for your job, based on the information you gathered from the ``qacct`` output. 

.. note:: A number of parameters can effect how much memory a job uses when running, including the node type and data-set size. Most users round-up their memory requests to the nearest whole GB of RAM. 


Requesting a longer runtime
---------------------------

By default, the scheduler imposes a maximum runtime of 24-hours for jobs submitted on your cluster. This ensures that run-away jobs do not continue processing for long periods of time without generating useful output. Users can request a longer runtime (with no upper limit) by using the ``-l h_rt=<hours>:<mins>:<secs>`` scheduler instruction. For example, to submit a job-script called ``longjob.sh`` with a 72-hour runtime, use the following command:

.. code:: bash

    [alces@login1(scooby) ~]$ qsub -l h_rt=72:0:0 longjob.sh
    Your job 39 ("longjob.sh") has been submitted


Job-script templates
--------------------

Your Alces Flight Compute cluster includes a number of job-script templates for common types of non-interactive jobs. The templates come complete with a range of different scheduler instructions built-in and are designed to use as the basis of your own job-scripts. 

To view the available template for your Flight Compute cluster, use the ``alces template list`` command:

.. code:: bash

    [alces@login1(scooby) ~]$ alces template list
     1 -> mpi-nodes    ... MPI multiple node
     2 -> mpi-slots    ... MPI multiple slot
     3 -> simple-array ... Simple serial array
     4 -> simple       ... Simple serial
     5 -> smp          ... SMP multiple slot

Templates can be previewed using their number or description - e.g. to look at the template for an array job based on the output above, use the command ``alces template show simple-array``. 

To create a new job-script based on a template, use the ``alces template copy <template-name> <job-script-filename>`` command; e.g. 

.. code:: bash

    [alces@login1(scooby) ~]$ alces template copy smp mysmpjob.sh
    alces template copy: template 'smp' copied to 'mysmpjob.sh'
    
    [alces@login1(scooby) ~]$ nano mysmpjob.sh
       <edit template to add include details of my application>
    
    [alces@login1(scooby) ~]$ qsub mysmpjob.sh
    Your job 41 ("mysmpjob.sh") has been submitted
    
    [alces@login1(scooby) ~]$ qstat
    job-ID  prior   name       user         state submit/start at     queue                          slots ja-task-ID
    -----------------------------------------------------------------------------------------------------------------
         41 2.40234 mysmpjob.s alces        r     05/15/2016 13:39:34 byslot.q@ip-10-75-0-114.eu-wes     2
    



Trouble-shooting
----------------

Your cluster job-scheduler is capable of running complex workflows, utilising advanced features to control every facet of running your jobs. It's worth reading through the job-script template to look at the common option available, and trying out different options before running production jobs on your cluster.

If you do run into problems, the ``qstat -j <job-id>`` command can be useful - as well as showing you the instructions you passed the scheduler with your job, the output of this command will also show you the current environment settings for your job, and list scheduling information. This can provide you with assistance to debug issues, and explain why jobs are still queuing when you think they should be running. 

Be patient with the job-scheduler if you have auto-scaling enabled - queuing jobs cannot start until new compute nodes have succesfully joined the cluster; the speed of scaling-up the cluster is governed by the performance of your Cloud provider, and the amount you've paid for your instance types. 


Further documentation
--------------------- 

We have just scratched the surface of using a cluster job-scheduler, and there are many more features and options than described here. A wide range of documentation available both on your Flight Compute cluster and online; 

 - Use the ``man qsub`` command for a full list of scheduler instructions
 - Use the ``man qstat`` command to see the available reporting options for running jobs
 - Use the ``man qacct`` command to see options for accessing the job-accounting database
 - Online documentation for the Grid-Engine scheduler series is `available here <http://wiki.gridengine.info/wiki/index.php/Main_Page>`_


Customising your job-scheduler
------------------------------

Your Alces Flight Compute cluster has been pre-configured with default queues, parallel-environments and resource limits based on a known working set used internationally by HPC sites and research organisations. They have been determined to deliver a good balance of safety and flexibility, and are designed to introduce concepts such as requesting resources which are commonplace for many HPC facilities. 

However - your personal Flight Compute cluster can be modified to suit whatever you need it to do. There are no right and no wrong answers here - you have full control over your facility to do whatever you want. Your login user is authorized to make configuration changes to the scheduler as desired - you can also use the ``sudo`` command to become the root-user to make any other changes you require. 

There is a graphical administration interface for the OGS scheduler - to use it, follow these instructions:

  1. Install the **Motif** software package and fonts needed by the OGS GUI; use the command ``sudo yum install motif xorg-x11-fonts-*``
  2. Use the ``alces session start gnome`` command to start a graphical desktop session, if you don't already have one.
  3. Start the graphical interface using the command ``qmon``
  
Be aware that any job-scripts you have already created (including the provided templates) and cluster auto-scaling support (if available) may rely to the default configuration delivered with your Flight Compute cluster. If you reconfigure the scheduler, we recommend that you disable auto-scaling (``alces configuration autoscaling disable``) and review your job-scripts for compatibility. 




