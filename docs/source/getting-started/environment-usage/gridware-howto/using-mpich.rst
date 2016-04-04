.. _using-mpich:

Using MPICH with an Alces Flight Compute environment
====================================================

The following guide will detail the basic usage of the MPICH MPI library for Linux, covering both the installation of the MPICH Gridware package as well as basic usage.

Prerequisites
-------------

*  Access to an Alces Flight Compute environment
*  Appropriate administrator access to install Gridware packages

Installing MPICH to your environment
------------------------------------

The MPICH Gridware package is easily installed using the ``alces gridware`` utility. To install ``mpich`` to your environment, run the following command as a privileged user:

.. code:: bash

    [alces@login1(cluster1) ~]$ alces gridware install mpich
    Installing base/mpi/mpich/3.0.4

     > Preparing package sources
            Download --> mpich-3.0.4.tar.gz ... OK
              Verify --> mpich-3.0.4.tar.gz ... OK

     > Preparing for installation
               Mkdir ... OK (/var/cache/gridware/src/mpi/mpich/3.0.4/gcc-4.8.5)
             Extract ... OK

     > Proceeding with installation
             Compile ... OK
               Mkdir ... OK (/opt/gridware/depots/17145e23/el7/pkg/mpi/mpich/3.0.4/gcc-4.8.5)
             Install ... OK
              Module ... OK

Running an example MPICH task: Hello World
------------------------------------------
The following section covers running an example application using MPICH. To begin, create the following file in your home directory - hello-world.c:

.. code:: c

    #include <mpi.h>
    #include <stdio.h>

    int main(int argc, char** argv) {
    MPI_Init(NULL, NULL);
    int world_size;
    MPI_Comm_size(MPI_COMM_WORLD, &world_size);
    int world_rank;
    MPI_Comm_rank(MPI_COMM_WORLD, &world_rank);
    char processor_name[MPI_MAX_PROCESSOR_NAME];
    int name_len;
    MPI_Get_processor_name(processor_name, &name_len);
    printf("Hello world from processor %s, rank %d"
           " out of %d processors\n",
           processor_name, world_rank, world_size);
    MPI_Finalize();
    }

Next - load the previously installed ``MPICH`` Gridware package, for example:

.. code:: bash

    [alces@login1(cluster1) ~]$ module load mpi/mpich
    mpi/mpich/3.0.4/gcc-4.8.5
     | -- libs/gcc/system
     |    * --> OK
     |
     OK

Next, using the ``mpicc`` compiler - compile your Hello World application:

.. code:: bash

    mpicc -o hello-world hello-world.c

Now - run your application using MPICH across 2 compute cores using the ``-np`` (number of processes):

.. code:: bash

    [alces@login1(openfoam) ~]$ mpiexec -np 2 ./hello-world
    Hello world from processor login1, rank 0 out of 2 processors
    Hello world from processor login1, rank 1 out of 2 processors

Your applications can also be run using MPICH on your compute nodes, by loading both your application module together with the MPICH module - and running in parallel with the ``mpiexec <options> appname`` command.
