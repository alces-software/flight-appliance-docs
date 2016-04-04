.. _alces-flight-compute:

Alces Flight Compute
####################

The Alces Flight Compute appliance provides two primary roles -- a cluster master node; hosting the primary cluster services, as well as compute node roles. The Alces Compute image is configured via user-data, which informs the Alces Compute image which role to take on.

The Compute image has been designed to simplify the deployment of HPC compute environments in the Cloud, significantly reducing the time-to-research.

The Alces Compute image providers researchers with powerful tools to get started with their research in the Cloud, including:

* **Applications on-demand**

The Alces HPC compute image contains tools to provide on-demand applications, with over 750 different applications libraries and compilers available through the *modules* environment together with Alces Gridware and optionally the `Alces Flight Application Manager Appliance`_

* **Dynamic self-configuring HPC scheduler**

The Alces HPC compute image self-configures, automatically creating a workload-ready HPC compute environment. The Alces HPC compute image is capable of deploying both scheduler master nodes, as well as worker nodes - with scheduler configurations included.

* **Familiar environment**

The Alces HPC compute image is deployed in a traditional HPC compute environment architecture, with cluster master nodes running the job scheduler - as well as dedicated compute nodes, shared directories and more.
