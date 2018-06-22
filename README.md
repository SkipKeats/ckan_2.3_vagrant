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

This method requires changing only the name of the preferred box in the Vagrant file, but the box must be a pre-configured for a CKAN stage: Development Testing (_CKAN23D_), Development Tools Installed (_CKAN23T_), Plugin and Extension Testing (_CKANP23E_), or Production Testing (_CKAN23P_).

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
3. Watch the terminal scroll. If you see a message the indicates the box needs updating, move to Step 4, otherwise jump to step 7.
4. After the machine has launched fully the first time, from the command line shut it down by typing `vagrant halt`.
5. Run the box update command, `vagrant box update`. The update function will run, updating the box to its most recent version.
6. Rerun `vagrant up`, which will restart/recreate the updated box.
7. Once the box instance's GUI window opens, log into the box. Use the vagrant account. Its login and password is the same as the account name. **_Do not upgrade_** the box to Ubuntu 16.04. At this initial stage, this box has a command line interface _only_. That will change later.
8. Stop the box again with `vagrant halt`.

##### Use the Virtual Box GUI to Modify Box Settings

The settings are specialized for this project. A video was created to show what the settings need to be. If creating a box from scratch, please as eKuber Ventures for access to the video.

Once the settings are adjusted, bring the box up again.

#### Begin Installation of Packages Required by CKAN

