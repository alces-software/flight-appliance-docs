.. _launching_on_os:

Launching on OpenStack
######################

Alces Flight Compute can be launched on your local OpenStack private cloud platform to give you access to your own, private HPC cluster using your on-premise infrastructure.

Prerequisites
=============

 * Alces OpenStack user account
 * Correctly connected to the cluster DMZ network
 * Appropriate resource quota to create the number of compute nodes you choose
 * OpenStack keypair added

How to deploy
=============

 1.  Log in to the OpenStack Horizon interface with your site credentials
 2.  Navigate to the Project -> Orchestration -> Stacks page
 3.  Select the Launch Stack button
 4.  **Template Source**: Select URL and enter the following template URL:
   * https://raw.githubusercontent.com/alces-software/flight-appliance-support/master/openstack-heat/templates/flight-compute.yaml
 5.  **Environment Source**: *Not required*
 6.  Click the Next button
 7.  **Stack Name**: Enter a stack name, this defines the cluster name
 8.  **Creation Timeout (minutes)**: Leave default 60
 9.  **Rollback On Failure**: Enabled
 10.  **Password for user**: Enter your OpenStack user password
 11.  **Cluster admin key**: Select your OpenStack keypair to assign to the administrator user account
 12.  **HPC Image**: Select the Alces Compute image from the list of available images
 13.  **Instance Type**: Select the instance type to deploy, this defines the number of cores and memory available to each instance
 14.  **Number of compute nodes**: Enter the number of dedicate compute nodes you wish to deploy
 15.  Click the Launch button.
 16.  Once the stack is in Status: Create In Progress - enter the stack and navigate to the Overview tab
 17.  Once the stack has finished creating, the Overview tab will provide you with the cluster public IP address, used to log in with the administrator user alces

Accessing your environment
==========================

1.  From the ``Overview`` tab of your stack, make a note of the cluster master node public IP address displayed, e.g. ``10.77.0.100``
2.  SSH to the public IP address as the administrator user you previously selected, e.g. ``alces`` - together with your previously selected OpenStack keypair, e.g. ``ssh -i ~/.ssh/openstack_key.pem alces@10.77.0.100``

Terminating your environment
============================

1.  From the ``Stacks`` page, select your previously created Alces Flight Compute stack - then select ``Delete Stacks``
