Vagrant.configure("2") do |config|
  config.vm.box = "generic/rocky8"

  # DHCP on libvirt network
  config.vm.network "private_network", type: "dhcp"

  config.vm.provider :libvirt do |lv|
    lv.cpus = 2
    lv.memory = 2048
  end

  config.ssh.insert_key = false
  config.vm.boot_timeout = 600
end
