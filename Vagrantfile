# -*- mode: ruby -*-
# vi: set ft=ruby :
Vagrant.configure("2") do |config|

  config.vm.box = "debian/bookworm64"

  config.vm.provision "shell", inline: <<-SHELL
      apt-get update
      apt-get install -y bind9 bind9-doc bind9utils 
    SHELL

    config.vm.define "dnsa" do |dnsa|
      dnsa.vm.hostname = "dnsa.ies.test"
      dnsa.vm.network "private_network", ip: "192.168.57.10"
      
      dnsa.vm.provision "shell", inline: <<-SHELL
        cp /vagrant/config/DNSA/named.conf.local /etc/bind/named.conf.local
        mkdir /etc/bind/zones
        cp /vagrant/config/dnsa/zones/* /etc/bind/zones/
        systemctl restart bind9
      SHELL
    end # dnsa
  
    config.vm.define "dnsb" do |dnsb|
      dnsb.vm.hostname = "dnsb.ies.test"
      dnsb.vm.network "private_network", ip: "192.168.57.11"
  
      dnsb.vm.provision "shell", inline: <<-SHELL
          cp /vagrant/config/dnsb/named.conf.local /etc/bind/named.conf.local
        systemctl restart bind9
      SHELL
    end # dnsb  l
end
