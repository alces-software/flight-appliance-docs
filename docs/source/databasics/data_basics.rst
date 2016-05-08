.. _data_basics:


Working with data and files
###########################

Organising data on your cluster
===============================

Shared filesystem
----------------- 

Your Flight Compute cluster includes a shared home filesystem which is mounted across the login and all compute nodes. Files copied to this area are available via the same absolute path on all cluster nodes. The size of this area is controlled by the setting used when you created your cluster. The shared filesystem is typically used for job-scripts, input and output data for the jobs you run.

Users must make sure that they copy data they want to keep off the shared filesystem before the Flight Compute cluster is terminated. This documentation provides example methods for copying data back to your local client system, or storing it in the **AWS Simple Storage Service (S3)**. 

Your home directory
-------------------

The shared filesystem includes the home-directory area for the user you created when your cluster was launched. Linux automatically places users in their home-directory when they login to a node. By default, Flight Compute will create your home-directory under the ``/home/`` directory, named after your username. For example, if your user is called **jane**, then your home-directory will have the absolute path ``/home/jane/``.

The Linux command line will accept the ``~`` (tilde) symbol as a substitute for the currently logged-in users' home-directory. The environment variable ``$HOME`` is also set to this value by default. Hence, the following three commands are all equivalent when logged in as the user **jane**:

 - ``ls /home/jane``
 - ``ls ~``
 - ``ls $HOME``
 

The **root** user in Linux has special meaning as a privileged user, and does not have a shared home-directory across the cluster. The **root** account on all nodes has a home-directory in ``/root``, which is separate for every node. For security reasons, users are not permitted to login to a node as the root user directly - please login as a standard user and use the ``sudo`` command to get privileged access. 

 
Local scratch storage
--------------------- 

Your compute nodes have an amount of disk space available to store temporary data under the ``/tmp`` mount-point. The size of this area will depend on the type and size of instance you selected at the time your cluster was launched. This area is intended for temporary data created during compute jobs, and shouldn't be used for long-term data storage. Compute nodes are configured to automatically clear up temporary space automatically, removing orphan data left behind by jobs. In addition, an auto-scaling cluster may automatically terminate idle nodes, resulting in the loss of any files stored in local scratch space. 

Users must make sure that they copy data they want to keep back to the shared filesystem after compute jobs have been completed. 


Copying data between nodes
--------------------------

Flight Compute cluster login and compute nodes all mount the shared filesystem, so it is not normally necessary to copy data directly between nodes in the cluster. Users simply need to place the data to be shared in their home-directory on the login node, and it will be available on all compute nodes in the same location. 

If necessary, users can use the ``scp`` command to copy files from the compute nodes to the login node; for example:

 - ``scp ip-10-75-0-235:/tmp/myfile.txt .``
 
Alternatively, users could login to the compute node (e.g. ``ssh ip-10-75-0-235``) and copy the data back to the shared filesystem on the node:

 - ``ssh ip-10-75-0-235
     cp /tmp/myfile ~/myfile``



Copying data files to the cluster
=================================

Many compute workloads involve processing data on the cluster - users often need to copy data files to the cluster for processing, and retrieve processed data and results afterwards. This documentation describes a number of methods of working with data on your cluster, depending on how users prefer to transfer it.


Using command-line tools to copy data
-------------------------------------

The cluster login node is accessible via SSH, allowing use of the ``scp`` and ``sftp`` commands to transfer data from your local client machine. Linux and Mac users can use in-built SSH support to copy files; e.g.

 - To copy file **mydata.zip** to a Flight Compute cluster on IP address 52.48.62.34:
    ``scp -i mykeyfile.pub mydata.zip jane@52.48.62.34:.``
    
    - replace ``mykeyfile.pub`` with the name of your SSH public key
    - replace ``jane`` with your username on the cluster
    
    
Windows users can download and install the `pscp <http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html>`_ command to perform the same operation:
    ``pscp -i mykeyfile.pub mydata.zip jane@52.48.62.34:/home/jane/.``
    
    
Both the ``scp`` and the ``pscp`` commands take the parameter ``-r`` to recursively copy entire directories of files to the cluster. 

