.. _overview:

Overview
########

The Alces Flight appliances are designed to simplify deployment of various compute environments using public and private Cloud providers including AWS and OpenStack. The Flight appliances lower the barrier to entry of Cloud computing, as well as reducing overall cost by serving a pre-configured environment to you in just minutes.

Available Appliances
--------------------
The currently available Alces Flight appliances include:

* `Alces Flight Compute Appliance`_
* `Alces Flight Storage Access Appliance`_
* `Alces Flight Application Manager Appliance`_
* `Alces Flight Galaxy Appliance`_

Alces Flight Compute Appliance
******************************
The Alces Flight Compute appliance provides two primary roles -- a cluster master node; hosting the primary cluster services, as well as compute node roles. The Alces Compute image is configured via user-data, which informs the Alces Compute image which role to take on.

The Compute image has been designed to simplify the deployment of HPC compute environments in the Cloud, significantly reducing the time-to-research.

The Alces Compute image providers researchers with powerful tools to get started with their research in the Cloud, including:

* **Applications on-demand**

The Alces HPC compute image contains tools to provide on-demand applications, with over 750 different applications libraries and compilers available through the *modules* environment together with Alces Gridware and optionally the `Alces Flight Application Manager Appliance`_

* **Dynamic self-configuring HPC scheduler**

The Alces HPC compute image self-configures, automatically creating a workload-ready HPC compute environment. The Alces HPC compute image is capable of deploying both scheduler master nodes, as well as worker nodes - with scheduler configurations included.

* **Familiar environment**

The Alces HPC compute image is deployed in a traditional HPC compute environment architecture, with cluster master nodes running the job scheduler - as well as dedicated compute nodes, shared directories and more.

Alces Flight Storage Access Appliance
*************************************
The Alces Flight Storage Access appliance provides users with a means to manage uploading, downloading and manipulating files in their cluster POSIX storage and object stores via their web browser.

The Alces Flight Storage Access appliance allows researchers to seamlessly interact with both POSIX filesystems and object storage in a consistent manner. Many different storage configurations can be added to the Storage Manager appliance, allowing researchers to couple storage from multiple Cloud environments together in one simple but powerful interface.

Alces Flight Application Manager Appliance
******************************************
The Alces Flight Application Manager appliance allows users to easily manage their applications, libraries and compilers from a single location. The Application Manager can be deployed one or many times to serve one or many compute environments.

The Application Manager appliance includes several key features, including:

*  **Applications on-demand**

The Application Manager appliance includes the Alces Gridware utility, simplifying installation and management of Linux applications, libraries and compilers. With the Application Manager appliance deployed, applications can be installed from any of your available compute environments, and automatically installed onto the Application Manager appliance - making them available to other compute environments instantly.

* **Share applications among all of your environments**

The Application Manager can be deployed once, to share your applications among many compute environments - reducing overall configuration time of each new compute environment you deploy.

Alces Flight Galaxy Appliance
*****************************
The Alces Flight Galaxy appliance provides users with a fully-configured Galaxy research compute environment, with two roles available -- including cluster master (web app) and cluster slave (Pulsar compute) roles. The Alces Galaxy appliance is configured via user-data, which informs the Alces Galaxy appliance which 'role' to take on.

The Alces Flight Galaxy appliance has been designed to provide researchers with a simple method of deploying their own Galaxy compute environments in Cloud environments.

* **Dynamic environment**
The Alces Flight Galaxy appliance can be deployed in a number of ways, and increase in size dynamically over time. Researchers may choose to start their environment with a single compute instance, hosting both the webapp service and Pulsar compute service, then in time add additional dedicated compute hosts to the environment. The Alces Galaxy appliance automatically registers new hosts in the environment, with no manual configuration necessary.

* **Science ready**
The Alces Flight Galaxy appliance self-configures, enabling you to focus on research rather than configuration and setup - additionally deployed compute hosts are also automatically configured and added to the environment, allowing you to scale your environment when required.

* **Secure environment**
The Alces Flight Galaxy appliance automatically generates SSL certificates, and joins the Alces public network ``cloud.compute.estate`` - allowing you to securely work in your deployed environment.
