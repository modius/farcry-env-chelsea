# -*- mode: ruby -*-
# vi: set ft=ruby :

# Vagrantfile API/syntax version. Don't touch unless you know what you're doing!
VAGRANTFILE_API_VERSION = "2"
# WEB_SERVER must be unique for local private network
WEB_SERVER = "192.168.3.22"
# WEB_HOSTNAME must match ./group_vars/vagrant domains setting
WEB_HOSTNAME = "chelsea.local"

Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|

  # Standard Ubuntu LTS base OS
  config.vm.box = "ubuntu/trusty64"

  # config hostmanager vagrant plugin for friendly dev hostname
  config.hostmanager.enabled = true
  config.hostmanager.manage_host = true
  config.hostmanager.ignore_private_ip = false
  config.hostmanager.include_offline = true

  config.ssh.forward_agent = true

  config.vm.define "web" do |web|
    # set network to private to host with fixed IP address
    web.vm.network :private_network, ip: WEB_SERVER
    
    # set local hostname
    web.vm.hostname = WEB_HOSTNAME

    # port forward virtual web server for troubleshooting at localhost:8080
    config.vm.network "forwarded_port", guest: 80, host: 8080
    
    # synch code and logs out of virtual and into host
    web.vm.synced_folder "code/", "/opt/www", create: true
    web.vm.synced_folder "logs/", "/logs", create: true
    
    # provision via ansible from the virtual machine
    web.vm.provision :shell, :path => "provisioning/ansible-run.sh", :args => "--playbook /vagrant/provisioning/webserver.yml --inventory /vagrant/inventory_temp --group webservers --environment vagrant", keep_color: true

    web.vm.provider "virtualbox" do |vb|
      # configure virtual machine settings
      vb.customize ["modifyvm", :id, "--memory", "1536", "--natdnshostresolver1", "on"]
    end
  end
end
