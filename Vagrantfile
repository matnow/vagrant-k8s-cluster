# -*- mode: ruby -*-
# vi: set ft=ruby :
IMAGE_NAME = "debian/contrib-buster64"
N = 2

Vagrant.configure("2") do |config|

  config.vm.provider "virtualbox" do |v|
    v.memory = 8000
    v.cpus = 2
  end
      
  config.vm.define "master" do |master|
      master.vm.box = IMAGE_NAME
      master.vm.network "private_network", ip: "192.168.50.10"
      master.vm.hostname = "k8s-master"
      master.vm.provision "ansible_local" do |ansible|
        ansible.playbook = "kubernetes-setup/master-playbook.yml"
        ansible.extra_vars = {
                node_ip: "192.168.50.10",
            }
      end
  end

  (1..N).each do |i|
    config.vm.define "node-#{i}" do |node|
        node.vm.box = IMAGE_NAME
        node.vm.network "private_network", ip: "192.168.50.#{i + 10}"
        node.vm.hostname = "node-#{i}"
        node.vm.provision "ansible_local" do |ansible|
          ansible.playbook = "kubernetes-setup/node-playbook.yml"
          ansible.extra_vars = {
                  node_ip: "192.168.50.#{i + 10}"
              }
        end
    end
  end
end
