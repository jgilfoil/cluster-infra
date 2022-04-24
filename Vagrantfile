# -*- mode: ruby -*-
# vi: set ft=ruby :

# All Vagrant configuration is done below. The "2" in Vagrant.configure
# configures the configuration version (we support older styles for
# backwards compatibility). Please don't change it unless you know what
# you're doing.


Vagrant.configure("2") do |config|

    config.vm.define "control", primary: true do |config|
      config.vm.box = "ubuntu/focal64"
      config.vm.hostname = "control"
      config.vm.synced_folder ".", "/vagrant"
      config.vm.synced_folder ".", "/code/cluster-infra"
      config.vm.synced_folder "../k8s-gitops", "/code/k8s-gitops"

      config.vm.provider "virtualbox" do |vb|
        vb.name = "control"
        vb.memory = "1024"
        vb.cpus = "2"
        vb.gui = false
      end
  
      config.vm.network :public_network

      config.vm.provision "file", source: "c:\\users\\jason\\.ssh\\id_rsa", destination: "/home/vagrant/.ssh/id_rsa"
  
      config.vm.provision "shell", inline: <<-SHELL
        echo "setup environment============================================"
        grep -qxF 'cd /code/cluster-infra' /home/vagrant/.profile || echo 'cd /code/cluster-infra' >> /home/vagrant/.profile
        grep -qxF 'alias k=kubectl' /home/vagrant/.profile || echo 'alias k=kubectl' >> /home/vagrant/.profile
        grep -qxF 'export PATH="${PATH}:${HOME}/.krew/bin"' || echo 'export PATH="${PATH}:${HOME}/.krew/bin"' >> /home/vagrant/.profile
        grep -qxF 'export ANSIBLE_CONFIG=/code/cluster-infra/ansible.cfg' /home/vagrant/.profile || echo 'export ANSIBLE_CONFIG=/code/cluster-infra/ansible.cfg' >> /home/vagrant/.profile
        echo "fix ssh key perms============================================"
        chmod 0600 /home/vagrant/.ssh/id_rsa
        echo "update default python from 2 to 3"
        update-alternatives --install /usr/bin/python python /usr/bin/python3 1    
      SHELL

      config.vm.provision "ansible_local" do |ansible|
        ansible.playbook = "provision_control_vm.yml"
        ansible.install_mode = "pip_args_only"
        ansible.pip_install_cmd = "sudo apt-get install -y python3-pip python-is-python3 haveged && sudo ln -s -f /usr/bin/pip3 /usr/bin/pip"
        ansible.pip_args = "ansible==4.3.0"
      end


    end

end