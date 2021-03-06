# -*- mode: ruby -*-
# vi: set ft=ruby :
default_box = "opensuse/Leap-15.3.x86_64"

# All Vagrant configuration is done below. The "2" in Vagrant.configure
# configures the configuration version (we support older styles for
# backwards compatibility). Please don't change it unless you know what
# you're doing.
Vagrant.configure("2") do |config|
  # The most common configuration options are documented and commented below.
  # For a complete reference, please see the online documentation at
  # https://docs.vagrantup.com.

  # Every Vagrant development environment requires a box. You can search for
  # boxes at https://vagrantcloud.com/search.

  config.vm.define "k3s" do |k3s|
    k3s.vm.box = default_box
    k3s.vm.hostname = "k3s"
    k3s.vm.network 'private_network', ip: "192.168.33.10",  virtualbox__intnet: true
    k3s.vm.network "forwarded_port", guest: 22, host: 2222, id: "ssh", disabled: true
    k3s.vm.network "forwarded_port", guest: 22, host: 2000 # Master Node SSH
    k3s.vm.network "forwarded_port", guest: 32747, host: 32747 # grafana NodePort
    k3s.vm.network "forwarded_port", guest: 3000, host: 3000 # grafana Port in k8
    k3s.vm.network "forwarded_port", guest: 8080, host: 8080 # hotrod
    k3s.vm.network "forwarded_port", guest: 16686, host: 8088 # <jaegerapp>-query in k8 
    k3s.vm.network "forwarded_port", guest: 30567, host: 30567 # hotrod app nodeport
    k3s.vm.network "forwarded_port", guest: 30686, host: 30686 # hotrod-query nodeport1
    k3s.vm.network "forwarded_port", guest: 30685, host: 30685 # hotrod-query nodeport2
    k3s.vm.network "forwarded_port", guest: 8888, host: 8888 # sample app fe
    k3s.vm.network "forwarded_port", guest: 8000, host: 8000 # sample app be
    k3s.vm.network "forwarded_port", guest: 9090, host: 9090 # prometheus UI  
    k3s.vm.network "forwarded_port", guest: 6443, host: 6443 # API Access
    for p in 30000..30100 # expose NodePort IP's
      k3s.vm.network "forwarded_port", guest: p, host: p, protocol: "tcp"
      end
    k3s.vm.provider "virtualbox" do |v|
      v.memory = "4096"
      v.name = "k3s"
      end
    k3s.vm.provision "shell", inline: <<-SHELL
      sudo zypper refresh
      sudo zypper --non-interactive install bzip2
      sudo zypper --non-interactive install etcd
      curl -sfL https://get.k3s.io | sh -
      sudo zypper --non-interactive install -t pattern apparmor
      curl -sfL https://get.k3s.io | sh -s - --kube-apiserver-arg=service-node-port-range=3000-32767
    SHELL
  end   


end