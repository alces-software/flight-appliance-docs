.. _configuration:

Configuration
#############

The following section details the initial configuration options available from user-data when deploying Alces compute environments in the Cloud.

HPC Environments
================
There are two types of Alces Flight Compute Appliance deployments available: 

* **Cluster master node**: Cluster scheduler host, shared NFS server, user login node
* **Cluster compute node**: Scheduler execution host

Each type requires user-data to be included upon deployment in order to correctly configure the image - examples of both master node and compute nodes are shown below: 

Cluster master node
-------------------
.. code-block:: yaml

    #cloud-config
    hostname: login1
    fqdn: login1.clusterware.alces.network
    write_files:
    - content: |
        cluster:
          uuid: '7b4249e8-637a-11e5-a343-7831c1c0e63c'
          token: '8R0c6lyMG1pJGP/wzg8dHA=='
          name: 'clusterware'
          role: 'master'
          tags:
            scheduler_roles: ':master:'
          quorum: 3
          gridware:
            depots:
            - name: benchmark
              url: https://s3-eu-west-1.amazonaws.com/packages.alces-software.com/depots/benchmark
        instance:
          users:
          - username: alces-cluster
            uid: 509
            group: alces-cluster
            gid: 509
            groups:
              - gridware
              - admins:388
            ssh_public_key: |
              ssh-rsa 1234 user
      owner: root:root
      path: /opt/clusterware/etc/config.yml
      permissions: '0640'

Cluster compute node
--------------------
.. code-block:: yaml

    #cloud-config
    hostname: node1
    fqdn: node1.clusterware.alces.network
    write_files:
    - content: |
        cluster:
          uuid: '7b4249e8-637a-11e5-a343-7831c1c0e63c'
          token: '8R0c6lyMG1pJGP/wzg8dHA=='
          name: 'clusterware'
          role: 'slave'
          tags:
            scheduler_roles: ':compute:'
          quorum: 3
        instance:
          users:
          - username: alces-cluster
            uid: 509
            group: alces-cluster
            gid: 509
            groups:
              - gridware
              - admins:388
            ssh_public_key: |
              ssh-rsa 1234 user
      owner: root:root
      path: /opt/clusterware/etc/config.yml
      permissions: '0640'

Configuration values
-------------------

Hostname
^^^^^^^^

.. code-block:: yaml

    hostname: node

This should be set to the desired hostname of the deployed system, i.e for a cluster master node: ``login1`` 

FQDN
^^^^

.. code-block:: yaml

    fqdn: node.alces.network

This should be set to ``<hostname>.network`` - allowing you to easily add your environment to your own public domain names

UUID
^^^^

.. code-block:: yaml

    uuid: '7b4249e8-637a-11e5-a343-7831c1c0e63c'

The cluster unique ID must be used across all deployed nodes in your environment. A new unique ID can be generated using the ``uuid`` tool, e.g. ``uuid -v4``

Token
^^^^^

.. code-block:: yaml

    token: '8R0c6lyMG1pJGP/wzg8dHA=='

The cluster token must be used across all deployed nodes in your environment. A new token can be generated using the ``openssl`` tool, e.g. ``openssl rand -base64 20``

Name
^^^^

.. code-block:: yaml

    name: clusterware

The name field defines the environments name, shown at user-login and in the bash-prompt, e.g. 

.. code-block:: bash

    [alces@login1(clusterware) ~]$

Role
^^^^

.. code-block:: yaml

    role: master

The ``role`` field defines whether the Alces Compute image is destined to configure itself as a cluster master node, or a worker node - only one ``master`` role should be set within the environment. 

Available options: 

* ``master``
* ``slave``

Tags
^^^^

.. code-block:: yaml

    tags:
      scheduler_roles: ':master:'

The ``tags`` section defines what type of automatic configuration should take place on each node - many tags are available for different roles, including storage manager roles, scheduler roles and galaxy roles. 

Typically, a dedicated cluster master node would use the tag: 

.. code-block:: yaml

    scheduler_roles: ':master:'

Cluster master nodes can also be configured with the ``:compute:`` tag - enabling them as a cluster execution host, allowing you to run scheduler jobs through the login node. This can be applied with: 

.. code-block:: yaml

    scheduler_roles: ':master:compute:'

Compute nodes are deployed with the ``:compute:`` tag only, e.g.

.. code-block:: yaml

    scheduler_roles: ':compute:'

Gridware
^^^^^^^^

.. code-block:: yaml

     gridware:
       depots:
        - name: benchmark
          url: https://s3-eu-west-1.amazonaws.com/packages.alces-software.com/depots/benchmark
    
