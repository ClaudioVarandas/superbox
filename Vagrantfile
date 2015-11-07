# -*- mode: ruby -*-
# vi: set ft=ruby :


php_timezone          = "UTC"    # http://php.net/manual/en/timezones.php
php_version           = "5.6"    # Options: 5.5 | 5.6
hhvm                  = "false"

Vagrant.configure(2) do |config|
    config.vm.box = "ubuntu/trusty64"
    config.vm.define "SuperBox" do |superbox|
    end

    config.vm.network "forwarded_port", guest: 80, host: 8000
    config.vm.network "private_network", ip: "172.16.100.100"


    # If using VirtualBox
    config.vm.provider :virtualbox do |vb|

      vb.name = "superbox"
      # Set server cpus
      vb.customize ["modifyvm", :id, "--cpus", "2"]
      # Set server memory
      vb.customize ["modifyvm", :id, "--memory", "1024"]

      # Set the timesync threshold to 10 seconds, instead of the default 20 minutes.
      # If the clock gets more than 15 minutes out of sync (due to your laptop going
      # to sleep for instance, then some 3rd party services will reject requests.
      vb.customize ["guestproperty", "set", :id, "/VirtualBox/GuestAdd/VBoxService/--timesync-set-threshold", 10000]

      # Prevent VMs running on Ubuntu to lose internet connection
      # vb.customize ["modifyvm", :id, "--natdnshostresolver1", "on"]
      # vb.customize ["modifyvm", :id, "--natdnsproxy1", "on"]

    end

    # Use NFS for the shared folder
    config.vm.synced_folder ".", "/vagrant",
              id: "core",
              :nfs => true,
              :mount_options => ['nolock,vers=3,udp,noatime,actimeo=2']
    # Provision Base Packages
    config.vm.provision "shell", path: "./scripts/base.sh"
    # optimize base box
    config.vm.provision "shell", path: "./scripts/base_box_optimizations.sh", privileged: true
    # Provision PHP
    config.vm.provision "shell", path: "./scripts/php.sh", args: [php_timezone, hhvm, php_version]

end
