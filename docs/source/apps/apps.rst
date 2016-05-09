.. _apps:


Software Applications
#####################

Flight Compute operating system
-------------------------------

The current revision of Alces Flight Compute builds personal, ephemeral clusters based on a 64-bit CentOS 7.2 Linux distribution. The same operating system is installed on all login and compute nodes across the cluster. The Linux distribution includes a range of software tools and utilities, packaged by the CentOS project as RPM files. These packages are available for users to install as required on login and compute nodes using the ``yum`` command. You can also install other RPM packages on your Flight Compute cluster by copying them and installing them using the RPM command. 

Shared cluster applications
---------------------------

While RPM packages are useful for system packages, they are not designed to manage software applications that have complex dependencies, span multiple nodes or coexist with incompatible applications. Alces Flight Compute clusters include a mechanism to install and manage software applications on your cluster called **Alces Gridware**. Gridware packages are centrally installed on your shared cluster filesystem, making them available to all cluster login and compute nodes. New applications are installed with supporting **environment module** files, and can be dynamically optimised at installation time for specific environments. 


modules environment management; usage and commands, dependencies
----------------------------------------------------------------

importance of "module display" to show licenses
-----------------------------------------------

installing new applications using alces gridware command
--------------------------------------------------------

enabling volatile repos and manual dependency resolution
--------------------------------------------------------

# edit /opt/gridware/etc/gridware.yml and un-comment volatile line
# alces gridware update

installing from depots
----------------------

installing commercial apps and license management
-------------------------------------------------

