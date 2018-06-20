# GSA CKAN 2.3 in Virtual Machine via Vagrant

The GSA/CKAN 2.3 instance installed in a Ubuntu/Trusty64 VirtualBox managed by Vagrant. The documentation is at CKAN; however, it is not _entirely_ complete. The CKAN 2.3.5 Setup Process follows. This process does **not** provision via Ansible, but installs the software directly in the virtual box manually.

## Host System Pre-requisites

Before a virtual system using Vagrant can be created, the system must be prepared. If the host does not already have the following items installed, installation is required:

1. Download the host system appropriate package from [Oracle VirtualBox](https://www.virtualbox.org/) and install it, **following the instructions closely**. Packages exist for Windows, macOS, Linux, and Solaris.
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

Differences exist between Development, Testing, and Production, as shown in the table below. Plugins and extensions are not included in the table because they are not part of system requirements. They will be detailed later.

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
| **Data Management System** | CKAN 2.3 | Same | Same |
| **Initiation File** | development.ini | UNKNOWN NOW | production.ini |

Please review the [general CKAN 2.3 information](http://docs.ckan.org/en/ckan-2.3.5/ "Title") before proceeding.

#### Preparing Trusty for CKAN

Prior to installing CKAN and Solr, the box must be prepared because the Ubuntu/Trusty64 Vagrant box pulled from the Vagrant repository if essentially empty. Consequently a few appetizers need to be circulated before the main course is served.

1. At the host machine command line (or at an IDE's command line if using one), run `vagrant up` using the provided Vagrantfile. And wait.
2. Vagrant will create a .vagrant directory and fill it will a few some files.
3. Watch the termina
l scroll. If you see a message the indicates the box needs updating, move to Step 4, otherwise jump to step 7.
4. After the machine has launched fully the first time, from the command line shut it down by typing `vagrant halt`.
5. Run the box update command, `vagrant box update`. The update function will run, updating the box to its most recent version.
6. Rerun `vagrant up`, which will restart/recreate the updated box.
7. Once the box instance's GUI window opens, log into the box. Use the vagrant account. Its login and password is the same as the account name. **_Do not upgrade_** the box to Ubuntu 16.04. At this initial stage, this box has a command line interface _only_. That will change later.
8. Stop the box again with `vagrant halt`.

##### Use the Virtual Box GUI to Modify Box Settings

The settings are specialized for this project. A video was created to show what the settings need to be. If creating a box from scratch, please as eKuber Ventures for access to the video.

Once the settings are adjusted, bring the box up again.

##### Begin Installation of Packages Required by CKAN

1. Verify that Python 2.7.6 is installed in the default box by typing: `python --version`. It should return Python 2.7.6, which is the required version and the version that should be installed by default.
2. If Python is not installed, run `sudo apt-get install python2.7-minimal==2.7.6-8ubuntu0.2`.
3. Now look at the [Install CKAN 2.3 from Source](http://docs.ckan.org/en/ckan-2.3.5/maintaining/installing/install-from-source.html "Title") information. Read it carefully. It must be installed from source _because the source is actually **GSA's CKAN fork**_, **NOT** from the official CKAN repository itself.
4. Complete step 1 in the instructions: _Install the required packages_. When finished, the following should be installed: python-dev, postgresql, libpq-dev, python-pip, python-virtualenv, git-core, solr-jetty, and openjdk-6-jdk. Please notice that the Java JDK is the **wrong** version. It will be modify later. **Do not modify it now**, it will break the installation process.

* [VirtualBox App Setup/Fixes](http://www.bogotobogo.com/Linux/Ubuntu_Desktop_on_Mac_OSX_using_VirtualBox_4_3_II.php "Title")

