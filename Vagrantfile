# -*- mode: ruby -*-
# vi: set ft=ruby :
require 'yaml'
settings = YAML.load_file 'vagrant.yml'

Vagrant.configure("2") do |config|
  config.vm.box = "ubuntu/bionic64"

  # config.trigger.before [:destroy, :halt] do |trigger|
  #   trigger.info = "Dump database"
  #   trigger.run_remote = {
  #     inline: "pg_dump -Fc #{settings['project_name']} -f /vagrant/#{settings['project_name']}.dump",
  #     privileged: false
  #   }
  # end

  config.ssh.insert_key = false
  config.vm.network "private_network", ip: settings['ip_address']
  config.vm.network "forwarded_port", guest: 8000, host: 8000 # Main site
  config.vm.network "forwarded_port", guest: 5432, host: 5434 # Postgres

  config.vm.synced_folder "src/", settings['home_directory'] + "/app"
  # config.vm.synced_folder "../test_outside/", settings['home_directory'] + "/separate_app"

  # Set up node_modules before anything else, doesn't work within ansible :(
  config.vm.provision "shell", inline: <<-SHELL
    echo "Preparing local node_modules folder…"
    mkdir #{settings['home_directory']}/node_modules_app
    chown vagrant:vagrant #{settings['home_directory']}/node_modules_app
  SHELL
  config.vm.provision "shell", run: "always", inline: <<-SHELL
    mount --bind #{settings['home_directory']}/node_modules_app #{settings['home_directory']}/app/node_modules
  SHELL


  config.vm.provision "ansible_local" do |ansible|
    ansible.playbook = "setup.yml"
    if settings['python_version'] == 'python3'
      ansible.extra_vars = { ansible_python_interpreter:"/usr/bin/python3" }
    else
      ansible.extra_vars = { ansible_python_interpreter:"/usr/bin/python" }
    end
    ansible.verbose = "v"
  end
end
