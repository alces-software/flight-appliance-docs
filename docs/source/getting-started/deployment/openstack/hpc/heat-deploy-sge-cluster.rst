.. _heat-deploy-sge-cluster:

Deploy an Alces ClusterWare research compute environment using OpenStack Heat
=============================================================================

The following steps will provide you with an Alces ClusterWare SGE cluster, complete with Gridware applications and all required OpenStack networking services. The cluster is comprised of a single user accessible login node and SGE compute nodes, with customisable instance types. 

**Prerequisites**
 * Alces OpenStack user account
 * Correctly connected to the cluster DMZ network
 * Appropriate resource quota to create the number of compute nodes you choose
 * OpenStack keypair added

Deploying via Horizon web interface
-----------------------------------

1.  Log in to the OpenStack Horizon interface with your site credentials
2.  Navigate to the ``Project -> Orchestration -> Stacks`` page
3.  Select the ``Launch Stack`` button
4.  **Template Source**: Select ``URL`` and enter the following template URL:
  * ``https://raw.githubusercontent.com/alces-software/flight-appliance-support/1.0/openstack-heat/hpc/X-node-cluster.yaml``
5.  **Environment Source**: *Not required* 
6.  Click the ``Next`` button
7.  **Stack Name**: Enter a stack name, this defines the cluster name
8.  **Creation Timeout (minutes)**: Leave default ``60``
9.  **Rollback On Failure**: ``Enabled``
10.  **Password for user**: Enter your OpenStack user password
11.  **Cluster admin key**: Select your OpenStack keypair to assign to the administrator user account
12.  **HPC Image**: Select the Alces Compute image from the list of available images
13.  **Instance Type**: Select the instance type to deploy, this defines the number of cores and memory available to each instance
14.  **Number of compute nodes**: Enter the number of dedicate compute nodes you wish to deploy
15.  **Gridware Depot**: Select the Gridware depot to install. Gridware depots install packs of popular applications for certain use-cases. 
16.  Click the ``Launch`` button. 
17.  Once the stack is in ``Status: Create In Progress`` - enter the stack and navigate to the ``Overview`` tab
18.  Once the stack has finished creating, the ``Overview`` tab will provide you with the cluster public IP address, used to log in with the administrator user ``alces``

Access your environment
-----------------------

1.  From the ``Overview`` tab of your stack, make a note of the cluster master node public IP address displayed, e.g. ``10.77.0.100``
2.  SSH to the public IP address as the ``alces`` administrator account - together with your previously selected OpenStack keypair, e.g. ``ssh -i ~/.ssh/openstack_key.pem alces@10.77.0.100``

Using your environment
----------------------

1. :ref:`Run an interactive application <run-an-interactive-application>`
2. :ref:`Run an MPI job over multiple nodes <run-an-mpi-job>`
3. :ref:`Run a graphical application <run-a-graphical-application>`
4. :ref:`Run storage benchmarks <run-storage-benchmarks>`
5. :ref:`Working with Gridware applications <working-with-gridware-applications>`

Destroying your environment
---------------------------

1.  From the Horizon dashboard, navigate to the ``Project -> Orchestration -> Stacks`` page
2.  Using the dropdown box for your stack name, select ``Delete Stack``
3.  Your stack is now deleted and all resources including any data are destroyed
