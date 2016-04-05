.. _alces-storage-overview:

Alces Storage Overview
======================

Alces ClusterWare: Storage provides simple but powerful management of your local and remote storage targets including file and object storage. The Alces Storage utility assists with the configuration of your storage targets - and provides a single interface to managing multiple types of storage.

Using the Alces Storage utility allows you to interact with multiple storage platforms under a single simple, but feature-rich interface - abstracting the underlying storage platform. The Alces Storage utility allows you to easily work with multiple storage tiers both interactively and within your automated workloads using the same familiar command set.

The Alces Storage utility currently provides support for:

-  Amazon S3 object storage
-  Dropbox object storage
-  Local POSIX storage

The following guides will assist you in configuring and using your local and remote storage targets, and some of the features available.

File storage
------------

* :ref:`File storage configuration <alces-storage-file-config>`
* :ref:`File storage usage <alces-storage-file-usage>`

Object storage
--------------

* :ref:`Object storage configuration <alces-storage-object-config>`
* :ref:`Object storage usage <alces-storage-object-usage>`

Why?
----

Unlike traditional High Performance Computing - there are typically no large scratch storage areas available - Cloud Computing requires you to localize your data to the machine - making use of its local disk for processing; then uploading results or output data back to more cost-effective storage tiers such as Amazon S3. Alces Storage is provided to easily interact with multiple storage tiers, including local scratch disks and a range of external object storage providers. 
