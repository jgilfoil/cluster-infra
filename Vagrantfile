# -*- mode: ruby -*-
# vi: set ft=ruby :

# All Vagrant configuration is done below. The "2" in Vagrant.configure
# configures the configuration version (we support older styles for
# backwards compatibility). Please don't change it unless you know what
# you're doing.

ODROID01 = "192.168.1.200"
ODROID02 = "192.168.1.201"
ODROID03 = "192.168.1.202"

Vagrant.configure("2") do |config|

    config.vm.define "k3scontrol", primary: true do |config|
      config.vm.box = "ubuntu/focal64"
      config.vm.hostname = "k3scontrol"
      config.vm.synced_folder ".", "/vagrant"
      config.vm.synced_folder ".", "/code"

      config.vm.provider "virtualbox" do |vb|
        vb.name = "k3scontrol"
        vb.memory = "1024"
        vb.cpus = "2"
        vb.gui = false
      end
  
      config.vm.network :public_network

      config.vm.provision "file", source: "c:\\users\\jason\\.ssh\\id_rsa", destination: "/home/vagrant/.ssh/id_rsa"
  
      config.vm.provision "shell", inline: <<-SHELL
        apt update -y
        apt-get upgrade -y
        echo "install pip3========================================"
        sudo apt-get -y install python3-pip
        echo "install upgraded cryptography========================================"
        sudo  pip3 install --user --upgrade cryptography
        # echo "install helm v3======================================================"
        # sudo curl -s https://raw.githubusercontent.com/helm/helm/master/scripts/get-helm-3 -o get_helm.sh
        # sudo chmod 700 get_helm.sh
        # sudo ./get_helm.sh
        echo "install requirements========================================"
        sudo -H -u vagrant pip3 install --user -r /vagrant/requirements.txt
        grep -qxF 'cd /vagrant' /home/vagrant/.profile || echo 'cd /vagrant' >> /home/vagrant/.profile
        grep -qxF 'export ANSIBLE_CONFIG=/vagrant/ansible.cfg' /home/vagrant/.profile || echo 'export ANSIBLE_CONFIG=/vagrant/ansible.cfg' >> /home/vagrant/.profile
        echo "fix ssh key perms============================================"
        chmod 0600 /home/vagrant/.ssh/id_rsa
        
      SHELL

    end

end