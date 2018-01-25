.. _graphicaldesktop:

Graphical desktop access to your login node
###########################################

Your Alces Flight Compute login node can run graphical desktop sessions to support users who want to run interactive applications across the cluster. The system can support a number of different sessions simultaneously, and allow multiple remote participants to connect to the same session to support training and collaborative activities. 


Launching a desktop session
===========================

All Flight Compute clusters come pre-installed with a Gnome desktop environment which users can start from the command-line as required. Users can launch a new session by using the ``alces session start gnome`` command. After launching the desktop, a message will be printed with connection details to access the new session:

.. code:: bash

    VNC server started:
        Identity: b7d8e878-19b7-11e6-96cc-0a949a3e07d9
            Type: gnome
            Host: 52.49.121.188
            Port: 5901
         Display: 1
        Password: JqQJWkA5
       Websocket: 41361
    
    Depending on your client, you can connect to the session using:
    
      vnc://alces:JqQJWkA5@52.49.121.188:5901
      52.49.121.188:5901
      52.49.121.188:1
    
    If prompted, you should supply the following password: JqQJWkA5


Users need a VNC client to connect to the graphical desktop session - for a list of tested clients, see :ref:`whatisit`. 

Users with Mac clients can use the URL provided in the command output to connect to the session; e.g. from the above example, simply enter ``vnc://alces:JqQJWkA5@52.49.121.188:5901`` into the Safari address bar. Linux and Windows users should enter the IP address and port number shown into their VNC client in the format ``IP:port``. For example - for the output above, Linux and Windows client users would enter ``52.49.121.188:5901`` into their VNC client:

.. image:: vncclient.jpg
    :alt: VNC client configuration
    
A one-time randomized password is automatically generated automatically by Flight Compute when a new session is started. Linux and Windows users may be prompted to enter this password when they connect to the desktop session. 

.. note:: If the desktop has been started on a compute node then you will need to :ref:`connect to the VPN <vpn>` and access the VNC session using the internal IP address (Login Node: `Master node IP address` from ``alces about identity``. Compute Node: `Primary IP address` from ``alces about node`` when logged into the node)

Once connected to the graphical desktop, users can use the environment as they would a local Linux machine:

.. image:: vncdesktop.jpg
    :alt: VNC desktop session
    

Connecting multiple users to the same session
---------------------------------------------

New graphical sessions are created in multi-user mode by default, allowing users from different locations to connect to the same session for training or collaborative projects. All users connect using the same connection details (e.g. IP-address and port number), and use the same one-time session password. 

Please note that users are only permitted to connect to your Flight Compute cluster login node if their IP address is within the set of networks allowed in the ``CIDR`` setting made at launch time. If you have issues with secondary users connecting to a graphical desktop session, please try entering ``0.0.0.0/0`` as your CIDR at cluster launch time to allow access from all users. 


Resizing the desktop to fit your screen
---------------------------------------

Specifying a size with the alces session tool
*********************************************

When launching a graphical desktop session using the ``alces session`` utility, a session resolution can be specified using the ``--geometry <size>`` option. For example, to launch a ``gnome`` desktop session with a resolution of 1920x1080 pixels, use the command:

.. code:: bash
   
    alces session start --geometry 1920x1080 gnome

By default, your graphical desktop session will launch with a compatibility resolution of 1024x768. Users can resize the desktop to fit their screens using the Linux ``xrandr`` command, run from within the graphical desktop session. 

To view the available screen resolutions, start a terminal session on your graphical desktop by navigating to the ``Applications`` menu in the top left-hand corner of the screen, then selecting the ``Terminal`` under the ``System tools`` menu.

.. image:: startingterminal.jpg
    :alt: Starting a terminal session

The ``xrandr`` command will display a list of available resolutions supported by your desktop:

.. code:: bash

    [alces@login1(scooby) ~]$ xrandr
    Screen 0: minimum 32 x 32, current 1024 x 768, maximum 32768 x 32768
    VNC-0 connected primary 1024x768+0+0 0mm x 0mm
       1920x1200     60.00
       1920x1080     60.00
       1600x1200     60.00
       1680x1050     60.00
       1400x1050     60.00
       1360x768      60.00
       1280x1024     60.00
       1280x960      60.00
       1280x800      60.00
       1280x720      60.00
       1024x768      60.00*
       800x600       60.00
       640x480       60.00

To set a new resolution, run the ``xrandr`` command again with the ``-s <resolution>`` argument; 

  - e.g. to change to 1280x1024, enter the command ``xrandr -s 1280x1024``
  
