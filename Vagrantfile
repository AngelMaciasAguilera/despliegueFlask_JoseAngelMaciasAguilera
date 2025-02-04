# -*- mode: ruby -*-
# vi: set ft=ruby :

# All Vagrant configuration is done below. The "2" in Vagrant.configure
# configures the configuration version (we support older styles for
# backwards compatibility). Please don't change it unless you know what
# you're doing.
Vagrant.configure("2") do |config|
  config.vm.box = "debian/bullseye64"

  config.vm.provision "shell", name: "general_provisions", inline: <<-SHELL
      sudo apt-get update
  SHELL

  
  
  config.vm.define "nginx_machine" do |nginx_machine| 
    nginx_machine.vm.network "forwarded_port", guest: 80, host: 8080
    nginx_machine.vm.network "forwarded_port", guest: 5000, host: 5000  
    nginx_machine.vm.network "private_network", ip: "192.168.56.10"
    nginx_machine.vm.provision "shell", name: "nginx_installation", inline: <<-SHELL
      apt -y install nginx
      mkdir -p /var/www/nginx_page/html
      chown -R www-data:www-data /var/www/nginx_page/html
       cp -vr /vagrant/python-flask-gunicorn-html/* /var/www/nginx_page/html
       chmod -R 755 /var/www/nginx_page
       systemctl restart nginx
       cp -v /vagrant/nginx_provisions/nginx-page /etc/nginx/sites-available/
       ln -s /etc/nginx/sites-available/nginx-page /etc/nginx/sites-enabled/
       rm /etc/nginx/sites-available/default
       rm /etc/nginx/sites-enabled/default
       systemctl restart nginx
    SHELL
  end 
end