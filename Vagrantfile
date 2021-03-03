Vagrant.configure("2") do |config|

  config.vm.define "Controlador" do |controlador|
      controlador.vm.provider "virtualbox" do |vb|
          vb.name = "Controlador"
          vb.cpus = 2
          vb.memory = 2048
      end

      controlador.vbguest.auto_update = false
      controlador.nfs.verify_installed = false
      controlador.vm.synced_folder '.', '/vagrant', disabled: true  
      controlador.vm.network "private_network", ip: "192.168.50.2"
      controlador.vm.provider :virtualbox
      controlador.vm.box = "generic/debian10"
      controlador.vm.hostname = "Controlador"
  end

  config.vm.define "Worker1" do |worker1|

      worker1.vm.provider "virtualbox" do |vb|
          vb.name = "Worker1"
          vb.cpus = 2
          vb.memory = 2048
      end

	  worker1.vbguest.auto_update = false
	  worker1.nfs.verify_installed = false
	  worker1.vm.synced_folder '.', '/vagrant', disabled: true
      worker1.vm.network "private_network", ip: "192.168.50.3"
      worker1.vm.provider :virtualbox
      worker1.vm.box = "generic/debian10"
      worker1.vm.hostname = "Worker1"

  end

  config.vm.define "Worker2" do |worker2|
      
      worker2.vm.provider "virtualbox" do |vb|
          vb.name = "Worker2"
          vb.cpus = 2
          vb.memory = 2048
      end

      worker2.vbguest.auto_update = false
      worker2.nfs.verify_installed = false
      worker2.vm.synced_folder '.', '/vagrant', disabled: true
      worker2.vm.network "private_network", ip: "192.168.50.4"
      worker2.vm.provider :virtualbox
      worker2.vm.box = "generic/debian10"
      worker2.vm.hostname = "Worker2"

  end
end
