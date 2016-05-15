.. _sge:


Open Grid Scheduler (SGE)
=========================

The `Open Grid Scheduler (OGS) <http://gridscheduler.sourceforge.net/>`_ cluster job-scheduler is an open-source implementation of the populate Sun Grid-Engine (SGE) codebase, with recent patches and updates to support newer Linux distributions. The syntax and usage of commands is identical to historical SGE syntax, and users can typically from one to another with no issues. This documentation provides a guide to using OGS on your Alces Flight Compute cluster to run different types of jobs. 

See :ref:`jobschedulers` for a description of the different use-cases of a cluster job-scheduler. 

Running an Interactive job
-------------------------- 

You can start a new interactive job on your Flight Compute cluster by using the ``qrsh`` command; the scheduler will search for an available compute node, and provide you with an interactive login shell on the node if one is available. 

.. code:: bash

    [alces@login1(defiant) ~]$ qrsh
    Warning: Permanently added '[ip-10-75-0-21.eu-west-1.compute.internal]:53024,[10.75.0.21]:53024' (ECDSA) to the list of known hosts.

    <<< -[ alces flight ]- >>>
    [alces@ip-10-75-0-21(defiant) ~]$ hostname -f
    ip-10-75-0-21.eu-west-1.compute.internal
    
    [alces@ip-10-75-0-21(defiant) ~]$ module load apps/R
    
    [alces@ip-10-75-0-21(defiant) ~]$ R
    
    R version 3.2.3 (2015-12-10) -- "Wooden Christmas-Tree"
    Copyright (C) 2015 The R Foundation for Statistical Computing
    Platform: x86_64-pc-linux-gnu (64-bit)
    > 

Alternatively, the ``qrsh`` command can also be executed from an interactive desktop session; the job-scheduler will automatically find an available compute node to launch the job on. Applications launched from the qrsh session are executed on the compute cluster node:

.. image:: interactivejob.jpg
     :alt: Running an interactive graphical job
     
When you've finished running your application, simply type ``logout``, or press **CTRL+D** to exit the interactive job. 

If the job-scheduler could not satisfy the resources you've requested for your interactive job (e.g. all your available compute nodes are busy running other jobs), it will report back after a few seconds with an error:

.. code:: bash

    [alces@login1(defiant) ~]$ qrsh 
    Your "qrsh" request could not be scheduled, try again later.

You can force an interactive job to queue for available resources by adding the parameter ``-now no`` to your ``qrsh`` command. 

Submitting a batch job
----------------------

Batch (or non-interactive) jobs allow users to leverage one of the main benefits of having a cluster scheduler; jobs can be queued up with instructions on how to run them and then executed across the cluster while the user does something else. Users submit jobs as scripts, which include instructions on how to run the job - the output of the job (stdout and stderror in Linux terminology) is written to files on disk for review later on. You can write a batch job that does anything that can be typed on the command-line. 

We'll start with a basic example - the following script is written in bash (the default Linux command-line interpreter). You can create the script yourself using the `Nano <http://www.howtogeek.com/howto/42980/the-beginners-guide-to-nano-the-linux-command-line-text-editor/>`_ command-line editor - use the command ``nano simplejobscript.sh`` to create a new file, then type in the contents below. The script does nothing more than print some messages to the screen (the **echo** lines), and sleeps for 120 seconds. We've saved the script to file called ``simplejobscript.sh`` - the ``.sh`` extension helps to remind us that this is a **sh**ell script, but isn't strictly necessary for Linux. 

.. code:: bash
    
    #!/bin/bash
    echo "Starting running on host $HOSTNAME"
    sleep 120
    echo "Finished running - goodbye from $HOSTNAME"
    
We can execute that script directly on the login node by using the command ``bash simplejobscript.sh`` - after a couple of minutes, we get the following output:

.. code:: bash

    Starting running on host login1
    Finished running - goodbye from login1

To submit your jobscript to the cluster job scheduler, use the command ``qsub simplejobscript.sh``. The job scheduler should immediately report the unique job-ID for your job; your job-ID is unique for your current Alces Flight Compute cluster - it will never be repeated once used.

.. code:: bash

    [alces@login1(defiant) ~]$ qsub simplejobscript.sh
    Your job 3 ("simplejobscript.sh") has been submitted

    [alces@login1(defiant) ~]$
    

Viewing and controlling queued jobs
-----------------------------------

Once your job has been submitted, use the ``qstat`` command to view the status of the job queue. If you have available compute nodes, your job should be shown in ``r`` (running) state; if your compute nodes are busy, or you've launched an auto-scaling cluster and currently have no running nodes, your job may be shown in ``qw`` (queuing/waiting) state until compute nodes are available to run it. 

.. code:: bash

    [alces@login1(defiant) ~]$ qstat
    job-ID  prior   name       user         state submit/start at     queue                          slots ja-task-ID
    -----------------------------------------------------------------------------------------------------------------
         3 11.02734 simplejobs alces        r     05/15/2016 09:32:54 byslot.q@ip-10-75-0-131.eu-wes     1       


