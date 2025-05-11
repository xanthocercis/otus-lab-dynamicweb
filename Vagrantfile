Vagrant.configure(2) do |config|
  config.vm.define "DynamicWeb" do |vmconfig|
    vmconfig.vm.box = 'generic/ubuntu2004'
    vmconfig.vm.hostname = 'DynamicWeb'
    vmconfig.vm.network "forwarded_port", guest: 8083, host: 8083
    vmconfig.vm.network "forwarded_port", guest: 8081, host: 8081
    vmconfig.vm.network "forwarded_port", guest: 8082, host: 8082
    vmconfig.vm.provider "virtualbox" do |vbx|
      vbx.memory = "2048"
      vbx.cpus = "2"
      vbx.customize ["modifyvm", :id, '--audio', 'none']
    end

    # Копируем проект и playbook в VM
    vmconfig.vm.synced_folder "project", "/home/vagrant/project"
    vmconfig.vm.synced_folder ".", "/home/vagrant/ansible"

    # Устанавливаем Ansible и зависимости, затем выполняем playbook локально
    vmconfig.vm.provision "shell" do |s|
      s.inline = <<-SHELL
        sudo apt-get update
        sudo apt-get install -y python3-pip
        sudo pip3 install ansible
        ansible-playbook /home/vagrant/ansible/prov.yml
      SHELL
    end
  end
end