To retrieve files from the cluster, simply specify the location of the remote file first in the ``scp`` command, followed by the location on the local system to put the file; e.g.

 - To copy file **myresults.zip** from a Flight Compute cluster on IP address 52.48.62.34 to your local Linux or Mac client:
    ``scp -i mykeyfile.pub jane@52.48.62.34:/home/jane/myresults.zip .``


Using a graphical client to copy data
-------------------------------------

There are also a number of graphical file-management interfaces available that support the SSH/SCP/SFTP protocols. A graphical interface can make it easier for new users to manage their data, as they provide a simple drag-and-drop interface that helps to visualise where data is being stored. The example below shows how to configure the `WinSCP <https://winscp.net/eng/download.php>`_ utility on a Windows client to allow data to be moved to and from a cluster.

 - On a Windows client, download and install `WinSCP <https://winscp.net/eng/download.php>`_
 - Start WinSCP; in the **login** configuration box, enter the IP address of your Flight Compute cluster login node in the ``Host name`` box
 - Enter the username you configured for your cluster in the ``User name`` box
 - Click on the ``Advanced`` box and navigate to the ``SSH`` sub-menu, and the ``Authentication`` item
 - In the ``Private key file`` box, select your AWS private key, and click the ``OK`` box

.. image:: winscpconfig.jpg
    :alt: Configuring WinSCP
    
 - Optionally click the ``Save`` button and give this session a name
 - Click the ``Login`` button to connect to your cluster
 - Accept the warning about adding a new server key to your cache; this message is displayed only once when you first connect to a new cluster
 - WinSCP will login to your cluster; the window shows your local client machine on the left, and the cluster on the right
 - To copy files to the cluster from your client, click and drag them from the left-hand window and drop them on the right-hand window
 - To copy files from the cluster to your client, click and drag them from the right-hand window and drop them on the left-hand window

.. image:: winscpcopyfiles.jpg
    :alt: Copying files with WinSCP
 
The amount of time taken to copy data to and from your cluster will depend on a number of factors, including:

 - The size of the data being copied
 - The speed of your Internet link to the cluster; if you are copying large amounts of data, try to connect using using a wired connection rather than wireless
 - The type and location of your cluster login node instance
 

Object storage for archiving data
---------------------------------

As an alternative to copying data back to your client machine, users may prefer to upload their data to a cloud-based object storage service instead. Flight Compute clusters include tools for accessing data stored in the ``AWS S3 <https://aws.amazon.com/s3/>`_ object storage service, as well as the ``Dropbox <https://www.dropbox.com/>`_ cloud storage service. Benefits of using an cloud-based storage service include:

 - Data is kept safe and does not have to be independantly backed-up
 - Storage is easily scalable, with the ability for data to grow to practically any size
 - You only pay for what you use; you do not need to buy expansion room in advance
 - Storage service providers often have multiple tiers available, helping to reduce the cost of storing data
 - Data storage and retrieval times may be improved, as storage service providers typically have more bandwidth than individual sites
 - Your institution or facility may receive some storage capacity for free which you could use
 
Object storage is particularly useful for archiving data, as it typically provides a convenient, accessible method of storing data which may need to be shared with a wide group of individuals. 


Using alces storage commands
----------------------------

Your Flight Compute cluster includes command-line tools which can be used to enable access to existing AWS S3 and Dropbox accounts. A Ceph storage platform with a compatible **RADOS-gateway** can also be supported using S3 support. To enable access to these services, users must first enable them with the following commands:

 - ``alces storage enable s3`` - enables **AWS S3** service
 - ``alces storage enable dropbox`` - enables **Dropbox** service
 
Once enabled, a user can configure one or more storage services for use on the command-line, giving each one a friendly name to identify it. The syntax of the command is shown below:

  ``alces storage configure <friendly-name> <type-of-storage>``

For example; to configure access to an AWS S3 account using the access and secret key, the following commands can be used:

  ``
  [alces@login1(scooby) ~]$ alces storage configure my-s3area1 s3
  Display name [my-s3area1]:
  Access key: PZHAA6I2OEDF9F1RQS8Q
  Secret key: ********************
  Service address [s3.amazonaws.com]:
  alces storage configure: storage configuration complete
  ``
  
When configuring a Dropbox account, the user is provided with a URL that must be copied and pasted into a browser session on their local client machine:

<insert info here>

<alces storage examples>


Important of storing files safely before shutting down the cluster
------------------------------------------------------------------

