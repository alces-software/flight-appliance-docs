.. _run-storage-benchmarks:

How to run storage benchmarks on your cluster
=============================================

The following guide will detail how to run storage benchmarks on your
cluster to measure:

-  Write performance metrics to:
-  Shared ``/home`` NFS storage
-  Local compute node scratch storage

The following guide will use both the ``iozone`` GridWare application to
benchmark the storage performance of your ClusterWare environment.

1. `Why should I measure the performance of my
   cluster? <#why-should-i-measure-the-performance-of-my-cluster>`__
2. `Measuring write performance <#measuring-write-performance>`__

Why should I measure the performance of my cluster?
---------------------------------------------------

Public and private Clouds enable you to provision many difference
instance types, often suited to different types of workload - for
example one type of instance may be significantly more geared towards
memory-bound applications.

The requirements of your applications/tasks should be assessed - noting
in particular; maximum memory used per job, CPU requirements (fewer fast
cores or many slower cores), GPU requirements and disk I/O requirements.

Benchmarking your cluster using different compute node instance types
will help to achieve an optomised cluster, resulting in faster
time-to-results and overall reducing the cost of research if using
pay-to-use Clouds.

Prerequisites
-------------

The following prerequisites must have been met in order to follow this
guide:

-  Alces Flight Compute environment deployed
-  SSH access to the environment
-  ``benchmark`` GridWare depot installed
-  Administrator account access 

Measuring write performance
---------------------------

The following section will detail how to measure the write performance
of both shared storage and local compute node scratch storage using both
the ``iozone`` GridWare application. To avoid caching effects - the
total write should always be at least double the total available memory
of the benchmarked machine (if possible).

Shared ``/home`` storage performance
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

The following section will detail how to perform storage benchmarks on
the shared ``/home`` NFS area of your ClusterWare environment from a
remote NFS client.

To begin - SSH to any of the available compute nodes in your
environment, then load the ``iozone`` Gridware application, e.g.:

.. code:: bash

    [alces@node1(clusterware) ~]$ alces module load apps/iozone
    apps/iozone/3.420/gcc-4.8.5
     | -- libs/gcc/system
     |    * --> OK
     |
     OK

Next - run a single threaded IOZone test, preferably writing at least
double the amount of total memory on the compute node, if the disk space
is available in the shared ``/home`` NFS area.

Check the memory and available disk:

.. code:: bash

    [alces@node1(clusterware) ~]$ free -mh
                  total        used        free      shared  buff/cache   available
    Mem:           3.7G        111M        3.1G         16M        456M        3.4G
    Swap:            0B          0B          0B
    [alces@node1(clusterware) ~]$ df -h
    Filesystem                                     Size  Used Avail Use% Mounted on
    /dev/vda1                                       40G  3.7G   37G  10% /
    devtmpfs                                       1.9G     0  1.9G   0% /dev
    tmpfs                                          1.9G     0  1.9G   0% /dev/shm
    tmpfs                                          1.9G   17M  1.9G   1% /run
    tmpfs                                          1.9G     0  1.9G   0% /sys/fs/cgroup
    192.168.150.160:/home                           40G  4.0G   37G  10% /home
    192.168.150.160:/opt/gridware/depots/38730551   40G  4.0G   37G  10% /opt/gridware/depots/38730551
    tmpfs                                          380M     0  380M   0% /run/user/1000

We can see there is 40Gb available in the shared ``/home`` NFS
directory, and 4GB total memory on the compute node.

Run the ``iozone`` Gridware application - optimising for your own
ClusterWare environment. Change the ``-s 8G`` to suit your environment.

