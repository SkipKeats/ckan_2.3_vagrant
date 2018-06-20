# GSA CKAN 2.3 in Virtual Machine via Vagrant

The GSA/CKAN 2.3 instance installed in a Ubuntu/Trusty64 VirtualBox managed by Vagrant. The documentation is at CKAN; however, it is not _entirely_ complete. The CKAN 2.3.5 Setup Process follows,

## Host System Pre-requisites

Before a virtual system using Vagrant can be created, the system must be prepared. If the host does not already have the following items installed, installation is required:

1. Download the host system appropriate package from [VirtualBox](https://www.virtualbox.org/) and install it, **following the instructions closely**. Packages exist for Windows, macOS, Linux, and Solaris.
2. Then download and install [Vagrant](https://www.vagrantup.com/ "Title").
3. Then install two Vagrant plugins, listed below
4. The Vagrant VirtualBox Guest: `vagrant plugin install vagrant-vbguest`.
5. The Vagrant Hostmanager: `vagrant plugin install vagrant-hostmanager`.

Now the host system should be ready to procede.

## Build the Vagrant file -- _Vagrantfile_

Two methods exist for this arrangement, the Quick and the Dirty. The Quick is covered first, the Dirty afterwards as it is more complicated.

### The Quick

This method requires changing only the name of the preferred box in the Vagrant file, but the box must be a pre-configured for a CKAN stage: Development Testing (_CKAND_), Plugin and Extension Testing (_CKANPE_), or Production Testing (_CKANP_).

If using a pre-created CKAN-prepared Vagrant Box, use the included Vagrantfile, but edit line 6, replacing `config.vm.box = "ubuntu/trusty64"` with `config.vm.box = "Name of box needed"` where **Name of box needed** matches one of the boxed noted above.

Save the file and type `vagrant up`. Once the box loads, the virtual machine should be ready to go!

### The Dirty

Why the Dirty? Because a box must be built out from a basic Vagrant box to one that holds the glory that is CKAN. The following process details what must be done to ready a box. This process assumes the Vagrant box in use is [Ubuntu/Trusty64](https://app.vagrantup.com/ubuntu/boxes/trusty64 "Title"), which pulls from [Vagrant Box Search](https://app.vagrantup.com/boxes/search "Title") directly.

#### Differences between CKAN Development and Production

Differences exist between Development, Testing, and Production, as shown in the table below. Plugins and extensions are not included in the table because 

##### Environment Requirements

Server/Software | Development | Testing | Production |
|---------------|-------------|---------|------------|
| **Server** | Ubuntu 14.04 LTS (Trusty) - 64 bit | Same | Same |
| **Language** | Python v 2.7.6| Same    | Same       |
| **Java Runtime** | Java v 1.7.0_171; OpenJDK Runtime Env \(IcedTea 2.6.13\) \(7u171-2.6.13-0ubuntu0.14.04.2\) | Same | Same |
| **Web Server** | Paster | Paster | Apache 2.4.7 |
| **Java Server** | Eclipse Jetty v 6 | Eclipse Jetty 6 | Apache Tomcat 6 |
| **Search Server** | Solr v 4.2.1 | Same | Same |
| **NoSQL Server** | Redis v 2.8.4 | Same | Same |
| **SQL Server** | Postgres SQL v 9.3 | Same | Same |
| **DMS** | CKAN 2.3 | Same | Same |



* [General CKAN 2.3 Information](http://docs.ckan.org/en/ckan-2.3.5/ "Title")
* [Install CKAN 2.3 from Source](http://docs.ckan.org/en/ckan-2.3.5/maintaining/installing/install-from-source.html 
"Title")
* [VirtualBox App Setup/Fixes](http://www.bogotobogo.com/Linux/Ubuntu_Desktop_on_Mac_OSX_using_VirtualBox_4_3_II.php "Title")

