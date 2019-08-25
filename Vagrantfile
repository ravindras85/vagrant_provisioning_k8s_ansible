# -*- mode: ruby -*-
# vi: set ft=ruby :

ENV['VAGRANT_NO_PARALLEL'] = 'yes'

IMAGE_NAME = "bento/ubuntu-16.04"

Vagrant.configure("2") do |config|

  config.vm.provision "ansible" do |ansible|
    ansible.playbook = "playbook.yml"

##Kubernetes Master Node  
  config.vm.define "master" do |master|
    master.vm.box = IMAGE_NAME
    master.vm.hostname = "master"
    master.vm.network "private_network", ip: "192.168.50.10"
    master.vm.provider "virtualbox" do |v|
      v.name = "k8s-master"
      v.memory = 1024
      v.cpus = 2
    end
    master.vm.provision "ansible_local" do |ansible|
      ansible.playbook = "kubernetes-setup/master-playbook.yml"
    end
  end

N = 2

##Kubernetes Worker Node
(1..N).each do |i|
  config.vm.define "k8s-worker#{i}" do |workernode|
    workernode.vm.box = IMAGE_NAME
    workernode.vm.network "private_network", ip: "192.168.50.1#{i}"
    workernode.vm.hostname = "k8s-worker#{i}"
    workernode.vm.provider "virtualbox" do |v|
      v.name = "k8s-worker#{i}"
      v.memory = 1024
      v.cpus = 1
    end
    workernode.vm.provision "ansible_local" do |ansible|
      ansible.playbook = "kubernetes-setup/node-playbook.yml"
    end
  end
end
end
end
