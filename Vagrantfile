# -*- mode: ruby -*-
# vim: set ft=ruby :

Vagrant.configure("2") do |config|
  config.vm.define "server" do |server|
  server.vm.box = "centos/7"
  server.vm.box_version = "1804.02"
  server.vm.ignore_box_vagrantfile = true
  server.vm.synced_folder ".", "/vagrant", disabled: true
  server.vm.hostname = "server"
  server.vm.network "private_network", ip: "192.168.1.10"
server.vm.provider "virtualbox" do |vb|
    vb.cpus = 1
  #  vb.gui = true
    vb.memory = "512"
    vb.name = "server"
  end
   server.vm.provision "shell", inline: <<-SHELL
sudo su
 systemctl start nfs-server
 systemctl start rpcbind
 mkdir /var/upload
 touch /var/upload/Roman
 chmod o+rw /var/upload
 echo "/var/upload *(rw)" >> /etc/exports
 exportfs -var
 mkdir /var/upload1
 touch /var/upload1/ROman
 chmod o+rw /var/upload1
 echo "/var/upload1 *(rw)" >> /etc/exports
 exportfs -var
 systemctl start firewalld.service
 systemctl restart nfs-server
SHELL
end
config.vm.define "client" do |client|
  client.vm.box = "centos/7"
  client.vm.box_version = "1804.02"
  client.vm.ignore_box_vagrantfile = true
  client.vm.hostname = "client"
  client.vm.synced_folder ".", "/vagrant", disabled: true
  client.vm.network "private_network", ip: "192.168.1.11"
  client.vm.network "forwarded_port", guest: 22, host: 2200
  client.vm.provider "virtualbox" do |vb1|
      vb1.cpus = 1
   #   vb1.gui = true
      vb1.memory = "512"
      vb1.name = "client"
  end
 client.vm.provision "shell", inline: <<-SHELL
sudo su
  mkdir /mnt/upload
  mount -t nfs 192.168.1.10:/var/upload /mnt/upload
 touch /mnt/upload/Bendin
 mkdir /mnt/upload1
 mount -t nfs 192.168.1.10:/var/upload1 /mnt/upload1 -o udp
 touch /mnt/upload1/BEndin
                        SHELL


end

end
