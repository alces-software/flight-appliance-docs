.. _deployment:

Deployment
##########

The following section details how to deploy different types of Alces research compute environments using both OpenStack and AWS platforms.

HPC environments
----------------

AWS
^^^

* :ref:`Deploy a 3 node scheduler environment using CloudFormation<cfn-deploy-3-node-sge-cluster>`
* :ref:`Deploy an autoscaling spot compute cluster using CloudFormation<cfn-deploy-sge-spot-cluster>`
* :ref:`Manually deploy a HPC scheduler environment using EC2 console<manual-deploy-sge-cluster>`

OpenStack
^^^^^^^^^

* :ref:`Deploy an autoscaling scheduler environment using OpenStack Heat<heat-deploy-sge-cluster>`

Galaxy environments
-------------------

AWS
^^^

* :ref:`Deploy an all-in-one Galaxy compute environment on a single node using CloudFormation<cfn-deploy-galaxy-environment>`
* :ref:`Deploy a Galaxy environment with dedicated compute hosts using CloudFormation<cfn-deploy-multi-node-galaxy-environment>`
* :ref:`Manually deploy a Galaxy compute environment using EC2 console<manual-deploy-galaxy-environment>`

OpenStack
^^^^^^^^^

* :ref:`Deploy a Galaxy environment with dedicated compute hosts using OpenStack Heat<heat-deploy-galaxy-cluster>`