You can keep running the ``qstat`` command until your job finishes running and disappears from the queue. The output of your batch job will be stored in a file for you to look at. The default location to store the output file is your home-directory - the output file will be named in the format ``<jobscript-name>.o<job-ID>``. So - in the example above, our jobscript was called ``simplejobscript.sh`` and the job-ID was ``3``, so our output file is located at ``~/simplejobscript.sh.o3``. You can use the Linux ``more`` command to view your output file:

.. code:: bash
  
    [alces@login1(defiant) ~]$ more ~/simplejobscript.sh.o3
    Starting running on host ip-10-75-0-131.eu-west-1.compute.internal
    Finished running - goodbye from ip-10-75-0-131.eu-west-1.compute.internal


Your job runs on whatever node the scheduler can find which is available for use - you can try submitting a bunch of jobs at the same time, and using the ``qstat`` command to see where they run. The scheduler is likely to spread them around over different nodes in your cluster (if you have multiple nodes). The login node is not included in your cluster for scheduling purposes - jobs submitted to the scheduler will only be run on your cluster compute nodes. You can use the ``qdel <job-ID>`` command to delete a job you've submitted, whether it's running or still in queued state.

.. code:: bash
    
    [alces@login1(defiant) ~]$ qsub simplejobscript.sh
    Your job 4 ("simplejobscript.sh") has been submitted
    [alces@login1(defiant) ~]$ qsub simplejobscript.sh
    Your job 5 ("simplejobscript.sh") has been submitted
    [alces@login1(defiant) ~]$ qsub simplejobscript.sh
    Your job 6 ("simplejobscript.sh") has been submitted
    [alces@login1(defiant) ~]$ qsub simplejobscript.sh
    Your job 7 ("simplejobscript.sh") has been submitted
    [alces@login1(defiant) ~]$ qsub simplejobscript.sh
    Your job 8 ("simplejobscript.sh") has been submitted
    [alces@login1(defiant) ~]$ qstat
    job-ID  prior   name       user         state submit/start at     queue                          slots ja-task-ID
    -----------------------------------------------------------------------------------------------------------------
          4 11.15234 simplejobs alces        r     05/15/2016 09:43:48 byslot.q@ip-10-75-0-117.eu-wes     1       
          5 11.02734 simplejobs alces        r     05/15/2016 09:43:49 byslot.q@ip-10-75-0-126.eu-wes     1       
          6 11.02734 simplejobs alces        r     05/15/2016 09:43:49 byslot.q@ip-10-75-0-131.eu-wes     1       
          7 11.02734 simplejobs alces        r     05/15/2016 09:43:49 byslot.q@ip-10-75-0-154.eu-wes     1       
          8 11.02734 simplejobs alces        r     05/15/2016 09:43:49 byslot.q@ip-10-75-0-199.eu-wes     1       
 
    [alces@login1(defiant) ~]$ qdel 8
    alces has registered the job 8 for deletion


Viewing compute host status
---------------------------

Users can use the ``qhost`` command to view the status of compute node hosts in your Flight Compute cluster. 

.. code:: bash

    [alces@login1(defiant) ~]$ qhost
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
-----------------

In order to promote efficient usage of your cluster, the job-scheduler automatically sets a number of default resources to your jobs when you submit them. These defaults must be overridden by users to help the scheduler understand how you want it to run your job - if we don't include any instructions to the scheduler, then our job will take the defaults shown below:

 - Number of CPU cores for my job: ``1``
 - Maximum job runtime (in hours): ``24``
 - Output file location: ``~/<jobscript-name>.o<jobID>``
 - Output file style: ``stdout`` and ``stderr`` merged in a single file
 - Amount of memory for my job: The arithmetic sum of ``total memory per node / total cores per node``
      e.g. With 36 core nodes that have 60GB of RAM, the default memory per job is set to around 1.5GB
      
This documentation will explain how to change these limits to suit the jobs that you want to run. You can also disable these limits if you prefer to control resource allocation manually by yourself.

.. note:: Scheduler limits are automatically enforced - e.g. if your job exceeds the requested runtime or memory allocation, it will automatically be stopped. 


viewing queue and host status
-----------------------------

deleting jobs from the queue
----------------------------

giving job-scheduler instructions
---------------------------------
(on command-line and in job-scripts)

simple directive examples
-------------------------

Requesting more resources 
-------------------------

Requesting more CPU cores
--------------

Requesting more memory
----------------------
 (including using qacct to find out how much has been used)

Longer runtime
--------------
 (including why you'd want to specify this)


Using the alces template command
--------------------------------

Using the graphical admin interface
-----------------------------------

.. code:: bash
    
    sudo yum install motif xorg-x11-fonts-*
qmon


All available directives
------------------------ 
Documentation of all available scheduler directives



