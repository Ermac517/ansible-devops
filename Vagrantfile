Vagrant.configure("2") do |config|
  config.vm.box = "generic/rocky9"

  # Use a stable private IP on the libvirt private network so both Vagrant
  # provisioning and manual Ansible runs target the same reachable address.
  config.vm.network "private_network", ip: "172.28.128.10"

  config.vm.provider :libvirt do |lv|
    lv.cpus = 2
    lv.memory = 2048
  end

  config.ssh.insert_key = false
  config.vm.boot_timeout = 600
  config.vm.provision "ansible" do |ansible|
    ansible.playbook = "playbook.yml"
    ansible.inventory_path = "hosts.ini"
  end
end
