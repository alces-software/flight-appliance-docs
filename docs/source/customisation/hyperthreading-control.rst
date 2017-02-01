.. hyperthreading-control:

Hyperthreading Control
======================

Some Cloud instances are configured with "virtual" CPU cores. For each physical processor core, the OS addresses a second virtual core - this is known as `Hyperthreading <https://en.wikipedia.org/wiki/Hyper-threading>`_. In some cases, your application may suffer a performance drop when Hyperthreading is enabled. Alces Flight Compute includes the necessary tools to easily and dynamically disable Hyperthreading on each of your instances.

Checking the status of Hyperthreading
-------------------------------------

You can check whether a node has Hyperthreading enabled or disabled by running the following command: 

.. code:: bash

  [alces@node-xf5(scooby) ~]$ alces configure hyperthreading
  alces configure hyperthreading: hyperthreading is enabled
  
.. note:: Hyperthreading status should be checked, disabled or enabled on a per-node basis. Users can check all nodes in their cluster by loading the PDSH module and using the pdsh command - e.g. ``module load services/pdsh; pdsh -g cluster alces configure hyperthreading``.


Disabling Hyperthreading
------------------------

Disabling Hyperthreading will dynamically turn off the secondary thread for each physical CPU core which your instance has access to. For most platforms, this will effectively halve the number of CPU cores reported by your instance. To see the current number of cores available to your instance, users can run the following example command:

.. code:: bash

  [alces@node-xf5(scooby) ~]$ cat /proc/cpuinfo | grep processor | wc -l
  8

Users can then disable Hyperthreading and confirm its status using the following commands:

.. code:: bash

  [alces@node-xf5(scooby) ~]$ alces configure hyperthreading disable
  alces configure hyperthreading: hyperthreading disabled
  [alces@node-xf5(scooby) ~]$ cat /proc/cpuinfo | grep processor | wc -l
  4

.. note:: Your cluster job-scheduler periodically checks compute nodes to confirm how many CPU cores are reported as online. After changing the hyperthreading setting for your compute nodes, your job-scheduler may take a few minutes to report the new online CPU count for each node. 