# -*- mode: ruby -*-
# vi: set ft=ruby :
require 'yaml'
settings = YAML.load_file 'vagrant.yml'

Vagrant.configure("2") do |config|
  config.vm.box = "ubuntu/xenial64"
  config.ssh.insert_key = false
  config.vm.network "private_network", ip: settings['ip_address']
  config.vm.network "forwarded_port", guest: 8000, host: 8000 # Main site
  config.vm.network "forwarded_port", guest: 5432, host: 5434 # Postgres

  config.vm.synced_folder "src/", settings['home_directory'] + "/app"
  config.vm.synced_folder "../test_outside/", settings['home_directory'] + "/separate_app"

  config.vm.provision "ansible_local" do |ansible|
    ansible.playbook = "setup.yml"
    ansible.extra_vars = { ansible_python_interpreter:"/usr/bin/python3" }
    ansible.verbose = "vv"
  end
end
