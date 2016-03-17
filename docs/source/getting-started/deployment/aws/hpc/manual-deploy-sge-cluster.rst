.. _manual-deploy-sge-cluster:

Manually deploy an SGE compute cluster using AWS
======================================================
The following steps will detail how to create an SGE compute cluster complete with dedicated compute instances using the EC2 console.

**Prerequisites**

-  EU-IRELAND region selected
-  AWS key pair previously uploaded
-  Appropriate IAM permissions (primary user preferable)
-  VPC network with appropriate security groups available for use

Deploying the master node
-------------------------
To deploy the master node, doubling as the cluster login node - navigate to the EC2 console.

* Navigate to the ``Services -> EC2`` page
* Click ``Launch Instance``
* **Step 1: Choose an Amazon Machine Image (AMI)**

  * Select the **Community AMIs** tab
  * Enter the AMI ID ``ami-3758e244``
  * Click ``Select`` to continue to the next step
* **Step 2: Choose an instance type**

  * Choose an instance type to deploy
  * Click ``Next: Configure Instance Details`` to continue
* **Step 3: Configure Instance Details**

  * Number of instances: ``1``
  * Purchasing option: *optional - request spot instances if desired*
  * Network: Choose your existing network
  * Subnet: ``No preference``
  * Enable public IP allocation
  * IAM Role: ``None``
  * Shutdown behavior: ``Stop`` - *note: this option is disabled if spot instances are requested*
  * Enable termination protection: ``Disabled``
  * Monitoring: ``Disabled``
  * Tenancy: ``Run on a shared hardware instance`` - *note: this option is disabled if spot instances are requested*
  * *Optional*: Advanced details: Select ``User-data as text``, input the following example user-data. This sets a hostname and fully qualified domain name.

.. code-block:: bash

    #cloud-config
    hostname: login1
    fqdn: login1.mycluster.alces.network

* Click ``Next: Add Storage`` to continue creating the master node

* **Step 4: Add Storage**

  * Leave defaults
  * Add additional EBS volumes if you wish to share additional storage across the environment later on
  * Click ``Next: Tag Instance``

* **Step 5: Tag Instance**

  * Name: ``mycluster-login1``, or enter your own value to identify the instance within your account
  * Click ``Next: Configure Security Group``

* **Step 6: Configure Security Group**

  * Select a security group with all ports outbound enabled and port 22 (SSH) inbound enabled for your IP address
  * Click ``Next: Review``

* **Step 7: Review**

  * Verify the details you have set are correct
  * Click ``Launch``
  * Select your keypair and select ``Launch Instances``

Once the instance has finished creating - log in to the instance and follow the below steps to begin configuring your environment: 

* Once logged in to your login node, run the ``alces configure`` command

  * Enter your preferred clustername, e.g. ``mycluster``
  * The automatically generated UUID and token can be used, however you may enter your own if you wish. Note these values down for future use when deploying compute nodes
  * Enter the ``role``, when deploying a login/master node - select ``master``
  * If deploying a cluster master node, do not enter a ``Master node IP``

Once the Alces Configure tool has finished, the configuration will take place - and alert you once it has finished. You can now deploy cluster compute nodes using the following steps, once the following information has been noted down: 

* Cluster UUID generated in ``alces configure`` tool
* Cluster token generated in ``alces configure`` tool
* Cluster name provided in ``alces configure`` tool
* Private IP address of cluster login node

Deploying cluster compute nodes
-------------------------------
From the EC2 console, repeat the following steps for as many cluster compute nodes you wish to add. 

* Navigate to the ``Services -> EC2`` page
* Click ``Launch Instance``
* **Step 1: Choose an Amazon Machine Image (AMI)**

  * Select the **Community AMIs** tab
  * Enter the AMI ID ``ami-3758e244``
  * Click ``Select`` to continue to the next step
* **Step 2: Choose an instance type**

  * Choose an instance type to deploy
  * Click ``Next: Configure Instance Details`` to continue
* **Step 3: Configure Instance Details**

  * Number of instances: ``1``
  * Purchasing option: *optional - request spot instances if desired*
  * Network: Choose your existing network
  * Subnet: ``No preference``
  * Enable public IP allocation
  * IAM Role: ``None``
  * Shutdown behavior: ``Stop`` - *note: this option is disabled if spot instances are requested*
  * Enable termination protection: ``Disabled``
  * Monitoring: ``Disabled``
  * Tenancy: ``Run on a shared hardware instance`` - *note: this option is disabled if spot instances are requested*
  * Advanced details: Select ``User-data as text``, input the following example user-data. For each deployed compute node, change the ``hostname`` and ``fqdn`` field:

  .. code-block:: bash

      #cloud-config
      hostname: node1
      fqdn: node1.mycluster.alces.network

* Click ``Next: Add Storage`` to continue creating the compute node

* **Step 4: Add Storage**

  * Leave defaults
  * Click ``Next: Tag Instance``

* **Step 5: Tag Instance**

  * Name: ``cluster1-node1``, or enter your own value to identify the instance within your account
  * Click ``Next: Configure Security Group``

* **Step 6: Configure Security Group**

  * Select a security group with all ports outbound enabled and port 22 (SSH) inbound enabled for your IP address
  * Click ``Next: Review``

* **Step 7: Review**

  * Verify the details you have set are correct
  * Click ``Launch``
  * Select your keypair and select ``Launch Instances``

Once the instance has launched - SSH to its public IP address. Once logged in, run the ``alces configure`` tool. 

You will need to provide the previously noted cluster name, UUID, token and cluster master node private IP address. 

Once the ``alces configure`` tool has finished, the node will shortly be registered into the environment, available for job submission. 

Using your environment
----------------------

See the `environment usage <environment-usage>`_ page for more information on getting started with your Alces compute environment. 