1. Verify that Python 2.7.6 is installed in the default box by typing: `python --version`. It should return Python 2.7.6, which is the required version and the version that should be installed by default.
2. If Python is not installed, run `sudo apt-get install python2.7-minimal==2.7.6-8ubuntu0.2`.
3. Now look at the [Install CKAN 2.3 from Source](http://docs.ckan.org/en/ckan-2.3.5/maintaining/installing/install-from-source.html "Title") information. Read it carefully. It must be installed from source _because the source is actually **GSA's CKAN fork**_, **NOT** from the official CKAN repository itself.
4. Complete Step 1 in the instructions: _Install the required packages_. When finished, the following should be installed: python-dev, postgresql, libpq-dev, python-pip, python-virtualenv, git-core, solr-jetty, and openjdk-6-jdk. Please notice that the Java JDK is the **wrong** version. It will be modify later. **Do not modify it now**, it will break the installation process.

#### Installation of Ubuntu Desktop GUI

The next step, before continuing with the CKAN process, is the installation of the Ubuntu Desktop GUI, which is necessary for verifying both CKAN and Solr.

1. Run `sudo apt-get update` to retrieve current packages for update.
2. Run `sudo apt-get install ubuntu-desktop` to begin installation of the desktop. Ubuntu will download a list of packages needed.
3. When the list is complete, the system will request confirmation of installation request. Accept it by typing `Y`.
4. The required packages will install.
5. Once complete, take the system down again with `vagrant halt`.
6. Restart it with `vagrant up`. The system will load, the new GUI will come up.
7. Once loaded, select **Vagrant** as user. Enter the same password as one would at the command line.

##### Customize the Ubuntu GUI

1. Firstly, fix the GUI's default root user issues. The GUI's default user is Ubuntu and it has a blank password. That must be fixed to proceed.
2. Click the icon on the top left of the desktop's sidebar. The tooltip will read _Search your computer and online sources_. A search interface opens. Type `terminal`. Icons representing applications will show below the search line. Select _Terminal_. **Terminal** will launch.
3. At the Terminal prompt, type `sudo passwd ubuntu`. Hit enter. A request for a password will appear. Use the same password as used for the Vagrant account. Repeat a second time. The Ubuntu account will now have a password.
4. Add the Terminal to the sidebar \(if desired\) by right-clicking on the icon and selecting _Lock to Launcher_.
5. Now install the Chromium web browser \(Chrome for Ubuntu\). Click on the _Ubuntu Software Center_ \(Shopping Bag icon with the 'A'\). In the search bar, type `Chromium` and double click the icon for installation.

Now we are ready to proceed.

##### Replace Java JDK 6 with 7

While it may seem odd, the replacement was delayed because swapping the JDKs prior to desktop installation seems to cause unexpected issues. It works better from within the GUI.

Read [this article](https://askubuntu.com/questions/150057/how-can-i-tell-what-version-of-java-i-have-installed "Title") before proceeding.

1. Show which versions are installed: `update-java-alternatives -l`.
2. Run in Terminal `apt-get -s remove openjdk-6-\* icedtea-6-\*` to show dependencies and what will eventually be removed.
3. Then install the openJDK 7 package: `sudo apt-get install openjdk-7-jdk`. [Openjdk-7-jdk details](https://launchpad.net/ubuntu/+source/openjdk-7/7u171-2.6.13-0ubuntu0.14.04.2 "Title").
4. Next run: `sudo apt-get remove openjdk-6-\* icedtea-6-\*`, which will remove version 6.
5. Verify the removal of version 6 by showing which versions remain: `update-java-alternatives -l`.
6. Finally verify that the new version is available for use: `java --version`.

Java is now the version required for the Production server.

##### Install the Virtual Box Guest Additions within the Ubuntu Desktop

The [Virtual Box Guest Additions](https://www.virtualbox.org/manual/ch04.html "Title") are important. They allow copying and drag and drop between the virtual box and the host desktop. Follow these instructions to setup the guest extensions for your particular host system:

* [MacOS](http://www.bogotobogo.com/Linux/Ubuntu_Desktop_on_Mac_OSX_using_VirtualBox_4_3_II.php "Title") **Note:** Make certain the VirtualBox software is up-to-date _before_ proceding.
* [Windows/Linux](https://www.howtogeek.com/howto/2845/install-guest-additions-to-windows-and-linux-vms-in-virtualbox/ "Title")

Once installation is finished, halt the virtual box and bring it back up. Once restored, the additions should be functional.

#### Return to the CKAN Setup instructions

1. In Terminal, resume installation at Step 2: _Install CKAN into a Python virtual environment_. Continue until Step 5, _Setup Solr_, section 1.
2. In a slight modification of the instructions, open Chromium and view the Jetty homepage: [http://localhost:8983/](http://localhost:8983/). If for some reason `localhost` fails, use the IP `127.0.0.1:8983`. The Jetty page should load.
3. Attempting to view the Solr welcome page, however, will result in the error show below in the blockquote:
4. Why? Because the Java update broke the JSP connectivity. Read and [follow the instructions](https://stackoverflow.com/questions/32047288/ckan-solr-jsp-support-not-configured "Title") to resolve the error.
5. Solr's welcome page should now load.
6. With the Solr error resolved, continue on to Step 2, section 2.

> `HTTP ERROR 500`
> `Problem accessing /solr/index.jsp. Reason:`
> `JSP support not configured`
> `Powered by Jetty://`


### Install Finished

If CKAN and Solr are both working, Stage 1 is complete.

#### Running the development CKAN instance again after restarting the Virtual Box

Remember that the CKAN development instance runs within a Python virtual environment, which means that the environment must be activated and the server restarted _before_ anything can be seen in the browser or done in either the browser or command line in the development instance.

The commands from within Terminal at the command prompt are as follows:

1. `. /usr/lib/ckan/default/bin/activate`
2. `cd /usr/lib/ckan/default/src/ckan`
3. `paster serve /etc/ckan/default/development.ini`

### As a Backup, Export the VirtualBox

To create a backup of the VM environment, from within the Oracle VM VirtualBox Manager, click on the menubar and select **File > Export Appliance**. From there, carefully follow the screen instructions.

## Recovering OR Associating a Vagrant Project with an Existing VirtualBox VM

**Important!!!** This process will _only_ work if a copy of the needed VirtualBox VM exists. The box will exist as a file: **ApplianceName.ova**. If such exported applicance does not exist, this procedure _will not work_. Additionally, the recovery assumes a pre-existing .vagrant structure exists as shown below:

`.vagrant`
`    | machines`
`        | default`
            | virtualbox
                |- action_provision
                |- action_set_name
                |- creator_uid
                |- id
                |- index_uuid
                |- private_key
                |- synced_folders
                |- vagrant_cwd
Vagrantfile`


