.. _alces-flight-galaxy:

Alces Flight Galaxy Appliance
#############################

The Alces Flight Galaxy appliance provides users with a fully-configured Galaxy research compute environment, with two roles available -- including cluster master (web app) and cluster slave (Pulsar compute) roles. The Alces Galaxy appliance is configured via user-data, which informs the Alces Galaxy appliance which 'role' to take on.

The Alces Flight Galaxy appliance has been designed to provide researchers with a simple method of deploying their own Galaxy compute environments in Cloud environments.

* **Dynamic environment**
The Alces Flight Galaxy appliance can be deployed in a number of ways, and increase in size dynamically over time. Researchers may choose to start their environment with a single compute instance, hosting both the webapp service and Pulsar compute service, then in time add additional dedicated compute hosts to the environment. The Alces Galaxy appliance automatically registers new hosts in the environment, with no manual configuration necessary.

* **Science ready**
The Alces Flight Galaxy appliance self-configures, enabling you to focus on research rather than configuration and setup - additionally deployed compute hosts are also automatically configured and added to the environment, allowing you to scale your environment when required.

* **Secure environment**
The Alces Flight Galaxy appliance automatically generates SSL certificates, and joins the Alces public network ``cloud.compute.estate`` - allowing you to securely work in your deployed environment.
