# -*- mode: ruby -*-
# vi: set ft=ruby :

require 'yaml'
servers = YAML.load_file(File.join(File.dirname(__FILE__), 'servers.yml'))


Vagrant.configure(2) do |config|
    
    config.hostmanager.enabled = false
    
    servers.each do |servers|
      # Define the nodes configuration doing a iteration 
        config.vm.define servers["name"] do |srv|

          # Config the VM
            srv.vm.box = servers["box"]
            srv.vm.hostname = servers["name"]
            srv.vm.network "private_network", ip: servers["ip"]
            srv.vm.network "forwarded_port", guest: 22, host: 2222, id: "ssh", disabled: true
            srv.vm.network "forwarded_port", guest: 22, host: servers["ssh_port"], auto_correct: true
            srv.vm.network "forwarded_port", guest: 8443, host: 8443, auto_correct: true
        
            srv.vm.provider "libvirt" do |virt|
                virt.memory = servers["ram"]
                virt.cpus = servers["vcpu"]
            end

            srv.vm.provider "virtualbox" do |vb|
                vb.memory = servers["ram"]
                vb.cpus = servers["vcpu"]
            end

            srv.vm.provider "vmware_desktop" do |vmw|
                vmw.vmx["memsize"] = servers["ram"]
                vmw.vmx["numvcpus"] = servers["vcpu"]
            end

            if servers['name'] == 'k8smaster'
                srv.vm.provision "ansible" do |ansible|
                  ansible.playbook = "ansible/playbook.yml"
                  ansible.limit = "all"
                  ansible.groups = {
                    "k8s-master" => ["k8smaster"],
                    "k8s-node"  => ["k8snode01", "k8snode02", "k8snode03"],
                    "k8s:children" => ["k8s-master", "k8s-node"]
                  }
                end
            end
        end
    end

    
end
