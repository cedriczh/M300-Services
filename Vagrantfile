# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure("2") do |config|

  config.vm.box = "ubuntu/bionic64"

  config.vm.network "forwarded_port", guest: 80, host: 8080

  config.vm.synced_folder ".", "/var/www/html"

     # Customize the amount of memory on the VM:
     vb.memory = "2048"
   end

  config.vm.provision "shell", inline: <<-SHELL
    apt-get update
    apt-get install -y apache2 mysql-server phpmyadmin
    service apache2 restart
    
  SHELL
end
