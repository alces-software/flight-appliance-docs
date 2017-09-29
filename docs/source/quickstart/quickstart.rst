.. _quickstart:

.. warning:: The documentation here may not directly apply to your version of Flight, locate your version of Flight :ref:`here <index>`



Getting started as quickly as possible
======================================

This documentation is designed to provide a quick-start guide to your personal Flight Compute cluster, but if you've used clusters before then you might just need a *super-quick* guide to help you get started. Here are just the basics, along with things you should probably look at in more detail when you have time. Useful commands are included with each step - please refer to the full documentation for further details. 

 1. Launch your personal Alces Flight Compute cluster on AWS or a compatible OpenStack private cloud. To use the AWS platform, use the **Personal HPC compute cluster** launch option from the `Alces Flight Marketplace product page <http://tiny.cc/alcesflight>`_ **Manual Launch** tab to start a personal cluster in a new VPC. You will be asked various questions about how you want your cluster to look - most options are explained, with sensible defaults shown.

 2. Login to your cluster via SSH using your chosen username, and the SSH key registered with your cloud provider. Your cloud service should report the access IP address to log into once your cluster is launched. If you enabled cluster auto-scaling, compute nodes are automatically added/removed based on the status of your job-scheduler queue. Use ``alces configure autoscaling disable`` to turn off scaling if you'll be running jobs manually. 

 3. Start a graphical desktop session with the command ``alces session start gnome``. Connect using a VNC client, with the one-time password shown when you started the session. Multiple users can connect to the same session for collaborative projects. Use the ``xrandr`` command to change the screen resolution in a running desktop session. 

 4. Your user has full ``sudo`` access, and ``yum`` is configured to install Linux packages. Load the PDSH module with the command ``module load services/pdsh``, then use the ``nodeattr -n nodes`` command to see your list of compute node hostnames, and ``pdsh -g nodes <command>`` to run commands across all nodes in your cluster. The Flight Compute Solo product is designed to support a single user, although multiple users can login using the same account. 

 5. Your cluster has an NFS shared filesystem mounted across all nodes, which your home-directory is part of. Copy data to and from the cluster using SCP/SFTP. The ``alces storage`` commands provide access to object storage services including S3, Swift and Dropbox. Clusters can also be configured to use other storage types including parallel filesystems and shared filers, depending on your cloud platform.

 6. Use the ``alces gridware`` command to view and install software. The ``main`` repository lists stable packages, with a larger list of applications available as part of the ``volatile`` repository. 

 7. Your cluster comes with an installation of a batch job scheduler; the default is SLURM, and a range of other schedulers are also available. Use the ``alces template`` command for a list of template job-scripts to get you started. 

 8. Terminate the cluster stack via your cloud provider when you've finished working. Remember to copy any data you want to keep off the cluster to secure storage before terminating it. 

