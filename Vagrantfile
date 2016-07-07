# -*- mode: ruby -*-
# vi: set ft=ruby :


VAGRANTFILE_API_VERSION = "2"

Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|
  config.vm.box = "ubuntu/trusty64"
  config.vm.synced_folder "./shared_dir_host", "/shared_dir_guest"

  # https://github.com/dotless-de/vagrant-vbguest
  # $ vagrant plugin install vagrant-vbguest
  # set auto_update to false, if you do NOT want to check the correct additions version when booting this vm
  config.vbguest.auto_update = false
  # do NOT download the iso file from a webserver
  config.vbguest.no_remote = true 

  # ------ list with node details to create - start ------ #
  my_nodes = [
    {:hostName => 'box-zookeeper-1', :ip => '192.168.3.11', :ramGB => 1, :hdSizeGB => 45, :groups => ['zookeeper_hosts_grp'], :ports => [2181]},
    {:hostName => 'box-zookeeper-2', :ip => '192.168.3.12', :ramGB => 1, :hdSizeGB => 45, :groups => ['zookeeper_hosts_grp'], :ports => [2182]},
    {:hostName => 'box-kafka-1', :ip => '192.168.3.21', :ramGB => 1, :hdSizeGB => 50, :groups => ['kafka_hosts_grp'], :ports => [9092]},
    {:hostName => 'box-kafka-2', :ip => '192.168.3.22', :ramGB => 1, :hdSizeGB => 50, :groups => ['kafka_hosts_grp'], :ports => [9093]},
    {:hostName => 'box-hadoop-1', :ip => '192.168.3.31', :ramGB => 1, :hdSizeGB => 60, :groups => ['hadoop_hosts_grp', 'postgresql_hosts_grp'], :ports => [50070, 50075, 50090, 50105]},
    {:hostName => 'box-hadoop-2', :ip => '192.168.3.32', :ramGB => 1, :hdSizeGB => 60, :groups => ['hadoop_hosts_grp', 'postgresql_hosts_grp'], :ports => [50071, 50076, 50091, 50106]}
  ]
  # ------ list with node details to create - end ------ #

  my_groups = {"all_groups:children" => []}
  my_nodes.each do |node|  ## ------ creating group list - start ------ ###
    if node.has_key?(:groups) then
      node[:groups].each do |grp|
        if my_groups.has_key?(grp) then
          my_groups[grp] << node[:hostName].to_s
        else
          my_groups[grp] = [node[:hostName].to_s]
          my_groups["all_groups:children"] << grp
        end
      end
    end
  end  ## ------ creating group list - end ------ ###

  my_nodes.each_with_index do |itemNode, index|  ## --- loop my_nodes - start --- ###
    # install: $ vagrant plugin install vagrant-hostmanager
    # configuration of /etc/hosts for each node
    config.hostmanager.enabled = true
    #config.hostmanager.manage_host = true
    config.hostmanager.manage_host = false
    config.hostmanager.manage_guest = true
    config.hostmanager.ignore_private_ip = false
    config.hostmanager.include_offline = true

    config.vm.define itemNode[:hostName] do |machine|  ## --- machine definition - start --- ##

      machine.vm.hostname = itemNode[:hostName].to_s  # set hostname
      machine.vm.network "private_network", ip: itemNode[:ip]  # set ip address
      if itemNode.has_key?(:ports) then  # set ports
        itemNode[:ports].each do |my_port|     
          machine.vm.network "forwarded_port", guest: my_port, host: my_port
        end
      end

      # ----------------- customize nodes configuration - start ----------------- #
      #
      # this section convert vmdk to vdi, sets a vm name and resize hd just 1 time
      # if you want execute these steps for first time, check that 'path_to_vdi_file' doesn't exists
      # if so, then remove it: $ rm -r <HOME_PATH>/VirtualBox VMs/<VM_NAME>
      # 
      machine.vm.provider "virtualbox" do |vb|  
        vb.name = "#{machine.vm.hostname}"
        path_to_vdi_file = "#{ENV["HOME"]}/VirtualBox VMs/#{vb.name}/box-disk1.vdi"

        # check if VDI exists
        if ! File.exist?(path_to_vdi_file)
          puts " --> VM/VDI file (#{path_to_vdi_file}) doesn't exist."
          vb.customize ["modifyvm", :id, "--memory", itemNode[:ramGB] * 1024]
          vb.customize ["modifyvm", :id, "--cpus", "2"]
          vb.customize ["modifyvm", :id, "--ostype", "Ubuntu_64"]
          vb.customize ["modifyvm", :id, "--acpi", "on"]
          vb.customize ["modifyvm", :id, "--ioapic", "on"]
          vb.customize [
            "storagectl", :id, 
            "--name", "SATAController", 
            "--controller", "IntelAHCI", 
            "--portcount", "1", 
            "--hostiocache", "on"
          ]
          vb.customize [
            "clonehd", "#{ENV["HOME"]}/VirtualBox VMs/#{vb.name}/box-disk1.vmdk", 
                       "#{ENV["HOME"]}/VirtualBox VMs/#{vb.name}/box-disk1.vdi", 
            "--format", "VDI"
          ]
          # resizing, size by default is 40 GB = 40 * 1024
          vb.customize [
            "modifyhd", "#{ENV["HOME"]}/VirtualBox VMs/#{vb.name}/box-disk1.vdi",
            "--resize", itemNode[:hdSizeGB] * 1024
          ]
          vb.customize [
            "storageattach", :id, 
            "--storagectl", "SATAController", 
            "--port", "0", 
            "--device", "0", 
            "--type", "hdd",
            "--nonrotational", "on",
            "--medium", "#{ENV["HOME"]}/VirtualBox VMs/#{vb.name}/box-disk1.vdi" 
          ]
        else 
          puts " --> VM/VDI file (#{path_to_vdi_file}) has been customized."
        end

      end  
      # ----------------- customize nodes configuration - end ----------------- #


      # only execute once the Ansible provisioner, when all the machines are up and ready.
      if index == my_nodes.size - 1 then
        machine.vm.provision :ansible do |ansible|
          # disable default limit to connect to all the machines
          ansible.limit = "all"
          ansible.playbook = "playbook.yml"
          ansible.groups = my_groups
        end
      end
    end  ## --- machine definition - end --- ##


  end  ## --- loop my_nodes - end --- ###

end