# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure(2) do |config|

    if !ENV['VAGRANT_DEFAULT_PROVIDER']
        puts " [!] VAGRANT_DEFAULT_PROVIDER environment is not set, required!"
        exit 1
    end

    if !ENV['MY_DEV_PATH']
        puts " [!] MY_DEV_PATH environment is not set, required!"
        exit 1
    end

    puts
    puts "=> Using VAGRANT_DEFAULT_PROVIDER=#{ENV['VAGRANT_DEFAULT_PROVIDER']}"
    puts "=> Using MY_DEV_PATH=#{ENV['MY_DEV_PATH']}"
    puts

    # middleman
    config.vm.network "forwarded_port", guest: 4567, host: 4567
    # Rails
    # config.vm.network "forwarded_port", guest: 3000, host: 3000
    # # Redis
    # config.vm.network "forwarded_port", guest: 6379, host: 6379

    if ENV['VAGRANT_DEFAULT_PROVIDER'] == 'parallels'
        config.vm.provider "parallels" do |parallels|
            config.vm.box = 'parallels/ubuntu-14.04'
            config.vm.synced_folder ENV['MY_DEV_PATH'], "/develop"

            parallels.memory = 1024
            parallels.cpus = 2
            parallels.customize ["set", :id, "--on-window-close", "keep-running"]
            # parallels.update_guest_tools = false
            # parallels.check_guest_tools = false
        end
    end


    if ENV['VAGRANT_DEFAULT_PROVIDER'] == 'virtualbox'
        config.vm.network "private_network", ip: "192.168.33.57" # required for 'nfs' synced_folders (a private network with static IP)

        config.vm.provider "virtualbox" do |vb|
            config.vm.box = "ubuntu/trusty64"

            config.vm.synced_folder ENV['MY_DEV_PATH'], "/develop", type: "nfs" # NFS synced_folders require a 'private_network' to be configured!

            # Don't boot with headless mode
            # vb.gui = false

            # Use VBoxManage to customize the VM. For example to change memory:
            # vb.customize ["modifyvm", :id, "--memory", "3072"]
            vb.memory = 1024
            vb.cpus = 2
        end
    end

    # Install latest docker
    config.vm.provision "docker"

    config.vm.provision :ansible, :playbook => "vagrant_files/playbook.yml"
end
