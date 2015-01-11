# -*- mode: ruby -*-
# vi: set ft=ruby :

# Vagrantfile API/syntax version. Don't touch unless you know what you're doing!
VAGRANTFILE_API_VERSION = "2"

Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|
  config.vm.box      = "Centos7LDAP"
  config.vm.box_url     = "https://s3.amazonaws.com/rhcsa/ldap.box"
  config.vm.network   "forwarded_port", guest: 389, host: 389

  config.vm.provider "virtualbox" do |vb|
    vb.customize ["modifyvm", :id, "--memory", "1024"]
    vb.customize ["modifyvm", :id, "--cpus", "2"]
    # following configs resolve a bug causing bundle gem to hang 5 seconds per get
    vb.customize ["modifyvm", :id, "--natdnsproxy1", "off"]
    vb.customize ["modifyvm", :id, "--natdnshostresolver1", "off"]
  end

  config.vm.provision "shell",
    inline: 'sudo yum update -y;'
end
