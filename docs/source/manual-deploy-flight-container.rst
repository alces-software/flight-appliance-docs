.. _manual-deploy-flight-container-appliance:

The following guide details how to manually deploy a single Alces Flight Container Appliance using an Alces OpenStack environment. 

Prerequisites
-------------
-  Alces OpenStack user account
-  Correctly connected to the cluster DMZ network
-  OpenStack keypair added

Deploying via Horizon web interface
-----------------------------------
1.  Log in to the OpenStack Horizon interface with your site credentials
2.  Navigate to the ``Project -> Compute -> Instances`` page
3.  Select the ``Launch Instance`` button
4.  In the ``Details`` tab, enter the following with your own customisations where noted. For any option not provided, leave default:
  -  **Instance name**: ``docker1`` - or your own name
  - **Flavor**: ``m1.small`` - or set your own. This defines the number of CPU cores, memory and disk available to the instance
  -  **Instance Count**: ``1``
  -  **Instance Boot Source**: ``Boot from Image``
  -  **Image Name**: Select the Alces Flight Container Appliance
5.  In the ``Access & Security`` tab, enter the following: 
  -  **Key Pair**: Select your OpenStack keypair
  -  **Security Groups**: ``default``
6.  From the ``Networking`` tab, enter the following:
  -  **Selected Networks**: Select the ``internal`` network from the list of ``Available Networks``
7.  From the ``Post-creation`` tab, enter the following: 
  - Enter the following sample user-data as ``Direct Input``, with your own hostname: 

.. code:: yaml

    #cloud-config
    hostname: docker1
    fqdn: docker1.docker.alces.network
    write_files:
    - content: |
        cluster:
          uuid: '7b4249e8-637a-11e5-a343-7831c1c0e63c'
          token: '8R0c6lyMG1pJGP/wzg8dHA=='
          name: 'docker'
          role: 'master'
      owner: root:root
      path: /opt/clusterware/etc/config.yml
      permissions: '0640'

8.  Select the ``Launch`` button to begin deploying your Alces Flight Container Appliance instance
9.  Assign a Floating IP to your instance

Access your environment
-----------------------

1.  From the ``Project -> Compute -> Instances`` tab, copy the floating IP address of your instance - for example ``10.77.0.50``
2.  Using your favourite SSH client, connect to the Alces Flight Container Appliance instance as the ``alces`` administrator user together with your OpenStack keypair, e.g.: 
  -  ``ssh -i ~/.ssh/openstack-key.pem alces@10.77.0.50``

Using your environment
----------------------

1. :ref:`Basic usage of an Alces Flight Container Appliance <basic-usage-flight-container-appliance>`

Destroying your environment
---------------------------

1.  From the ``Project -> Compute -> Instances`` page, locate your instance
2.  Using the dropdown menu, select ``Terminate Instance``
3.  Your instance is now destroyed and cleaned up.
