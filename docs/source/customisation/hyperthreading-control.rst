.. hyperthreading-control:

Hyperthreading Control
======================

Some Cloud instances are configured with "virtual" CPU cores. For each physical processor core, the OS addresses a second virtual core - this is known as `Hyperthreading <https://en.wikipedia.org/wiki/Hyper-threading>`_. In many cases, your application may suffer a performance drop when Hyperthreading is enabled. Alces Flight Compute includes the necessary tools to easily and dynamically disable Hyperthreading on each of your instances.

Checking the status of Hyperthreading
-------------------------------------

You can check whether a node has Hyperthreading enabled or disabled by running the following command, which returns whether or not the node is;

* Hyperthreading-capable or not
* Hyperthreading is enabled or disabled

You can view the status of your instance(s) with the following command: 

.. code:: bash

  [alces@node-xf5(vlj) ~]$ alces configure hyperthreading
  alces configure hyperthreading: hyperthreading is enabled

Disabling Hyperthreading
------------------------

You can disable Hyperthreading on your instances using the following command. This will halve the amount of cores available to your instance. To see the number of cores available to your instance you can run the following example command:

.. code:: bash

  [alces@node-xf5(vlj) ~]$ cat /proc/cpuinfo | grep processor | wc -l
  8

You can then disable Hyperthreading and check it was successful using the following commands:

.. code:: bash

  [alces@node-xf5(vlj) ~]$ alces configure hyperthreading disable
  alces configure hyperthreading: hyperthreading disabled
  [alces@node-xf5(vlj) ~]$ cat /proc/cpuinfo | grep processor | wc -l
  4
