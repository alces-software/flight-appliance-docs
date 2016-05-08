.. _whatisit:

What is Alces Flight Compute?
=============================

Alces Flight Compute is a software appliance designed to help researchers and scientists build their own high-performance compute cluster quickly and easily. The basic structure provided for users is as follows:

 - One login node, plus a configurable number of compute nodes
 - An Enterprise Linux operating system
 - A shared filesystem, mounted across all nodes
 - A batch job scheduler
 - Access to a library of software applications

Flight is designed to get researchers started with HPC as quickly as possible, providing a pre-configured environment which is ready for work immediately. The cluster you build is personal to you - users have root-access to the environment, and can setup and configure the system to their needs. 

Who is it for?
--------------

**Flight is designed for use by end-users** - that's the scientists, researchers, engineers and software engineers who actually run compute workloads and process data. This documentation is designed to help these people to get the best out this environment, without needing the assistance of teams of IT professionals. Flight provides tools which enable users to service themselves - it's very configurable, and can be expanded by individual users to deliver a scalable platform for computational workloads. 


What doesn't it do?
-------------------

**An important part of having ultimate power to control your environment is taking responsibility for it.** While no one is going to tell you how you should configure your cluster, you need to remember good security practice, and look after both your personal and research data. Flight provides you with a personal, single-user cluster - please use responsibility. Flight is not a replacement for your national super-computer centre.

If you're running Flight on AWS, then there are some great tutorials written by the Amazon team on how to secure your environment. Just because you're running on public cloud, doesn't mean that your cluster is any less secure than a cluster running in your basement. Start at `AWS Security Pages <https://aws.amazon.com/security>`_, and talk to a security expert if you're still unsure.

For Flight clusters running on OpenStack, you need to contact the administration team who gave you your OpenStack account. They are usually helpful and friendly people, so try and explain what you want to do as clearly and concisely as possible. If you are having problems accessing the cluster once it's launched, remember that you can always do a few trials runs on AWS first, just to make sure you've got the hang of it - the Flight software is the same, whatever platform it's running on.


How much does it cost?
----------------------

Alces Flight Compute is an open-source software appliance and is freely available to users, released under the General Public License (GPL). Please see `Alces Flight EULA <https://s3-eu-west-1.amazonaws.com/flight-aws-marketplace/2016.1/EULA.txt>`_ for details. 

You are likely to incur infrastructure costs from your platform provider (i.e. AWS, or your local OpenStack team) if you are using their compute resources. This documentation will highlight the configuration points that can effect how much you might be charged, but a full cost estimate for your work is outside the scope of Flight documentation. Your platform providers are there to help though - if in doubt, start small (e.g. a handful of nodes for a few hours) and review the costs before scaling your workload up. It's worth spending some time reviewing your costs over the longer term as well, as you may be able to reduce the bill if you run jobs at a different time of day, on different types of resources, or even in a different geographic region if your datasets allow. 


Prerequisites
-------------

You're going to need access to some computers. If you already have access to cloud resources, then great - you're ready to go. There are some specific requirements depending on your platform type, which are discussed in the relevant chapters of this guide. If you don't have access to anything yet then that's fine too - just sign up for an AWS account now and you'll have all the access you need. 

You'll need a client device as well - something to log into your cluster from. The requirements on client devices are fairly minimal, but you'll need:

 - **A computer with a screen and a keyboard**; you can probably access on a tablet or smartphone too, if your typing is good enough
 - **A relatively modern web-browser**; Microsoft Edge, Google Chrome, Mozilla Firefox, Apple Safari and pretty much anything else that was written/updated in the last couple of years should all be fine.
 - **An SSH client**; this comes built-in for Linux and Mac based machines, but you'll need to install one for Windows clients or some tablets.
 - **Internet access**; it seems dumb to list this as a requirement for running HPC on cloud resources, but a surprising number of sites actually limit outbound connectivity to the Internet. 
 - ***Optionally*; A graphical data transfer tool**; you don't actually need one of these, but they can really help new users get started more quickly. 
 

It's worth checking for centrally-managed client systems that you can install the software that you need - some research sites don't allow users to install new software. Here are some recommendations of software that you can use on client machines; this is far from a complete list, but should help you get started:

 - SSH client:
     - Use the built-in ``ssh`` client for Mac and Linux
     - For Windows, try `Putty <http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html>`_ or `SmaTTY <http://smartty.sysprogs.com/>`_
     
 - Web-browser:
     - Use the built-in Safari browser On Macs
     - For Linux and Windows, install `Firefox <http://www.mozilla.org/firefox>`_ or `Chrome <https://www.google.com/chrome/browser/desktop/>`_
     
 - VNC (Graphical desktop client):
     - Use the built-in VNC client on Macs
     - For Linux, install "vncviewer" package, or install `RealVNC viewer <https://www.realvnc.com/download/viewer/linux/>`_
     - For Windows, install `TurboVNC <https://sourceforge.net/projects/turbovnc/>`_
     
 - Graphical file-transfer tools:
     - For Macs and Linux, install `Cyberduck <http://cyberduck.ch/>`_ or `Filezilla <https://filezilla-project.org/>`_
     - For Windows, try `WinSCP <https://winscp.net/>`_, `Cyberduck <http://cyberduck.ch/>`_ or `Filezilla <https://filezilla-project.org/>`_

We've tried to make recommendations for open-source and/or free software client software here - as ever, please read and obey the licensing terms, and try to contribute to the supporting projects either financially, or by referencing them in your research publications. 


Where can I get help?
---------------------

This documentation is designed to walk users through the first stages of creating their clusters, and getting started in the environment. Capable users with some experience can be up and running in a handful of minutes - don't panic if it takes you a little more time, especially if you've not used Linux or HPC clusters before. Firstly - don't worry that you might break something complicated and expensive; one of the joys of having your own personal environment to work in is that no one can see what you did wrong, and nothing is at risk of being broken, aside from the data and work you've done yourself in the environment. 

We encourage new users to run through a few tutorials in this documentation - even if you have plenty of HPC experience, the product moves forward all the time and new features are constantly popping up that could save you effort in future. If you do run into problems, try replicating the steps you went through to get where you are - sometimes a typo in a command early-on in your workflow might not cause any errors until right at the end of your work. It can help to work collaboratively with other researchers running similar jobs - not only are two sets of eyes better than one, you'll both get something out of working together to achieve a shared goal.

There is a community site for supporting the Flight software - `it's available online <https://community.alces-flight.com/>`_. This website is designed to help users share their experiences of running Flight clusters, report any bugs with the software, and share knowledge to help everyone work more effectively. There is no payment required for using this service, except for the general requirement to be nice to each other - if you find the site useful, then please pay the favour back by helping another user with their problem. 

The Flight community support site is a great resource for helping with HPC cluster usage, but for software application support you're going to need to contact the developers of the packages themselves. Each software package installed by Flight comes with a link to the online home of the package (e.g. ``module display apps/gromacs``), where you can highlight any issues to the package maintainers. Remember that many of these software products are open-source and you've paid no fee to use them - try to make your bug-reports and enhancement requests as helpful and friendly as possible to the application developers. They've done you a great service by making their software available for you to use - please be respectful of their time and effort if you need to contact them, and remember to credit their software in your research publications. 

If you're a big company or research group and want to pay for support delivered direct-to-you, then please `contact us <info@alces-flight.com>`. We provide consultancy and targeted support services directly and via a network of partners - it's this that funds the open-source Flight projects. 