.. code:: bash

    [alces@node1(clusterware) ~]$ iozone -i 0 -e -r 1M -s 8G -t 1
        Iozone: Performance Test of File I/O
                Version $Revision: 3.420 $
            Compiled for 64 bit mode.
            Build: linux-AMD64 

        Contributors:William Norcott, Don Capps, Isom Crawford, Kirby Collins
                     Al Slater, Scott Rhine, Mike Wisner, Ken Goss
                     Steve Landherr, Brad Smith, Mark Kelly, Dr. Alain CYR,
                     Randy Dunlap, Mark Montague, Dan Million, Gavin Brebner,
                     Jean-Marc Zucconi, Jeff Blomberg, Benny Halevy, Dave Boone,
                     Erik Habbinga, Kris Strecker, Walter Wong, Joshua Root,
                     Fabrice Bacchella, Zhenghua Xue, Qin Li, Darren Sawyer,
                     Vangel Bojaxhi, Ben England, Vikentsi Lapa.

        Run began: Mon Jan 18 10:10:12 2016

        Include fsync in write timing
        Record Size 1024 KB
        File size set to 8388608 KB
        Command line used: iozone -i 0 -e -r 1M -s 8G -t 1
        Output is in Kbytes/sec
        Time Resolution = 0.000001 seconds.
        Processor cache size set to 1024 Kbytes.
        Processor cache line size set to 32 bytes.
        File stride size set to 17 * record size.
        Throughput test with 1 process
        Each process writes a 8388608 Kbyte file in 1024 Kbyte records

        Children see throughput for  1 initial writers  =  363782.66 KB/sec
        Parent sees throughput for  1 initial writers   =  363772.88 KB/sec
        Min throughput per process          =  363782.66 KB/sec 
        Max throughput per process          =  363782.66 KB/sec
        Avg throughput per process          =  363782.66 KB/sec
        Min xfer                    = 8388608.00 KB

        Children see throughput for  1 rewriters    =  351427.81 KB/sec
        Parent sees throughput for  1 rewriters     =  351421.89 KB/sec
        Min throughput per process          =  351427.81 KB/sec 
        Max throughput per process          =  351427.81 KB/sec
        Avg throughput per process          =  351427.81 KB/sec
        Min xfer                    = 8388608.00 KB



    iozone test complete.

The ``Parent sees throughput for  1 initial writers`` figure is the
result to note down.

Local compute node scratch performance
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

From the compute node - navigate to the local scratch directory
(``/tmp``) - and re-run the IOZone application, again optimising for the
available space and memory, e.g.:

.. code:: bash

    [alces@node1(clusterware) tmp]$ iozone -i 0 -e -r 1M -s 8G -t 1
        Iozone: Performance Test of File I/O
                Version $Revision: 3.420 $
            Compiled for 64 bit mode.
            Build: linux-AMD64 

        Contributors:William Norcott, Don Capps, Isom Crawford, Kirby Collins
                     Al Slater, Scott Rhine, Mike Wisner, Ken Goss
                     Steve Landherr, Brad Smith, Mark Kelly, Dr. Alain CYR,
                     Randy Dunlap, Mark Montague, Dan Million, Gavin Brebner,
                     Jean-Marc Zucconi, Jeff Blomberg, Benny Halevy, Dave Boone,
                     Erik Habbinga, Kris Strecker, Walter Wong, Joshua Root,
                     Fabrice Bacchella, Zhenghua Xue, Qin Li, Darren Sawyer,
                     Vangel Bojaxhi, Ben England, Vikentsi Lapa.

        Run began: Mon Jan 18 10:20:08 2016

        Include fsync in write timing
        Record Size 1024 KB
        File size set to 8388608 KB
        Command line used: iozone -i 0 -e -r 1M -s 8G -t 1
        Output is in Kbytes/sec
        Time Resolution = 0.000001 seconds.
        Processor cache size set to 1024 Kbytes.
        Processor cache line size set to 32 bytes.
        File stride size set to 17 * record size.
        Throughput test with 1 process
        Each process writes a 8388608 Kbyte file in 1024 Kbyte records

        Children see throughput for  1 initial writers  =  359056.06 KB/sec
        Parent sees throughput for  1 initial writers   =  359046.27 KB/sec
        Min throughput per process          =  359056.06 KB/sec 
        Max throughput per process          =  359056.06 KB/sec
        Avg throughput per process          =  359056.06 KB/sec
        Min xfer                    = 8388608.00 KB

        Children see throughput for  1 rewriters    =  405812.44 KB/sec
        Parent sees throughput for  1 rewriters     =  405754.21 KB/sec
        Min throughput per process          =  405812.44 KB/sec 
        Max throughput per process          =  405812.44 KB/sec
        Avg throughput per process          =  405812.44 KB/sec
        Min xfer                    = 8388608.00 KB



    iozone test complete.

