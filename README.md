# GSA CKAN 2.3 in Virtual Machine via Vagrant

The GSA/CKAN 2.3 instance installed in a Ubuntu/Trusty64 VirtualBox managed by Vagrant.

## CKAN 2.3.5 Setup Process

The documentation is at CKAN; however, it is not _entirely_ complete.

### Host System Pre-requisites

Before a virtual system using Vagrant can be created, the system must be prepared. If the host does not already have the following items installed, installation is required:

1. Download the host system appropriate package from [VirtualBox](https://www.virtualbox.org/) and install it, **following the instructions closely**. Packages exist for Windows, macOS, Linux, and Solaris.
2. Then download and install [Vagrant](https://www.vagrantup.com/ "Title").
3. Then install two Vagrant plugins, listed below
4. The Vagrant VirtualBox Guest: `vagrant plugin install vagrant-vbguest`.
5. The Vagrant Hostmanager: `vagrant plugin install vagrant-hostmanager`.

Now the system should be ready to procede.

create a VirtualBox via Vagrant

* [General CKAN 2.3 Information](http://docs.ckan.org/en/ckan-2.3.5/ "Title")
* [Install CKAN 2.3 from Source](http://docs.ckan.org/en/ckan-2.3.5/maintaining/installing/install-from-source.html 
"Title")
* [VirtualBox App Setup/Fixes](http://www.bogotobogo.com/Linux/Ubuntu_Desktop_on_Mac_OSX_using_VirtualBox_4_3_II.php "Title")

