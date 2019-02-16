# -*- mode: ruby -*-
# vi: set ft=ruby :

require 'yaml'
servers = YAML.load_file(File.join(File.dirname(__FILE__), 'servers.yml'))

servers.each do |servers|

  Vagrant.configure(2) do |config|
  
      # Define the nodes configuration doing a iteration 
    config.vm.define servers["name"] do |srv|

          # Config the VM
        srv.vm.box = servers["box"]
        srv.vm.hostname = servers["name"]
        srv.vm.network "private_network", ip: servers["ip"]
        srv.vm.network "forwarded_port", guest: 22, host: 2222, id: "ssh", disabled: true
        srv.vm.network "forwarded_port", guest: 22, host: servers["ssh_port"], auto_correct: true
        
        if servers["provider"] == "virtualbox"
            srv.vm.provider "virtualbox" do |vb|
                vb.memory = servers["ram"]
                vb.cpus = servers["vcpus"]
            end
        elsif servers["provider"] == "vmware"
            srv.vm.provider "vmware_desktop" do |vmw|
                vmw.vmx["memsize"] = servers["ram"]
                vmw.vmx["numvcpus"] = servers["vcpus"]
            end
        elsif servers["provider"] == "libvirt"
            srv.vm.provider "libvirt" do |virt|
                virt.memory = servers["ram"]
                virt.cpus = servers["vcpus"]
            end
        end
    end
  end
end