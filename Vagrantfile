
# -*- mode: ruby -*-
# vi: set ft=ruby :

VAGRANTFILE_API_VERSION = "2"

Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|
  # Use Ubuntu 14.04 Trusty Tahr 64-bit as our operating system
  config.vm.box = "ubuntu/trusty64"
  
  # Configurate the virtual machine to use 2GB of RAM
  config.vm.provider :virtualbox do |vb|
    vb.customize ["modifyvm", :id, "--memory", "2048"]
  end

  # Forward the Rails server default port to the host
  config.vm.network :forwarded_port, guest: 80, host: 80
  config.vm.network :forwarded_port, guest: 3000, host: 3000

  # Use Chef Solo to provision our virtual machine
  config.vm.provision :chef_solo do |chef|
    chef.cookbooks_path = ["cookbooks", "site-cookbooks"]

    chef.add_recipe "apt"
    chef.add_recipe "nodejs"
    chef.add_recipe "ruby_build"
    chef.add_recipe "rvm::system"
    chef.add_recipe "rvm::vagrant"
    chef.add_recipe "vim"
    chef.add_recipe 'postgresql::client'
    chef.add_recipe 'postgresql::server'

    #Install Ruby 2.2.3
    chef.json = {
       rvm: {
          rubies: [
            "2.2.3"
          ],
          default_ruby: "ruby-2.2.3",
          user_default_ruby: "ruby-2.2.3"
        },
        postgresql: {
          password: {
            postgres: "password"
        }
      },
      run_list: ["recipe[postgresql::server]"]
    }

    #needed for bcrypt
    config.vm.provision :shell, :inline => "sudo apt-get install libgmp3-dev -y"

  end

end