# -*- mode: ruby -*-
# vim: set ft=ruby :

Vagrant.configure(2) do |config|
  config.vm.box = "debian/bullseye64"
  config.vm.box_version = "11.20230615.1"
  config.vm.provider "virtualbox" do |v|
    v.memory = 1024
    v.cpus = 1
  end

  config.vm.define "Zabbix" do |zab|
    zab.vm.network "forwarded_port", guest: 80, host: 8080
    zab.vm.hostname = "Zabbix"
  end
end
