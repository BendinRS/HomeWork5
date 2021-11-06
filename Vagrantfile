# -*- mode: ruby -*-
# vim: set ft=ruby :

Vagrant.configure("2") do |config|
  config.vm.define "server" do |server|
  server.vm.box = "centos/7"
  server.vm.box_version = "1804.02"
  server.vm.ignore_box_vagrantfile = true
  server.vm.synced_folder ".", "/vagrant", disabled: true
  server.vm.hostname = "server"
  server.vm.network "private_network", ip: "192.168.1.10", netmask: "24", virtual_box_intnet: "mynetwork"
server.vm.provider "virtualbox" do |vb|
    vb.cpus = 1
    vb.gui = true
    vb.memory = "512"
    vb.name = "server"
  end
#  server.ssh.username = "vagrant"
 # server.ssh.password = "vagrant"
#  server.vm.provision "shell", inline: <<-SHELL
#  vagrant plugin update vagrant-vbguest
#  SHELL
#  server.vm.provision "shell", inline: <<-SHELL
#				mkdir -p ~root/.ssh
#				cp ~vagrant/.ssh/auth* ~root/.ssh
#			SHELL
 # server.vm.provision "file", source: "nfs_server.sh", destination: "/home/vagrant/nfs_server.sh"
 # server.vm.provision "shell", inline: "/home/nfs_server.sh"
   server.vm.provision "shell", inline: <<-SHELL
 systemctl start nfs-server
 systemctl start rpcbind
 mkdir /var/upload
 chmod o+rw /var/upload
 echo "/var/upload *(rw)" >> /etc/exports
 exportfs -a
 systemctl restart nfs-server
 touch file111 /var/upload/
 touch file222 /var/upload/
 touch file333 /var/upload/
SHELL
end
config.vm.define "client" do |client|
  client.vm.box = "centos/7"
  client.vm.box_version = "1804.02"
  client.vm.ignore_box_vagrantfile = true
  client.vm.hostname = "client"
  client.vm.synced_folder ".", "/vagrant", disabled: true
  client.vm.network "private_network", ip: "192.168.1.11", netmask: "24", virtual_box_intnet: "mynetwork"
  client.vm.network "forwarded_port", guest: 22, host: 2200
  client.vm.provider "virtualbox" do |vb1|
      vb1.cpus = 1
      vb1.gui = true
      vb1.memory = "512"
      vb1.name = "client"
  end
#  client.ssh.username = "vagrant"
#  client.ssh.password = "vagrant"

#  client.vm.provision "shell", inline: <<-SHELL
#  vagrant plugin update vagrant-vbguest
#  SHELL
 client.vm.provision "shell", inline: <<-SHELL
  mkdir /mnt/upload
  mount -t nfs 192.168.1.10:/var/upload /mnt/upload

                        SHELL


 # client.vm.provision "file", source: "nfs_client.sh", destination: "/home/vagrant/nfs_client.sh"
 # client.vm.provision "shell", inline: "/home/nfs_client.sh"
end
  #  mkdir /mnt/upload
 #  mount -t nfs 192.168.1.1:/var/upload /mnt/upload
end
