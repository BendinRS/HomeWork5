# HomeWork5

## Создаем собственный вагрантфайл

+ Задаем параметры первой виртуальной машины
```
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
  ```
  + Пишем скрипт
  ```
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
 systemctl restart nfs-server
SHELL
end
```
+ Задаем параметры второй виртуальной машины
```
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
  ```
  + Пишем скрипт. Присоединяем две разные папки по протоколам tcp и udp
  ```
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
```
## Результаты работы
```
root@RomanUbuntu:/home/roman/otus/nfs# vagrant ssh client
Last login: Sat Nov  6 12:41:33 2021 from 10.0.2.2
[vagrant@client ~]$ cd /mnt/ 
[vagrant@client mnt]$ ls
upload  upload1
[vagrant@client mnt]$ ls /mnt/upload
Bendin  Roman
[vagrant@client mnt]$ ls /mnt/upload1
BEndin  ROman
[vagrant@client mnt]$ 
```
```
root@RomanUbuntu:/home/roman/otus/nfs# vagrant ssh server
[vagrant@server ~]$ cd /var/
[vagrant@server var]$ ls
adm  cache  db  empty  games  gopher  kerberos  lib  local  lock  log  mail  nis  opt  preserve  run  spool  tmp  upload  upload1  yp
[vagrant@server var]$ ls /var/upload
Bendin  Roman
[vagrant@server var]$ ls /var/upload1
BEndin  ROman
[vagrant@server var]$ 
```
