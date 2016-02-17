.. _cfn-deploy-3-node-sge-cluster:

Deploy a 3 node Alces HPC environment using AWS CloudFormation
==============================================================

The following steps will provide you with an Alces Clusterware SGE
cluster complete with Gridware applications and all required AWS
networking services. The cluster is comprised of a single user
accessible login node and SGE compute nodes, with customisable instance
types.

**Prerequisites**

-  EU-IRELAND region selected
-  AWS key pair previously uploaded
-  Appropriate IAM permissions (primary user preferable)

Deploying via AWS web interface
-------------------------------

1.  Navigate to the CloudFormation console
2.  Click ``Create Stack``
3.  Choose the ``Specify an S3 URL`` option - enter the following URL:
    ``https://s3-eu-west-1.amazonaws.com/clusterware/cloudformation/1.0.2/3-node-cluster.json``
4.  Click ``Next``
5.  Enter a ``stack name``, for example ``ResearchCluster1``
6.  Select your desired AWS key pair from the list of available keys.
    This is used to access the cluster administrator account
    (``alces``). If no AWS key pair is selected - the cluster will fail
    to create.
7.  Enter your chosen ``cluster name``, for example ``research-cluster``
8.  Select the ClusterWare GridWare depot to install, depots install a
    pack of applications e.g. ``benchmark`` installs popular
    benchmarking utilities
9.  Select the instance type to deploy (``small`` or ``large``). Small
    nodes each have 1 core and 2GB memory, large nodes each have 2 cores
    and 8GB memory.
10. Enter a ``NetworkCIDR`` if you would like to restrict access to your
    environment for increased security
11. Click ``Next`` to go to the ``Tags`` page.
12. On the ``Tags`` page - click ``Next`` again to begin creating your
    stack. Once complete - the ``Outputs`` tab will display the cluster
    login nodes public IP address.

Access your environment
-----------------------

1. Navigate to the CloudFormation console
2. Select the created CloudFormation stack
3. Select the ``Output`` tag to locate the master node public IP address
4. SSH to the public IP address as the ``alces`` administrator account -
   together with your previously selected AWS key. E.g.
   ``ssh alces@50.0.0.50``

Using your environment
----------------------

See the `environment usage <environment-usage>`_ page for more information on getting started with your Alces compute environment. 

Destroying your environment
---------------------------

1. Navigate to the CloudFormation console
2. Select the created CloudFormation stack
3. Choose ``Delete Stack``
4. All previously created resources are destroyed and cleaned up