Your graphical desktop session will automatically resize to the new resolution requested. Use your local VNC client application to adjust the compression ratio, colour depth and frame-rate sessions in order to achieve the best user-experience for the desktop session.


Using alces session commands to enable other types of session
-------------------------------------------------------------

Your Alces Flight Compute cluster can also support other types of graphical session designed to provide interactive applications directly to users. To view the available types of session, use the command ``alces session avail``:

.. code:: bash

    [alces@login1(scooby) ~]$ alces session avail
    [ ] base/chrome
    [ ] base/cinnamon
    [*] base/default
    [ ] base/fvwm
    [*] base/gnome
    [ ] base/icewm
    [ ] base/terminal
    [ ] base/trinity
    [ ] base/xfce

Application types that are not marked with a star (``*``) need to be enabled before they can be started. To enable a new session type, use the command ``alces session enable <type>``. Enabling a new session type will automatically install any required application and support files. Once enabled, users can start a new session using the command ``alces session start <type>``.

.. note:: Enabling a new session type only enables it for the machine that you run the command from, any other nodes will need to have the type enabled too.

Viewing and terminating running sessions
----------------------------------------

Users can view a list of the currently running sessions by using the command ``alces session list``. One standard Flight Compute login node supports up to 10 sessions running at the same time. 

.. code:: bash

    [alces@login1(scooby) ~]$ alces session list
    +----------+------------+----------------------+-----------------+---------+------+----------+
    | Identity | Type       | Host name            | Host address    | Display | Port | Password |
    +----------+------------+----------------------+-----------------+---------+------+----------+
    | b7d8e878 | gnome      | login1               | 52.49.121.188   |      :1 | 5901 | JqQJWkA5 |
    | ce4c4372 | cinnamon   | login1               | 52.49.121.188   |      :2 | 5902 | V9r2IuXb |
    | d1d8342e | gnome      | login1               | 52.49.121.188   |      :3 | 5903 | 1HJRftxP |
    | d4c69a18 | terminal   | login1               | 52.49.121.188   |      :4 | 5904 | 0du74LNn |
    | d6d5f7cc | chrome     | login1               | 52.49.121.188   |      :5 | 5905 | YbR8vkFy |
    +----------+------------+----------------------+-----------------+---------+------+----------+


To display connection information for an existing session, use the command ``alces session info <session-ID>``. This command allows users to review the IP-address, port number and one-time password settings for an existing session. 

.. code:: bash

    [alces@login1(scooby) ~]$ alces session info b7d8e878
    Identity:     b7d8e878-19b7-11e6-96cc-0a949a3e07d9
    Type:         gnome
    Host name:    login1
    Host address: 52.49.121.188
    Port:         5901
    Display:      1
    Password:     JqQJWkA5
    Websocket:    41361
    URL:          vnc://alces:JqQJWkA5@52.49.121.188:5901

Users can terminate a running session by ending their graphical application (e.g. by logging out of a Gnome session, or exiting a terminal session), or by using the ``alces session kill <session-ID>`` command. A terminated session will be immediately stopped, disconnecting any users. 



Securing your graphical desktop session
=======================================

As the VNC protocol does not natively provide support for security protocols such as SSL, you may wish to take steps to secure access to your VNC sessions. For Flight Solo cluster users, access to your VNC desktop will be restricted to the ``CIDR`` entry provided a launch time - i.e. only users accessing from an authorized IP address will be able to connect to a remote desktop launched on your cluster.


Alces Clusterware VPN
---------------------

Alces Flight clusters are automatically launched with an OpenVPN service, complete with SSL encryption to secure your VNC desktop connection. Refer to the ``alces about vpn`` command to locate the configuration files for your platform.  You can find OpenVPN downloads and documentation for your client system `here <https://openvpn.net/>`__.


Alces Flight Access service
---------------------------

Delivered via the web, Alces Flight Access provides an easy-access overview of the interactive sessions your are running across your clusters. As well as allowing you to start and terminate sessions, Alces Flight Access shows you connection details along with an image of the current state of the session and provides secure, encrypted
access to your interactive sessions directly from within your web browser.

If you are using an Enterprise edition of Alces Flight Compute, please contact us at <support@alces-flight.com> to find out how to provision an Alces Flight Access appliance.


Other solutions
---------------

Several third party tools exist to help you secure your VNC connections.  One option is `ssvnc`, available `here <http://www.karlrunge.com/x11vnc/ssvnc.html>`__.

Alternatively, you could use an SSH tunnel to access your session. `Refer to online guides for setup instructions <http://www.cl.cam.ac.uk/research/dtg/attarchive/vnc/sshvnc.html>`_.


