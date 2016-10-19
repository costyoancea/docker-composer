# -*- mode: ruby -*-
# vi: set ft=ruby :

VAGRANTFILE_API_VERSION = "2"

Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|
    config.vm.hostname = "oreilly.local"

    required_plugins = %w(vagrant-vbguest vagrant-hostmanager vagrant-docker-compose)

    plugins_to_install = required_plugins.select { |plugin| not Vagrant.has_plugin? plugin }
    if not plugins_to_install.empty?
        puts "Installing plugins: #{plugins_to_install.join(' ')}"
        if system "vagrant plugin install #{plugins_to_install.join(' ')}"
            exec "vagrant #{ARGV.join(' ')}"
        else
            abort "Installation of one or more plugins has failed. Aborting."
        end
    end

    if Vagrant.has_plugin?("vagrant-hostmanager")
        config.hostmanager.enabled = true
        config.hostmanager.manage_host = true
        config.hostmanager.manage_guest = true
        config.hostmanager.ignore_private_ip = false

    end

    config.vm.box = "ubuntu/trusty64"
    config.vm.box_check_update = true

    config.vm.synced_folder ".", "/vagrant", disabled: true
    config.vm.synced_folder "..", "/var/www", create: true

    config.vm.provision :docker
    config.vm.provision :docker_compose

    #  config.vm.provision :docker_compose, yml: "/vagrant/docker-compose.yml", run: "always"


    # Get current folder name
    @vagrant_root = File.basename(Dir.getwd)

    config.vm.provision "shell", path: "docker.sh", args: @vagrant_root, run: "always"

end
