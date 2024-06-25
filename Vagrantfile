# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure("2") do |config|

  config.vm.provision "file", source: "keys/examid.pub", destination: "/home/vagrant/.ssh/"

  config.vm.define "controlnode" do |controlnode|
    controlnode.vm.box = "ubuntu/focal64"
    controlnode.vm.hostname = "controlnode"
    controlnode.vm.network "private_network", ip: "192.168.56.4"
    controlnode.vm.synced_folder "./ansible","/home/vagrant/ansible"
    controlnode.vm.provision "file", source: "keys/examid", destination: "/home/vagrant/.ssh/"
    controlnode.vm.provision "shell", inline: <<-SHELL
      sed -i 's/ChallengeResponseAuthentication no/ChallengeResponseAuthentication yes/#g' /etc/ssh/sshd_config
      service ssh restart
      sudo apt update -y && sudo apt-add-repository ppa:ansible/ansible && sudo apt -y install sshpass ansible
      chmod 600 /home/vagrant/.ssh/examid
      chmod 644 /home/vagrant/.ssh/examid.pub
    SHELL
  end

  config.vm.define "server" do |server|
    server.vm.box = "ubuntu/focal64"
    server.vm.hostname = "server"
    server.vm.network "private_network", ip: "192.168.56.10"
    server.vm.provision "shell", inline: <<-SHELL
      chmod 644 /home/vagrant/.ssh/examid.pub
      cat /home/vagrant/.ssh/examid.pub >> /home/vagrant/.ssh/authorized_keys
    SHELL
  end
end
