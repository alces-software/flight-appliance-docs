.. _mpiapps:


Parallel (MPI) Applications
===========================

What is an MPI?
---------------

High-performance compute clusters are often used to run *Parallel Applications* - i.e. software applications which simultaneously use resources from two or more computers at the same time. This can allow software programs to run bigger jobs, run them faster, and to work with larger data-sets than can be processed on a single computer. Parallel programming is hard - developing software to run on a single computer is difficult enough, but extending applications to run across multiple computers at the same means doing many more internal checks while your program is running to make sure your software runs correctly, and to deal with any errors that occur. 

A number of standards for parallel programming have been produced to assist software developers in this task. These published standards are often accompanied by an implementation of a software application programming interface (API) which presents a number of standard methods for parallel communication to software developers. By writing their software to be compatible with a published API, software developers can save time by relying on the API to deal with the parallel communications themselves (e.g. transmitting messages, dealing with errors and timeouts, interfacing with different hardware, etc.). The APIs for parallel processing are commonly known as message-passing interfaces (MPIs). 


What MPIs are available?
------------------------

A number of different MPIs are under active development; for Linux clusters, there are a number of common versions available, including:

 - `OpenMPI <https://www.open-mpi.org/>`_; a modern, open-source implementation supporting a wide array of hardware, Linux distributions and applications
 - `MPICH <https://www.mpich.org/>`_; an older open-source implementation largely superceeded by OpenMPI, but still available for compatibility reasons
 - `MVAPICH <mvapich.cse.ohio-state.edu/>`_; an open-source MPI supporting verbs transport across Infiniband fabrics
 - `Intel MPI <https://software.intel.com/en-us/intel-mpi-library>`_; a commercial MPI optimised for Intel CPUs and interconnects
 - `IBM Platform MPI <http://www-03.ibm.com/systems/uk/platformcomputing/products/mpi/>`_; a commercial MPI optimised for particular commercial applications and interconnects

The choice of which MPI to use for any particular use-case can depend on the application you want to run, the hardware you have available to run it on, if you have a license for a commercial application, and many other factors. Discussion and comparison of the available MPIs is outside the scope of this documentation - however, it should be possible to install and run any application that supports your underlying platform type and Linux distribution on an Alces Flight Compute cluster. 


How do I use an MPI?
--------------------

Most MPIs are distributed as a collection of:
 
 - Software libraries that your application is compiled against
 - Utilities to launch and manage an MPI session
 - Documentation and integrations with application and scheduler software
 
You can use your Alces Flight Compute cluster to install the MPI you want to use, then compile and install the software application to be run on the cluster. Alternatively, users can install their own MPI and application software manually into the ``/opt/apps/`` directory of the cluster. 

To run a parallel application, users typically start a new MPI session with parameters which instruct the MPI which nodes to include in the job, and which application to run. Each MPI requires parameters to be specified in the correct syntax - most also require a list of the compute nodes that will be participating in the job to be provided when a new session is started.


Running an MPI job via the cluster scheduler
--------------------------------------------

Most users utilise the cluster job-scheduler to orchestrate launching of parallel jobs. The job-scheduler is responsible for identifying which nodes will be participating in the parallel job, and passing that information on to the MPI. When an MPI is installed on your Alces Flight Compute cluster using the ``alces gridware`` command, an integration for your chosen job-scheduler is automatically installed and configured at the same time. Please see the next section of this documentation for more information on launching a parallel job via your cluster job-scheduler. 


Running an MPI job manually
---------------------------

In some environments, users may wish to manually run MPI jobs across the compute nodes in their cluster without using the job-scheduler. This can be useful when writing and debugging parallel applications, or when running parallel applications which launch directly on compute nodes without requiring a scheduler. A number of commercial applications may fall into this category, including Ansys Workbench, Ansys Fluent, Mathworks Matlab and parallelised R-jobs.

The example below demonstrates how to manually run the **Intel Message-passing Benchmark** application through OpenMPI on an Alces Flight Compute cluster. The exact syntax for your application and MPI may vary, but users should be able to follow the concepts discussed below to run their own software. You will need at least two compute nodes available to run the following example.

