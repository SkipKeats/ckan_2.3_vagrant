#-*- mode: ruby -*-
# vi: set ft=ruby

# Ubuntu/trusty64 -- Official Ubuntu Server 14.04 LTS (Trusty Tahr)
Vagrant.configure("2") do |config|
    config.vm.box = "ubuntu/trusty64"

    # Check to see if the box is outdated. If so, update
    config.vm.box_check_update = true

    # Configure private network using hostmanager plugin
    config.hostmanager.enabled = true
    config.hostmanager.manage_host = true
    config.hostmanager.ignore_private_ip = false
    config.hostmanager.include_offline = true
    config.vm.hostname = 'ckan.local'
    config.vm.network :private_network, ip: '192.168.10.85'

    # Box configuration changes
    config.vm.provider "virtualbox" do |vb|
        # Display the VirtualBox GUI when booting the machine
        vb.gui = true

        # Edit the default box name
        vb.name = "Trusty64: CKAN 2.3"

        # Customize the amount of memory on the VM
        vb.memory = "4096"
    end 
end