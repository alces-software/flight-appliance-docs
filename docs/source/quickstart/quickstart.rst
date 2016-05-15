.. _quickstart:


Getting started as fast as possible
===================================

This documentation is already designed to provide a quick-start guide, but if you've used clusters before then you might just need a super-quick guide to getting started with your new cluster. Here are just the basics, along with things you should probably look at in more detail when you have time. Useful commands are included with each step - please refer to the full documentation for future details. 

1. Launch your personal Alces Flight Compute cluster on AWS or a compatible Open-stack private cloud. You will be asked various questions about how you want your cluster to look - most options are explained, and include sensible defaults. 

2. Login to your cluster using your chosen username, and the SSH key registered with your provider. Your cloud service should report the access IP address to login to once your cluster is launched. If you enabled cluster auto-scaling, compute nodes are automatically added/removed based on the status of your job-scheduler queue. Use ``alces configure autoscaling disable`` to turn off scaling if you'll be running jobs manually. 

3. Your user has full ``sudo`` access; ``yum`` is configured to install packages. Use the ``nodeattr -n nodes`` command to see your list of compute node hostnames, and ``pdsh -g nodes <command>`` to run commands across all nodes in your cluster.

4. Your cluster has an NFS shared filesystem mounted across all nodes, which your home-directory is part of. Copy data to and from the cluster using SCP/SFTP. The ``alces storage`` commands provide access to object storage services including S3 and Dropbox.

5. Use the ``alces gridware`` command to view and install software. The ``main`` repository lists stable packages, with a larger list of applications available as part of the ``volatile`` repository. 

6. Your cluster comes with an installation of the SGE job-scheduler; use the ``alces template`` command for a list of template job-scripts to get you started. 

7. Terminate the cluster stack via your cloud provider when you've finished working. Remember to copy any data you want to keep off the cluster to secure storage before terminating it. 

