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
  * Enter the AMI ID ``ami-f6b61e85``
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
  * Advanced details: Select ``User-data as text``, input the following example user-data.

.. code-block:: bash

    #cloud-config
    hostname: login1
    fqdn: login1.clusterware.alces.network
    write_files:
    - content: |
        cluster:
          uuid: '7b4249e8-637a-11e5-a343-7831c1c0e63c'
          token: '8R0c6lyMG1pJGP/wzg8dHA=='
          name: 'clusterware'
          role: 'master'
          tags:
            scheduler_roles: ':master:'
          quorum: 3
          gridware:
            depots:
            - name: benchmark
              url: https://s3-eu-west-1.amazonaws.com/packages.alces-software.com/depots/benchmark
        instance:
          users:
          - username: alces-cluster
            uid: 509
            group: alces-cluster
            gid: 509
            groups:
              - gridware
              - admins:388
            ssh_public_key: |
              ssh-rsa 1234 user
      owner: root:root
      path: /opt/clusterware/etc/config.yml
      permissions: '0640'

It is advised to note the :ref:`configuration details <configuration>` - in particular creating your own ``uuid`` and ``token``.

  * Click ``Next: Add Storage`` to continue creating the master node

* **Step 4: Add Storage**

  * Leave defaults
  * Add additional EBS volumes if you wish to share additional storage across the environment later on
  * Click ``Next: Tag Instance``

* **Step 5: Tag Instance**

  * Name: ``cluster1-master``, or enter your own value to identify the instance within your account
  * Click ``Next: Configure Security Group``

* **Step 6: Configure Security Group**

  * Select a security group with all ports outbound enabled and port 22 (SSH) inbound enabled for your IP address
  * Click ``Next: Review``

* **Step 7: Review**

  * Verify the details you have set are correct
  * Click ``Launch``
  * Select your keypair and select ``Launch Instances``

.. note:: Once the instance has launched, note down the cluster master nodes ``PrivateIP`` - this is used to launch the cluster compute nodes. The previously used ``uuid`` and ``token`` should also be noted down for later use.

Deploying cluster compute nodes
-------------------------------
From the EC2 console, repeat the following steps for as many cluster compute nodes you wish to add. It is possible to launch many nodes at once by entering a higher number in the ``Number of instances`` field, however you will not be able to set hostnames such as ``node01``, ``node02``, ``node03``.

* Navigate to the ``Services -> EC2`` page
* Click ``Launch Instance``
* **Step 1: Choose an Amazon Machine Image (AMI)**

  * Select the **Community AMIs** tab
  * Enter the AMI ID ``ami-f6b61e85``
  * Click ``Select`` to continue to the next step
* **Step 2: Choose an instance type**

  * Choose an instance type to deploy
  * Click ``Next: Configure Instance Details`` to continue
* **Step 3: Configure Instance Details**

  * Number of instances: ``1`` *optionally select more instances as previously described*
  * Purchasing option: *optional - request spot instances if desired*
  * Network: Choose your existing network
  * Subnet: ``No preference``
  * Disable public IP allocation
  * IAM Role: ``None``
  * Shutdown behavior: ``Stop`` - *note: this option is disabled if spot instances are requested*
  * Enable termination protection: ``Disabled``
  * Monitoring: ``Disabled``
  * Tenancy: ``Run on a shared hardware instance`` - *note: this option is disabled if spot instances are requested*
  * Advanced details: Select ``User-data as text``, input the following example user-data. For each deployed compute node, change the ``hostname`` and ``fqdn`` field:

  .. code-block:: bash

      #cloud-config
      hostname: node1
      fqdn: node1.clusterware.alces.network
      write_files:
      - content: |
          cluster:
            uuid: '7b4249e8-637a-11e5-a343-7831c1c0e63c'
            token: '8R0c6lyMG1pJGP/wzg8dHA=='
            name: 'clusterware'
            role: 'slave'
            master: 10.0.0.5
            tags:
              scheduler_roles: ':compute:'
            quorum: 3
          instance:
            users:
            - username: alces-cluster
              uid: 509
              group: alces-cluster
              gid: 509
              groups:
                - gridware
                - admins:388
              ssh_public_key: |
                ssh-rsa 1234 user
        owner: root:root
        path: /opt/clusterware/etc/config.yml
        permissions: '0640'

.. warning:: The ``uuid`` and ``token`` previously generated must be identical in order for the cluster to correctly configure the additional compute nodes.
             The ``master`` field should also contain the ``PrivateIP`` of the cluster master node, previously noted down.

* Click ``Next: Add Storage`` to continue creating the master node

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

Accessing your environment
--------------------------
Once your environment has been deployed, you can access the environment via SSH using the AWS keypair previously selected, for example:

.. code-block:: bash

    ssh -i ~/.ssh/amazon_key.pem alces@52.50.0.50

The Alces Compute appliance uses the ``alces`` user as the default administrator user.

Using your environment
----------------------

See the `environment usage <environment-usage>`_ page for more information on getting started with your Alces compute environment. 
