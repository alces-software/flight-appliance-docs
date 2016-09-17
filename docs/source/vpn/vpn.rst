.. _vpn:

Cluster VPN
===========

Your Flight Compute cluster is configured with a Virtual Private Network (VPN) - allowing you to connect your workstation to the cluster network. The cluster VPN provides your machine with an IP address that is part of the environment's network, allowing direct communication with compute nodes for high-performance graphical application access and data transfer. All communications exchanged over a VPN are automatically encrypted using a certificate unique to your cluster.

The Alces Flight Compute VPN uses `OpenVPN <https://openvpn.net/>`_ - so you may wish to use a client that is capable of connecting using OpenVPN configurations. 

.. note:: For best security practice, each Alces Flight Compute cluster you deploy is configured with a unique VPN configuration. If you run multiple clusters, users need to create a profile on your client machine for each Alces Flight Compute environment you want to connect to.

Available VPN clients
---------------------

There are many VPN clients available which support OpenVPN configurations - some popular clients for each platform include; 

MacOS
`````

* `TunnelBlick <https://tunnelblick.net/>`_

Linux
`````

* NetworkManager includes support for OpenVPN configurations

Windows
```````

* `OpenVPN <https://openvpn.net/index.php/open-source/downloads.html>`_


Obtaining VPN configuration
---------------------------

Your Alces Flight Compute cluster automatically packages the VPN configuration for a number of VPN clients including OpenVPN and TunnelBlick. To view information including download paths for configuration packs, use the ``alces about vpn`` command as per the following example:

.. code:: bash

  [alces@login1(sge) ~]$ alces about vpn
       VPN configuration path: /opt/clusterware/etc/openvpn/client/clusterware
         Tarred configuration: /opt/clusterware/etc/openvpn/client/clusterware-openvpn.tgz
         Zipped configuration: /opt/clusterware/etc/openvpn/client/clusterware-openvpn.zip
    Tunnelblick configuration: /opt/clusterware/etc/openvpn/client/clusterware-tunnelblick.zip
            Download web page: https://sge-0a8de07e.cloud.alces.network/vpn/
     Download access username: vpn
     Download access password: **********

You can then obtain the VPN configuration packs either through command-line tools such as `scp`, or through the Alces Flight web page as shown in the ``alces about vpn`` output and connect using the steps provided by your VPN client instructions. 

.. note:: The VPN configuration download web page is only available to users running an Alces Flight Compute *Enterprise* edition cluster. The VPN configuration page available to Alces Flight Compute *Enterprise* edition clusters also includes handy instructions on how to connect using various clients.
