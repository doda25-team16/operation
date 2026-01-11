# model-service/Vagrantfile

require "fileutils"
NUMBER_OF_WORKERS = 2

# HOST Configuration
# Automatically create a shared portable folder if does not exist
local_shared_path = File.join(File.dirname(__FILE__), "shared")
unless File.directory?(local_shared_path)
  puts "./shared directory does not exist. Creating one under: #{local_shared_path}"
  FileUtils.mkdir(local_shared_path)
end

# NODES Configuration
Vagrant.configure("2") do |config|
  hosts_list = []

  hosts_list << {"VM_name" => "ctrl", "ip" => "192.168.56.110"}
  (1..NUMBER_OF_WORKERS).each do |i|
    hosts_list << {"VM_name" => "node-#{i}", "ip" => "192.168.56.#{110 + i}"}
  end

  # Default Nodes
  config.vm.box = "bento/ubuntu-24.04"
  config.vm.provider "virtualbox" do |vb|
    vb.gui = false
  end


  # Shared folder for A3 Excellent
  config.vm.synced_folder local_shared_path, "/mnt/shared",
    type: "virtualbox",
    mount_options: ["dmode=777", "fmode=777"]

  # Control Node
  config.vm.define "ctrl" do |ctrl|
    ctrl.vm.hostname = "ctrl"
    # "host-only" interface (NIC)
    ctrl.vm.network "private_network", ip: "192.168.56.110"
    
    ctrl.vm.provider "virtualbox" do |wvb| # wvb = worker virtual box
      wvb.memory = 4096
      wvb.cpus = 1
    end

    # After VM initialization, set-up General Node
    ctrl.vm.provision :ansible do |ctrl_ansible|
      ctrl_ansible.compatibility_mode = "2.0"
      ctrl_ansible.playbook = "ansible/general.yaml"
      # Generate a reusable Ansible inventory with all active nodes
      ctrl_ansible.inventory_path = "ansible/inventory.cfg"
      ctrl_ansible.host_key_checking = false
      # Add groups so the generated inventory has [control] and [node]
      ctrl_ansible.groups = {
        "control" => ["ctrl"],
        "node" => hosts_list.select { |h| h["VM_name"] != "ctrl" }.map { |h| h["VM_name"] }
      }
      ctrl_ansible.extra_vars = { hosts_list: hosts_list }
    end

    # Install Ansible inside only the control node for extra options
    ctrl.vm.provision "shell", inline: <<-SHELL
      sudo apt-get update -y
      sudo apt-get install -y software-properties-common
      sudo add-apt-repository --yes --update ppa:ansible/ansible
      sudo apt-get install -y ansible
    SHELL

    # # Control Node specific playbook
    ctrl.vm.provision :ansible do |ctrl_ansible|
      ctrl_ansible.compatibility_mode = "2.0"
      ctrl_ansible.playbook = "ansible/ctrl.yaml"
    end
  end

  # Worker Nodes
  (1..NUMBER_OF_WORKERS).each do |i|
    config.vm.define "node-#{i}" do |node|
      node.vm.hostname = "node-#{i}"
      # "host-only" interface (NIC)
      node.vm.network "private_network", ip: "192.168.56.#{110 + i}" # ip is indexed by worker#
      
      node.vm.provider "virtualbox" do |nvb| # nvb = worker virtual box
        nvb.memory = 6144
        nvb.cpus = 2
      end
      # After VM initialization, set-up General Node
      node.vm.provision :ansible do |node_ansible|
        node_ansible.compatibility_mode = "2.0"
        node_ansible.playbook = "ansible/general.yaml"
        node_ansible.extra_vars = { hosts_list: hosts_list }
      end

      # Worker Node specific playbook
      node.vm.provision :ansible do |node_ansible|
        node_ansible.compatibility_mode = "2.0"
        node_ansible.playbook = "ansible/node.yaml"
      end
    end
  end
end
