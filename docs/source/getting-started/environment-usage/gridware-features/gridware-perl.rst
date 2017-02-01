.. _gridware-perl:

Working with Gridware Applications: Perl
========================================

The following guide will detail the installation of Perl, along with some of the advanced features of Alces Gridware when working with the Perl Gridware package. 

Perl Installation
-------------------

Many different versions of Perl are available through the Alces Gridware utility - you can see the list of available packages with the ``alces gridware search`` function, for example: 

.. code:: bash

    [alces@login1(scooby) ~]$ alces gridware search --name perl
    base/apps/perl/5.10.1          base/apps/perl/5.12.4
    base/apps/perl/5.14.2          base/apps/perl/5.16.1
    base/apps/perl/5.16.3          base/apps/perl/5.18.0
    base/apps/perl/5.20.2          base/apps/perl/5.8.8
    base/apps/perl/5.8.9           base/apps/sierraperl/20080201
    base/libs/bioperl/1.2.3        base/libs/bioperl/1.6.901
    base/libs/bioperl/1.6.923

To install, for example - Perl version 5.20.2; run the following command: 

.. code:: bash

    [alces@login1(scooby) ~]$ alces gridware install apps/perl/5.20.2
    Preparing to install main/apps/perl/5.20.2
    Installing main/apps/perl/5.20.2
    Importing apps-perl-5.20.2-el7.tar.gz
    
     > Fetching archive
            Download ... OK
    
     > Preparing import
             Extract ... OK
              Verify ... OK
    
     > Processing apps/perl/5.20.2/gcc-4.8.5
           Preparing ... OK
           Importing ... OK
         Permissions ... OK
    
     > Finalizing import
              Update ... OK
        Dependencies ... OK
    
    Installation complete.


Once the compilation has finished - the Perl 5.20.2 Gridware package will be available for use; check its availability and load using: 

.. code:: bash

    [alces@login1(scooby) ~]$ module avail
    ---  /opt/gridware/local/el7/etc/modules  ---
      apps/perl/5.20.2/gcc-4.8.5
    [alces@login1(scooby) ~]$ module load apps/perl
    apps/perl/5.20.2/gcc-4.8.5
     | -- libs/gcc/system ... SKIPPED (already loaded)
     |
     OK
    [alces@login1(scooby) ~]$ perl --version
    This is perl 5, version 16, subversion 3 (v5.16.3) built for x86_64-linux-thread-multi 
    
Multiple versions of a package can exist at one time, however only one version of a particular application module can be loaded at any one time. To load a different version of Perl: 

.. code:: bash

    [alces@login1(scooby) ~]$ alces module load apps/perl/5.18.0/gcc-4.8.5
    apps/perl/5.18.0/gcc-4.8.5 ... VARIANT (have alternative: apps/perl/5.20.2/gcc-4.8.5)
    [alces@login1(scooby) ~]$ alces module unload apps/perl/5.20.2/gcc-4.8.5
                  apps/perl/5.20.2/gcc-4.8.5 ... UNLOADING --> OK
    [alces@login1(scooby) ~]$ alces module load apps/perl/5.18.0/gcc-4.8.5
    apps/perl/5.18.0/gcc-4.8.5
     | -- libs/gcc/system ... SKIPPED (already loaded)
     |
     OK
    [alces@login1(scooby) ~]$ perl --version
    This is perl 5, version 18, subversion 0 (v5.18.0) built for x86_64-linux

Installation of language libraries
----------------------------------

Through the Alces Gridware utility, installation of lanaguage libraries is possible both on a system-wide level, and also on a per-user basis. The following section details both system-wide language library installation, as well as user-level language library installation. 

System-wide language libraries: Perl
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

As the ``alces`` administrator user, or any other sudo enabled user that can switch to root - change to the ``root`` user account.

Next, load the version of Perl you wish to add language libraries to - for example ``perl/5.20.2``

.. code:: bash

    [root@login1(scooby) ~]# module load apps/perl/5.20.2
    apps/perl/5.20.2/gcc-4.8.5
     | -- libs/gcc/system
     |    * --> OK
     |
     OK

Next - use the ``cpan`` utility to install the Perl libraries you, or additional system users require - for example: 

.. code:: bash

    [root@login1(scooby) ~]# cpan Date::Simple
    Fetching with Net::FTP:
    ftp://cpan.etla.org/pub/CPAN/authors/01mailrc.txt.gz
    Reading '/opt/gridware/share/perl/5.20.2/cpan/sources/authors/01mailrc.txt.gz'
    <--snip-->

The ``Date::Simple`` module will now be available to any system user loading the ``Perl 5.20.2`` Gridware package. 

To verify successful installation, switch to a non-root user; for example ``alces`` will now be able to see and use the ``Date::Simple`` module: 

.. code:: bash

    [alces@login1(scooby) ~]$ module load apps/perl/5.20.2
    apps/perl/5.20.2/gcc-4.8.5
     | -- libs/gcc/system
     |    * --> OK
     |
     OK
    [alces@login1(scooby) ~]$ cpan -l 2>&1 | grep Date::Simple | head -n1
    Date::Simple	3.03


User-specific language libraries: Perl
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Users may also wish to install their own language libraries, these will be unavailable to other users of the environment. 

As the user you wish to install a Perl module for, load the ``perl`` Gridware application, then use ``cpan`` to install the required module: 

.. code:: bash

    [alces@login1(scooby) ~]$ cpan File::Slurp
    Fetching with Net::FTP:
    ftp://cpan.etla.org/pub/CPAN/authors/01mailrc.txt.gz
    Reading '/home/barney/gridware/share/perl/5.20.2/cpan/sources/authors/01mailrc.txt.gz'
    <-- snip -->
    [alces@login1(scooby) ~]$ cpan File::Slurp
    Reading '/home/barney/gridware/share/perl/5.20.2/cpan/Metadata'
      Database was generated on Fri, 19 Feb 2016 02:41:02 GMT
    File::Slurp is up to date (9999.19).

The ``File::Slurp`` installation was successful - and we can now use it as the ``alces`` user. Switching to another user will confirm the user-level installation success; the ``root`` user will not be able to use the ``File::Slurp`` Perl module, requiring the module be installed again: 

.. code:: bash

    [root@login1(scooby) ~]# alces module load apps/perl/5.20.2
    [root@login1(scooby) ~]# cpan File::Slurp
    Fetching with Net::FTP:
    ftp://cpan.etla.org/pub/CPAN/authors/01mailrc.txt.gz
    Reading '/home/alces/gridware/share/perl/5.20.2/cpan/sources/authors/01mailrc.txt.gz'
