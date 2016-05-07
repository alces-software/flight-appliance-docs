.. _run-an-interactive-application:

Run an interactive application
==============================

How to run an interactive application using your Alces Flight Compute environment 

The following guide will teach you how to gain an interactive session on a compute node, then run an application interactively

Prerequisites
-------------

- Alces Flight Compute environment deployed 
-  At least 1 compute node deployed to the environment
- ``benchmark`` Gridware depot installed

Running an interactive application
----------------------------------

Once access has been gained to the Alces Flight login node - gain an
interactive session on one of the compute nodes, with all of that nodes
resources using the ``qrsh`` command:

.. code:: bash 

    [alces@login1(clusterware) ~]$ qrsh -pe mpinodes-verbose 1
    Warning: Permanently added '[node1.clusterware.alces.network]:60692,[192.168.150.183]:60692' (ECDSA) to the list of known hosts.
           `.:/+/
       ./oooo+`
     `/oooooo.                       Welcome to flight-cluster
     /oooooo/
     ooooooo-  ./o/                Alces Clusterware (r2016.2)
     +oooooo/`+ooo       Based on CentOS Linux 7.2.1511 (Core)
     -oooooooooooo
      :ooooooooooo. `:+:`
       -+ooooooooo+:ooo`
        `:ooooooooooooo.                               `.
          `:+oooooooooo+`                              /o/
            `-+ooooooooo+-                 :.   .+/   -ooo:
               .:+oooooooo+-`            .+o: `:ooo  :oooo+
                  `-/oooooooo/..-....-:/+ooo//oooo+/+ooooo/
                      ./oooooooo+oooooooooooooooooooooooo/`
                        .+oooooooooo++oooooooooooooooo+:.
                          .:/+ooooooo-`..--::::::::-.`
                                `-:/oo+
                                   `-.  -[ alces flight ]-
    TIPS:

    'module avail'            - show available application environments
    'module add <modulename>' - add a module to your current environment
    
    'alces gridware'          - manage software for your environment
    'alces howto'             - guides on how to use your research environment
    'alces session'           - start and manage interactive sessions
    'alces storage'           - configure and address storage facilities
    'alces template'          - tailored job script templates
    
    'qstat'                   - show summary of running jobs
    'qsub'                    - submit a job script
    'qdesktop'                - submit an interactive session request
    
    'aws help'                - show help for AWS CLI

Once the interactive session has been gained - load the ``memtester``
Gridware application:

.. code:: bash

    [alces@node1(clusterware) ~]$ module load apps/memtester/4.3.0/gcc-4.8.5 
    apps/memtester/4.3.0/gcc-4.8.5
     | -- libs/gcc/system
     |    * --> OK
     |
     OK

Next - run the ``memtester`` application, testing the available memory
on the compute node. In this example we will test 7168MB of memory, 12
times:

.. code:: bash

    [alces@node1(clusterware) ~]$ free -mh
                  total        used        free      shared  buff/cache   available
    Mem:           7.6G        135M        7.1G         16M        419M        7.3G
    Swap:            0B          0B          0B
    [alces@node1(clusterware) ~]$ memtester 7168 12
    memtester version 4.3.0 (64-bit)
    Copyright (C) 2001-2012 Charles Cazabon.
    Licensed under the GNU General Public License version 2 (only).

    pagesize is 4096
    pagesizemask is 0xfffffffffffff000
    want 7168MB (7516192768 bytes)
    got  955MB (1002348544 bytes), trying mlock ...locked.
    Loop 1/12:
      Stuck Address       : ok         
      Random Value        : ok
      Compare XOR         : ok
      Compare SUB         : ok
      Compare MUL         : ok
      Compare DIV         : ok
      Compare OR          : ok
      Compare AND         : ok
      Sequential Increment: ok
      Solid Bits          : ok         
      Block Sequential    : testing   8

Many applications can be run interactively in the method described
above.

