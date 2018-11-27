# -*- mode: ruby -*-
# vi: set ft=ruby :

# All Vagrant configuration is done below. The "2" in Vagrant.configure
# configures the configuration version (we support older styles for
# backwards compatibility). Please don't change it unless you know what
# you're doing.
Vagrant.configure("2") do |config|
  config.vm.box = "ubuntu/xenial64"
  config.ssh.insert_key = false
  config.vm.network "private_network", ip: "192.168.0.10"
  config.vm.network "forwarded_port", guest: 8000, host: 8000 # Main site
  config.vm.network "forwarded_port", guest: 5432, host: 5434 # Postgres

  config.vm.provision "ansible_local" do |ansible|
    ansible.playbook = "setup.yml"
    ansible.extra_vars = { ansible_python_interpreter:"/usr/bin/python2" }
  end
end
