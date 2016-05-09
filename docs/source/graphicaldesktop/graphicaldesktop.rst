.. _graphicaldesktop:

Graphical desktop access to your login node
###########################################

Your Alces Flight Compute login node can also run graphical desktop sessions to support users who want to run interactive applications across the cluster. The system can support a number of different sessions simultaneously, and allow multiple remote participants to connect to the same session to support training and collaboration activities. 


Launching a desktop session
===========================

All Flight Compute clusters come pre-installed with a Gnome desktop environment which users can start from the command-line as required. Users can launch a new session by using the ``alces session start gnome`` command. After launching the desktop, a message will be printed with connection detail to access the new session:

<insert screenshot of output>

Users need a VNC client to connect to the graphical desktop session - a list of tested clients is provided on the page `whatisit` page. Users with Mac clients can use the URL provided in the command output to connect to the session. Linux and Windows users should enter the IP address and port number shown into their VNC client in the format ``IP:port``. For example - for the output above, Linux and Windows client users would enter ``1.2.3.4:5901`` into their VNC client:

<insert screenshot of VNC client>

A one-time randomized password is automatically generated automatically by Flight Compute when a new session is started. Linux and Windows users may be prompted to enter this password when they connect to the desktop session. 

Once connected to the graphical desktop, users can use the environment as they would a local Linux machine:

<screenshot of desktop>


Connecting multiple users to the same session
---------------------------------------------

New graphical sessions are created in multi-user mode by default, allowing users from different locations to connect to the same session for training or collaborative projects. All users connect using the same connection details (e.g. IP-address and port number), and use the same one-time password. 

Please note that users are only permitted to connect to your Flight Compute cluster login node if their IP address is within the set of networks allowed your your ``CIDR`` setting made at launch time. If you have issues with secondary users connecting to a graphical desktop session, please try entering ``0.0.0.0/0`` as your CIDR at cluster launch time to allow access from all users. 


Resizing the desktop to fit your screen
---------------------------------------

By default, your graphical desktop session will launch with a compatibility resolution of 1024x768. Users can resize the desktop to fit their screens using the Linux ``xrandr`` command, run from within the graphical desktop session. 

To view the available screen resolutions, start a terminal session on your graphical desktop by navigating to the ``System`` menu in the top left-hand corner of the screen, then selecting the ``Terminal`` under the ``System tools`` menu.

<screenshot of starting a terminal>

The ``xrandr`` command will display a list of available resolutions supported by your desktop:

<screenshot of xrandr>

To set a new resolution, run the ``xrandr`` command again with the ``-s <resolution>`` argument; 

  - e.g. to change to 1280x1024, enter the command ``xrandr -s 1280x1024``
  
Your graphical desktop session will automatically resize to the new resolution requested. 


Using alces session commands to enable other types of session
-------------------------------------------------------------

Your Alces Flight Compute cluster can also support other types of graphical sessions designed to provide interactive applications directly to users. To view the available types of session, use the command ``alces session avail``:

<screenshot>

Application types that are not marked with a star (``*``) need to be enabled before they can be started. To enable a new session type, use the command ``alces session enable <type>``. Enabling a new session type will automatically install any required application and support files. Once enabled, users can start a new session using the command ``alces session start <type>``.

<screenshot>

Viewing and terminating running sessions
----------------------------------------

Users can view a list of the currently running sessions by using the command ``alces session list``. A standard Flight Compute login node supports up to XXX sessions running at the same time. 

<screenshot>

To display connection information for an existing session, use the command ``alces session info <session-ID>``. This command allows users to review the IP-address, port number and one-time password settings for an existing session. 

<screenshot>

Users can terminate a running session by using the ``alces session stop <session-ID>`` command. A terminated session will be immediately stopped, disconnecting any users. 
