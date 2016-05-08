.. _launching_on_aws:

Launching on AWS
================

Alces Flight Compute can be launched on the Amazon Web Services (AWS) public cloud platform to give you instant access to your own, private HPC cluster from anywhere in the world. You can choose what resources your cluster will start with (e.g. number of nodes, amount of memory, etc.), and for how long the cluster will run. 


Prerequisites
=============

There are some things that you need to get ready before you can launch your own cluster on AWS. They are:

 - `**Client prerequisites** .. _whatisit:`
 - **Get yourself an AWS account**; this might be your personal account, or you may have a sub-account as part of your institution or company

Your AWS account must have appropriate permissions to do the following:
 - Launch Cloud Formation templates
 - Create a VPC (virtual private cloud)
 - Create subnets and allocate IP addresses
 - Create an IAM permission
 
More details on `AWS Identity and Access Management (IAM) are available here <https://aws.amazon.com/iam/>`_.


Creating your Cluster
=====================

How much will it cost?
----------------------

The Alces Flight software appliance itself it free; however, you're likely to incur costs when running a cluster on AWS resources. Charges typically fall into the following categories:

 - `EC2 <https://aws.amazon.com/ec2/>`_ charges for running instances (your login and compute nodes) 
 - `EBS <https://aws.amazon.com/ebs/>`_ charges for shared cluster filesystem capacity
 - `S3 <https://aws.amazon.com/s3/>`_ charges for storing data as objects
 - `Data-egress charges <https://aws.amazon.com/blogs/publicsector/aws-offers-data-egress-discount-to-researchers/>`_ for network traffic out of AWS
 - `Miscellaneous other charges <https://aws.amazon.com/pricing/services/>`_(e.g. IP address allocation, DNS entry updates, etc.)

Most charges are made per unit (e.g. per compute node instance, or per GB of storage space) and per hour, often with price breaks for using more of a particular resource at once. A full breakdown of pricing is beyond the scope of this document, but there are several tools designed to help you estimate the expected charges:

 - `AWS Simple Monthly Calculator <https://calculator.s3.amazonaws.com/index.html>`_
 - `AWS TCO Calculator <https://awstcocalculator.com/>`_


Finding Alces Flight Compute on AWS
-----------------------------------

Sign-in to your AWS account, and navigate to the `AWS Marketplace <https://aws.amazon.com/marketplace>`_. Search for **Alces Flight** in the search box provided to find the Flight Compute product. Click on the *Continue* button to view details on how to launch. 

As well as an Amazon Machine Image (AMI), Flight Compute subscribers are provided with a Cloud-Formation template (CFT) that can be used to launch your own cluster rapidly after answering a few setup questions. Advanced users can also use the AMI directly with their own CFTs to provide more customised environments for specialised requirements. This documentation is designed to assist new users when launching with the CFT provided on the AWS Marketplace page. 


How to answer Cloud-Formation questions
---------------------------------------

When you choose to start a Flight Compute cluster from AWS Marketplace, you will be prompted to answer a number of questions about what you want the environment to look like. Flight will automatically launch your desired configuration based on the answers you give. The questions you'll be asked are the following:

 - **Stack name**; this is the name that you want to call your cluster. It's fine to enter "cluster" here if this is your first time, but entering something descriptive will help you keep track of multiple clusters if you launch more. Naming your cluster after colours (red, blue, orange), your favourite singer (clapton, toriamos, bieber) or Greek legends (apollo, thor, aphrodite) keep things more interesting. Avoid using spaces and punctuation, and names longer than 16 characters.


on-demand vs SPOT and autoscaling
---------------------------------

Accessing your cluster
----------------------

What happens when you terminate the cluster
-------------------------------------------