.. note:: Before running applications manually on compute nodes, verify that auto-scaling of your cluster is not enabled. Auto-scaling typically uses job-scheduler information to control how many nodes are available in your cluster, and should be disabled if running applications manually. Use the command ``alces configure autoscaling disable`` command to turn off autoscaling before attempting to run jobs manually. 

 1. Install the application and MPI you want to run. The **benchmarks** software depot includes both **OpenMPI** and **IMB** applications, so install and enable that by running these commands:
 
     - ``alces gridware depot install benchmark``
     - ``alces gridware depot enable benchmark``
     
 2. Create a list of compute nodes to run the job on. The following command will use your **genders** group to create a hostfile:
 
     - ``cd ; nodeattr -n nodes > mynodesfile``
     
 3. Load the module file for the **IMB** application; this will also load the **OpenMPI** module file as a dependency. Add the module file to load automatically at login time:
 
.. code:: bash

    [alces@login1(defiant) ~]$ module initadd apps/imb
    [alces@login1(defiant) ~]$ module load apps/imb
    apps/imb/4.0/gcc-4.8.5+openmpi-1.8.5
     | -- libs/gcc/system
     |    * --> OK
     | -- mpi/openmpi/1.8.5/gcc-4.8.5
     |    | -- libs/gcc/system ... SKIPPED (already loaded)
     |    * --> OK
     |
     OK

 4. Start the parallel application in a new **mpirun** session, with the following parameters:
 
     - ``-np 2`` - use two CPU cores in total 
     - ``-npernode 1` - place a maximum of one MPI thread on each node
     - ``-hostfile mynodesfile`` - use the list of compute nodes defined in the file ``mynodesfile`` for the MPI job
     - ``$IMBBIN/IMB-MPI1`` - run the binary **IMB-MPI1**, located in the ``$IMBBIN`` directory configured by the ``apps/imb`` module
     - ``PingPong`` - a parameter to the **IMB-MPI1** application, this option instructs it to measure the network bandwidth and latency between nodes
     
.. code:: bash

    [alces@login1(defiant) ~]$ mpirun -np 2 -npernode 1 -hostfile mynodesfile $IMBBIN/IMB-MPI1 PingPong
    
     benchmarks to run PingPong
    #------------------------------------------------------------
    #    Intel (R) MPI Benchmarks 4.0, MPI-1 part
    #------------------------------------------------------------
    # Date                  : Sat May 14 15:37:49 2016
    # Machine               : x86_64
    # System                : Linux
    # Release               : 3.10.0-327.18.2.el7.x86_64
    # Version               : #1 SMP Thu May 12 11:03:55 UTC 2016
    # MPI Version           : 3.0
    # MPI Thread Environment:
            
    # Calling sequence was:  
    # /opt/gridware/depots/2fe5b915/el7/pkg/apps/imb/4.0/gcc-4.8.5+openmpi-1.8.5/bin//IMB-MPI1 PingPong
    
    # Minimum message length in bytes:   0
    # Maximum message length in bytes:   4194304
    #
    # MPI_Datatype                   :   MPI_BYTE
    # MPI_Datatype for reductions    :   MPI_FLOAT
    # MPI_Op                         :   MPI_SUM
    #
    
    # List of Benchmarks to run:
    # PingPong
    
    #---------------------------------------------------
    # Benchmarking PingPong
    # #processes = 2
    #---------------------------------------------------
           #bytes #repetitions      t[usec]   Mbytes/sec
                0         1000         3.37         0.00
                1         1000         3.22         0.30
                2         1000         3.89         0.49
                4         1000         3.96         0.96
                8         1000         3.99         1.91
               16         1000         3.87         3.95
               32         1000         3.90         7.83
               64         1000         3.91        15.59
              128         1000         4.62        26.44
              256         1000         4.86        50.19
              512         1000         5.89        82.95
             1024         1000         6.08       160.58
             2048         1000         6.98       279.72
             4096         1000        10.35       377.26
             8192         1000        17.43       448.32
            16384         1000        31.13       501.90
            32768         1000        56.90       549.22
            65536          640        62.37      1002.09
           131072          320       127.54       980.10
           262144          160       230.23      1085.88
           524288           80       413.88      1208.08
          1048576           40       824.77      1212.45
          2097152           20      1616.90      1236.93
          4194304           10      3211.40      1245.56
    
    # All processes entering MPI_Finalize
