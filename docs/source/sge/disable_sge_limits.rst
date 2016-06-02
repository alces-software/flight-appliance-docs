.. _disable_sge_limits:

Disabling resource limits for your job-scheduler
################################################

By default, an Alces Flight Compute cluster enables limits on the resources your jobs can use on the compute nodes of your cluster. These limits are designed to help protect the compute nodes from being overloaded, and help to promote efficient usage of your compute cluster. It is important to train HPC cluster users in requesting the right resources for their jobs, as this is a fundamental principle when working on shared HPC facilities, where users do not have the benefit of exclusive use.

Experienced HPC cluster users working on a personal Alces Flight Compute cluster may prefer to disable the job-scheduler resource limits and manage compute node resources themselves. These instructions provide a guide to disabling the default limits for your compute cluster job scheduler. 

.. note:: Once scheduler resource limits are disabled, users must manually ensure that the compute node instances selected have enough resources to run the jobs submitted to the job-scheduler queue. 

Open-grid scheduler
===================

By default, your Alces Flight Compute cluster sets limits for the following properties of your compute jobs:

 - Number of CPU cores
 - Amount of memory per requested slot (measured per CPU core, or complete node)
 - Run time (measured in seconds of node occupancy)
 
 

Requesting more CPU cores for your job
--------------------------------------

The default number of CPU cores for your job is one. This setting is used in the absence of a Parallel Environment (PE) request and cannot be sensibly over-ridden. Users can however the job-scheduler to request a default parallel-environment by adding a job-scheduler instruction into their local ``$HOME/.sge_request`` file. For example; if a user wanted to ensure that all jobs requested the **SMP** parallel environment with 2 CPU-cores, they could add the following into their ``$HOME/.sge_request`` file:

.. code:: bash

  -pe smp 2
  
.. note:: Job-scheduler instructions provided as parameters to the ``qsub`` command, or included in job-scripts are prioritised over the contents of your ``$HOME/.sge_request`` file. 

Use the command ``man sge_request`` for more information on configuring default settings for job submitted on your compute cluster. 



Disabling memory resource limits
--------------------------------

Users can follow the process below to disable memory limits for jobs running via the cluster job-scheduler.

 1. Use the ``sudo -s`` command on the cluster login node to become the root user.
 
 2. Run the command ``EDITOR=nano qconf -mc`` 
 
 3. Using the ``nano`` editor, change the settings for the ``h_vmem`` entry so that:
     - requestable is set to **NO**
     - consumable is set to **NO**
     - default is set to **0** (zero)
 
 4. Press ``CTRL+X`` to exit the nano editor


Once memory limits are disabled, cluster users can submit jobs without a ``-l h_vmem`` setting, allowing jobs to use as much memory as is available on their cluster compute nodes. 

.. note:: A job submitted via the job-scheduler that uses more memory than your compute node can provide may be automatically killed by Linux, or could in extreme cases cause your compute node to stop responding. Users must manually manage memory resources if memory resource limits are disabled in the cluster job scheduler. 


Disabling the default runtime limit
-----------------------------------

Submitted jobs automatically have a default runtime applied, unless overridden with the ``-l h_rt=`` parameter to ``qrsh`` or ``qsub`` on the command-line, or included as a job-scheduler instruction in your job-script.

Users can optionally disable the default runtime limit for their cluster by following these steps:

 1. Edit the system-wide grid-scheduler defaults file; for example, to use the nano editor, use the command:
 
    ``nano $GRIDSCHEDULERDIR/etc/common/sge_request``
    
 2. Remove the ``h_rt=24:00:00`` section at the end of the file
 
 3. Save the file by pressing ``CTRL+X``
 
 
Once the default runtime limit is disabled, cluster users can submit jobs without a ``-l h_rt=`` setting, allowing jobs to run forever until they complete or are terminated by the user.

.. note:: A job submitted via the job-scheduler without a run-time limit will run forever until it naturally completes or is terminated by the user. Users should review their jobs to ensure that suitable infrastructure has been selected for long-running jobs, and may wish to setup billing alerts via their platform provider to ensure that run-away jobs are identified and stopped promptly. 