The Gridware configuration section allows you to install Gridware ``depots`` - ``depots`` are pre-packaged selections of applications, libraries and compilers for a specific purpose - made available for use upon user login using the Linux ``modules`` environment. 

Examples of available Gridware depots include: 

* ``benchmark`` - popular Linux benchmarking utilities
* ``bio`` - popular bioinformatics tools
* ``chem`` - popular chemistry tools and applications

Multiple Gridware depots can be installed at the same time, for example: 

.. code-block:: yaml

     gridware:
       depots:
        - name: benchmark
          url: https://s3-eu-west-1.amazonaws.com/packages.alces-software.com/depots/benchmark
        - name: chem
          url: https://s3-eu-west-1.amazonaws.com/packages.alces-software.com/depots/chem

The Alces Application Manager appliance can also be used to pre-install and manage Gridware depots.

Galaxy Environments
==================
There are two types of Alces Galaxy Appliance deployments available:

* **Webapp/Galaxy master**: Galaxy environment master, hosts the Galaxy web interface providing an interface for researchers
* **Pulsar compute service**: The Galaxy compute service ``pulsar`` can be configured on both the Galaxy environment master node, as well as additional dedicated compute nodes

Each type requires user-data to be included upon deployment in order to correctly configure the image - examples of both master node and compute nodes are shown below: 

Galaxy master node
------------------
.. code-block:: yaml

    #cloud-config
    hostname: master1
    fqdn: master1.galaxy.alces.network
    write_files:
    - content: |
        cluster:
          uuid: '7b4249e8-637a-11e5-a343-7831c1c0e63c'
          token: '8R0c6lyMG1pJGP/wzg8dHA=='
          name: 'galaxy'
          role: 'master'
          tags:
            galaxy_roles: ':master:compute:'
      owner: root:root
      path: /opt/clusterware/etc/config.yml
      permissions: '0640'

Galaxy compute node
-------------------
.. code-block:: yaml

    #cloud-config
    hostname: node1
    fqdn: node1.galaxy.alces.network
    write_files:
    - content: |
        cluster:
          uuid: '7b4249e8-637a-11e5-a343-7831c1c0e63c'
          token: '8R0c6lyMG1pJGP/wzg8dHA=='
          name: 'galaxy'
          role: 'slave'
          tags:
            galaxy_roles: ':compute:'
      owner: root:root
      path: /opt/clusterware/etc/config.yml
      permissions: '0640'

Configuration values
-------------------

Hostname
^^^^^^^^

.. code-block:: yaml

    hostname: node

This should be set to the desired hostname of the deployed system, i.e for a Galaxy master node: ``master1`` 

FQDN
^^^^

.. code-block:: yaml

    fqdn: node.alces.network

This should be set to ``<hostname>.network`` - allowing you to easily add your environment to your own public domain names

Galaxy research compute environments are also automatically added to the Alces public network `cloud.compute.estate`, with SSL certificates automatically generated for your Galaxy environment.

UUID
^^^^

.. code-block:: yaml

    uuid: '7b4249e8-637a-11e5-a343-7831c1c0e63c'

The cluster unique ID must be used across all deployed nodes in your environment. A new unique ID can be generated using the ``uuid`` tool, e.g. ``uuid -v4``

Token
^^^^^

.. code-block:: yaml

    token: '8R0c6lyMG1pJGP/wzg8dHA=='

The cluster token must be used across all deployed nodes in your environment. A new token can be generated using the ``openssl`` tool, e.g. ``openssl rand -base64 20``

Name
^^^^

.. code-block:: yaml

    name: galaxy

The name field defines the environments name, shown at user-login and in the bash-prompt, e.g. 

.. code-block:: bash

    [alces@master1(galaxy) ~]$

Role
^^^^

.. code-block:: yaml

    role: master

The ``role`` field defines whether the Alces Galaxy appliance is destined to configure itself as a Galaxy master node, or a worker node - only one ``master`` role should be set within the environment. 

Available options: 

* ``master``
* ``slave``

Tags
^^^^

.. code-block:: yaml

    tags:
      galaxy_roles: ':master:'

The ``tags`` section defines what type of automatic configuration should take place on each node - many tags are available for different roles, including storage manager roles, scheduler roles and galaxy roles. 

Typically, a Galaxy master node would use the tag: 

.. code-block:: yaml

    galaxy_roles: ':master:'

Galaxy master nodes can also be configured with the ``:compute:`` tag - enabling them as a cluster execution host, allowing you to run Galaxy compute jobs all through a single instance. This can be applied with: 

.. code-block:: yaml

    galaxy_roles: ':master:compute:'

Compute nodes are deployed with the ``:compute:`` tag only, e.g.

.. code-block:: yaml

    galaxy_roles: ':compute:'

