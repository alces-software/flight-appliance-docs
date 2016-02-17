.. _cfn-deploy-galaxy-environment:
Deploy a single node Alces Galaxy research environment using AWS CloudFormation
===============================================================================

The following steps will provide you with a single-node Alces Galaxy research compute environment, fully-configured ready for research. The cluster is comprised of a single compute instance, which hosts both the Galaxy webapp as well as providing the compute service. 

**Prerequisites**

-  EU-IRELAND region selected
-  AWS key pair previously uploaded
-  Appropriate IAM permissions (primary user preferable)

Deploying via AWS web interface
-------------------------------

1.  Navigate to the CloudFormation console
2.  Click ``Create Stack``
3.  Choose the ``Specify an S3 URL`` option - enter the following URL: ``https://s3-eu-west-1.amazonaws.com/flight-appliance-support/galaxy/single-node.json``
4.  Click ``Next``
5.  Enter a ``stack name``, for example ``GalaxyCluster1``
6.  Enter a ``Cluster Name`` - this defines your environments name
7.  Select the ``InstanceFlavour`` to use, this defines the number of cores, memory and disk available to the instance
8.  Select the ``KeyPair`` to use - this is your AWS keypair previously uploaded, and enables administrator access to the Galaxy instance
9.  For increased security, enter your workstations network address CIDR to restrict access to your environment
10.  Click ``Next`` to go to the ``Tags`` page.
11.  Click the ``Next`` button again to begin creating your stack. Once complete - the ``Outputs`` tab will display the Galaxy master node public IP address. 

Access your environment
-----------------------

1.  Navigate to the CloudFormation console
2.  Select the created CloudFormation stack
3.  Select the ``Output`` tab to locate the master node public IP address
4.  SSH to the public IP address as the ``alces`` administrator account - together with your previously select AWS key, e.g. ``ssh -i ~/.ssh/amazon_key.pem alces@50.0.0.50``

The Galaxy configuration stage can take a few minutes to complete
depending on the instance type chosen - view the progress with:

.. code:: bash

    tail -f /var/log/clusterware/instance.log

Once the configuration is complete - gain access information for the
Galaxy environment:

.. code:: bash

    [alces@login1(galaxy) ~]$  alces about environment
          Clusterware release: 2016.01
             Public host name: galaxy-ea944b00.cloud.compute.estate
             Clusterware name: galaxy-ea944b00
      Galaxy - admin username: admin@galaxy-ea944b00.cloud.compute.estate
      Galaxy - admin password: wzcx8dc1Ah
        Galaxy - access point: https://galaxy-ea944b00.cloud.compute.estate:64443/
           Platform host name: ec2-52-49-2-206.eu-west-1.compute.amazonaws.com
            Public IP address: 52.49.2.206

Using your environment
----------------------

Read up on using your Galaxy environment:

-  https://wiki.galaxyproject.org/

Destroying your environment
---------------------------

1. Navigate to the CloudFormation console
2. Select the created CloudFormation stack
3. Choose ``Delete Stack``
4. All previously created resources are destroyed and cleaned up
