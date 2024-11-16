# -*- mode: ruby -*-
# vi: set ft=ruby :
Vagrant.configure("2") do |config|

  config.vm.box = "debian/bookworm64"

  config.vm.provision "shell", inline: <<-SHELL
      apt-get update
      apt-get install -y bind9 bind9-doc bind9utils 
    SHELL
end
