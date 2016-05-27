 .. _template_launch:

Launching Alces Flight on AWS using a CloudFormation template
#############################################################

Using AWS Marketplace is a convenient method to launch your Alces Flight cluster, using a pre-defined template that collect common configuration options for an HPC compute cluster. For advanced users, it is also possible to launch individual instances from the Alces Flight AMI - for more details, see - :ref:`manual_launch`.

Users can also create their own CloudFormation templates to launch different cluster configurations. While a base knowledge of CloudFormation is required, this method is often preferable to configuring individual instances as it allows clusters to be repeatably launched once a customized template has been created.

Alces provides a number of example templates that are intended to assist users in generating their own templates. The templates below use the Flight AMI from AWS Marketplace to build your cluster - users are encouraged to review these templates with an aim to launching their own, customised environments.


Example AWS CloudFormation templates
#####################################

8-node, 16-node and 32-node fixed-size clusters
-----------------------------------------------

https://s3.amazonaws.com/alces-flight-templates/2016.2r3/8-node.json
https://s3.amazonaws.com/alces-flight-templates/2016.2r3/16-node.json
https://s3.amazonaws.com/alces-flight-templates/2016.2r3/32-node.json

This template is designed to create an 8-node cluster, and provides a choice of compute and login node instance types. The template creates:

 - A VPC, subnet and gateway for the cluster
 - An on-demand login node with EBS (Magnetic) storage
 - 8, 16 or 32 compute nodes of on-demand or spot type in an autoscaling group
 
 
Variable sized cluster
----------------------

https://s3.amazonaws.com/alces-flight-templates/2016.2r3/x-node.json

This template is designed to create a cluster of between 1 and 32 nodes, and provides a choice of compute and login node instance types. The template creates:

 - A VPC, subnet and gateway for the cluster
 - An on-demand login node with EBS (Magnetic) storage
 - A choice of compute nodes of on-demand or spot type in an autoscaling group
 
 
 
Demonstration cluster
---------------------

https://s3.amazonaws.com/alces-flight-templates/2016.2r3/demo.json

This template is designed to create a cluster of between 1 and 32 nodes, with higher performance SSD-backed EBS storage (sg2) and all software repositories enabled for demonstration purposes. The template creates:

 - A VPC, subnet and gateway for the cluster
 - An on-demand login node with SSD-backed EBS (sg2) storage
 - A choice of compute nodes of on-demand type
 - Both the main and volatile software repositories enabled
 
  
 
 


