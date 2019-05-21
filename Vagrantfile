# -*- mode: ruby -*-
# vi: set ft=ruby :
#

servers = [
    {
        :name => "k8s-master",
        :box => "centos/7",
        :box_version => "1902.01",
        :eth1 => "192.168.205.10",
        :mem => "2048",
        :cpu => "2"
    },
    {
        :name => "k8s-worker1",
        :box => "centos/7",
        :box_version => "1902.01",
        :eth1 => "192.168.205.11",
        :mem => "2048",
        :cpu => "2"
    },
    {
        :name => "k8s-worker2",
        :box => "centos/7",
        :box_version => "1902.01",
        :eth1 => "192.168.205.12",
        :mem => "2048",
        :cpu => "2"
    }
]

Vagrant.configure("2") do |config|

    servers.each do |opts|
        config.vm.define opts[:name] do |config|

            config.vm.box = opts[:box]
            config.vm.box_version = opts[:box_version]
            config.vm.hostname = opts[:name]
            config.vm.network :private_network, ip: opts[:eth1]

            config.vm.provider "virtualbox" do |v|

                v.name = opts[:name]
            	  v.customize ["modifyvm", :id, "--groups", "/Kubernetes Lab"]
                v.customize ["modifyvm", :id, "--memory", opts[:mem]]
                v.customize ["modifyvm", :id, "--cpus", opts[:cpu]]

            end

            #if opts[:type] == "master"
            config.vm.provision "ansible" do |ansible|
                #ansible.become_user = "root"
                ansible.compatibility_mode = "2.0"
                #ansible.limit = opts[:name]
                ansible.playbook = "provision/prepare-node.yml"
            end
            #else
                #config.vm.provision "ansible" do |ansible|
                    #ansible.extra_vars = { type: "worker" }
                    #ansible.become_user = "root"
                    #ansible.compatibility_mode = "2.0"
                    #ansible.limit = opts[:name]
                    #ansible.playbook = "build-k8s-cluster.yml"
                #end
            #end
        end
    end
end 

