# coding: utf-8
# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure("2") do |config|

  ######################################################################
  #
  # Opciones GLOBALES
  #
  ######################################################################
  
  config.vm.box = "debian/bullseye64"
  
  config.vm.provider "virtualbox" do |vb|
    vb.memory = "256"
    vb.linked_clone = true
  end # virtualbox

  # Actualizar SO
  config.vm.provision "update", type:"shell", inline: <<-SHELL
    apt-get update 
  SHELL

  ######################################################################
  #
  # Servidor 'atlas'
  #
  ######################################################################
  
  config.vm.define "atlas" do |atlas|
    # Poner nombre de hosts
    atlas.vm.hostname = "atlas"
    # Tarjeta en red interna de atlas
    atlas.vm.network "private_network", ip: "192.168.57.10"
    # Instalar bind9
    atlas.vm.provision "bind9-install", type:"shell", inline: <<-SHELL
        apt-get update
        apt-get install -y bind9 bind9-utils bind9-doc
    SHELL

    # Copiar ficheros configuración 
    
    atlas.vm.provision "config", type:"shell", inline: <<-SHELL
      cp -v /vagrant/atlas-conf/resolv.conf /etc/
      # Copiar resto ficheros
      cp /vagrant/atlas-conf/192.168.57.dns /etc/bind/
      cp /vagrant/atlas-conf/named.conf.local /etc/bind/
      cp /vagrant/atlas-conf/named.conf.options /etc/bind/
      cp /vagrant/atlas-conf/olimpo.test.dns /etc/bind/
      systemctl restart bind9
      systemctl status bind9
    SHELL

  end # servidor

  ######################################################################
  #
  # Servidor 'ceo'
  #
  ######################################################################
  
  config.vm.define "ceo" do |ceo|
    # Poner nombre de host
    ceo.vm.hostname = "ceo"
    # Tarjeta en red interna de ceo
    ceo.vm.network "private_network", ip: "192.168.57.11"
    # Instalar bind9
    ceo.vm.provision "bind9-install", type:"shell", inline: <<-SHELL
        apt-get update
        apt-get install -y bind9 bind9-utils bind9-doc
    SHELL

    # Copiar ficheros configuración 
    
    ceo.vm.provision "config", type:"shell", inline: <<-SHELL
      cp -v /vagrant/ceo-conf/resolv.conf /etc/
      # Copiar resto ficheros
      cp /vagrant/ceo-conf/named.conf.local /etc/bind/
      cp /vagrant/ceo-conf/named.conf.options /etc/bind/
      systemctl restart bind9
      systemctl status bind9
    SHELL

  end # servidor

end # Configure
