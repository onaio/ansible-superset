# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure("2") do |config|
  config.vm.provision "ansible_local" do |ansible|
    ansible.playbook = "tests/test.yml"
    ansible.galaxy_role_file = "tests/requirements.yml"
    ansible.verbose = "v"
  end

  config.vm.provider "virtualbox" do |v|
    v.memory = 1024
    v.cpus = 2
  end

  config.vm.network "forwarded_port", guest: 8000, host: 8888
  config.vm.box = "ubuntu/xenial64"
  config.ssh.forward_agent = true
end