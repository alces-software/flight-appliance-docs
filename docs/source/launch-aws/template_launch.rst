 .. _template_launch:

Launching Alces Flight on AWS using a CloudFormation template
#############################################################

Using AWS Marketplace is a convenient method to launch your Alces Flight cluster, using a pre-defined template that collect common configuration options for an HPC compute cluster. For advanced users, it is also possible to launch individual instances from the Alces Flight AMI - for more details, see - :ref:`manual_launch`.

Users can also create their own CloudFormation (CFN) templates to launch different cluster configurations. While a base knowledge of CloudFormation is required, this method is often preferable to configuring individual instances as it allows clusters to be repeatably launched once a customized template has been created.

Alces provides a number of example templates that are intended to assist users in generating their own templates. The templates below use the Flight AMI from AWS Marketplace to build your cluster - users are encouraged to review these templates with an aim to launching their own, customised environments.


Example AWS CloudFormation template
#####################################

This AWS CloudFormation template is designed to create a Flight Solo cluster in the AWS region of your choice, including:

 - A VPC, subnet and gateway for the cluster
 - An on-demand login node with EBS (Magnetic) storage
 - A configurable number and type of compute node instances
 - A choice of compute nodes of on-demand or spot type in an autoscaling group
 
 https://s3-eu-west-1.amazonaws.com/flight-aws-marketplace/2017.1/FlightSolo_2017.1_community_x-node.json
 
.. note:: The CFN templates referenced utilise the Amazon Machine Images (AMI) available on AWS Marketplace. Before using the templates, users must subscribe to the community edition of Alces Flight on AWS Marketplace in order to get permission to use these AMIs. Follow the URL below to visit the product page to subscribe:

 http://tiny.cc/alcesflightmp
