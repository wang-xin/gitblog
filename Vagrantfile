Vagrant.configure("2") do |config|
  config.vm.box = "ubuntu16.04"
  config.vm.network "public_network", ip: "192.168.50.150"
  config.vm.hostname = "vagrant-ubuntu-1604"
  config.vm.synced_folder "./", "/app"
  config.vm.provider "virtualbox" do |vb|
    vb.cpus = "1"
    vb.memory = "1024"
    vb.name = "www.i-king.top"
  end


  config.vm.provision "shell", inline: <<-SHELL
    apt-get update
  SHELL
end