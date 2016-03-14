.. _manual-deploy-galaxy-environment:
Deploy a single-node Alces Galaxy research environment using AWS
================================================================

How to deploy a Galaxy research environment using AWS.

The following guide will:

-  Deploy a single instance hosting the Galaxy research compute
   environment
-  Display how to log in to the Galaxy web interface

Prerequisites
-------------

-  AWS account with appropriate privileges (preferably root account)
-  AWS keypair created
-  EU-WEST-1 region selected (IRELAND)

Deploying a Galaxy research environment
---------------------------------------

To begin - from the EC2 dashboard, select the ``Instances`` tab - then
select the ``Launch Instance`` button

1. **Step 1: Choose an Amazon Machine Image (AMI)**

-  Select the ``Community AMIs`` tab
-  Enter the latest Alces Galaxy AMI ID: ``ami-7f4ee50c``

2. **Step 2: Choose an Instance Type**

-  Select the instance type you wish to deploy

3. **Step 3: Configure Instance Details**

-  Leave default values in place unless stated below:
-  ``Number of instances``: 1
-  ``Purchasing option``: If desired, request a spot instance for better
   instance pricing at the cost of reduced availability
-  ``Network``: Choose a network to place the cluster login node in
-  ``Auto-assign Public IP``: Enable
-  ``Advanced Details``: Enter the example user data, located at
   ``https://raw.githubusercontent.com/alces-software/imageware/1.0/support/userdata-examples/aws/galaxy/login?token=AKD0RgAvf6gqPY-9EMmEj4Q-Kf7ko19eks5WpiD9wA%3D%3D``

4. **Step 4: Add Storage**

-  Increase the root volume size if required

5. **Step 5: Tag Instance**

-  Tag the instance with a name if required

6. **Step 6: Configure Security Group**

-  Select ``Create a new security group``
-  ``Security group name``: ``Galaxy-SG``
-  Add ``All traffic``, port range ``0-65535`` and set the source either
   to anywhere, or your machines IP range

7. **Step 7: Review**

-  If all details are appropriate, select the ``Review and Launch``
   button

8. **Select a Keypair**

-  Choose a keypair - this will be used to log in to the instance as the
   ``alces`` user

Once the instance has finished creating - obtain the public IP address
from the EC2 instance console and SSH to the Galaxy instance using the
``alces`` user, together with the AWS key selected on instance creation.
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

What's next?
------------

Read up on using your Galaxy environment:

-  https://wiki.galaxyproject.org/

