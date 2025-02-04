# -*- mode: ruby -*-
# vi: set ft=ruby :

# All Vagrant configuration is done below. The "2" in Vagrant.configure
# configures the configuration version (we support older styles for
# backwards compatibility). Please don't change it unless you know what
# you're doing.
Vagrant.configure("2") do |config|
  config.vm.box = "debian/bullseye64"

  config.vm.provision "shell", name: "general_provisions", inline: <<-SHELL
      sudo apt-get -y update
  SHELL

  config.vm.define "nginx_machine" do |nginx_machine| 
    nginx_machine.vm.network "forwarded_port", guest: 80, host: 8080
    nginx_machine.vm.network "forwarded_port", guest: 5000, host: 5000  
    nginx_machine.vm.network "private_network", ip: "192.168.56.10"
    nginx_machine.vm.provision "shell", name: "nginx_installation", inline: <<-SHELL
      sudo apt -y install nginx
      sudo rm /etc/nginx/sites-available/default
      sudo rm /etc/nginx/sites-enabled/default
      sudo systemctl restart nginx
    SHELL
  end 

  config.vm.provision "shell", name: "python_provision", inline: <<-SHELL
    sudo apt-get install -y python3-pip
    pip3 install pipenv
    
    PATH=$PATH:/home/vagrant/.local/bin
    pip3 install python-dotenv
  SHELL

  config.vm.provision "shell", name: "app_deploy", inline: <<-SHELL
    sudo mkdir -p /var/www/app
    sudo chown -R vagrant:www-data /var/www/app
    sudo chmod -R 775 /var/www/app
    sudo cp -vr /vagrant/app_provision/.env  /var/www/app   
    cd /var/www/app
    pipenv shell
    pipenv install flask gunicorn
    sudo cp -vr /vagrant/app_provision/application.py  /var/www/app  
    sudo cp -vr /vagrant/app_provision/wsgi.py /var/www/app    
    sudo cp -vr /vagrant/app_provision/flask_app.service  /etc/systemd/system  
    sudo systemctl daemon-reload
    sudo systemctl enable flask_app
    sudo systemctl start flask_app
    sudo cp -vr /vagrant/app_provision/app.conf   /etc/nginx/sites-available/
    sudo ln -s /etc/nginx/sites-available/app.conf /etc/nginx/sites-enabled/
    sudo systemctl restart nginx
  SHELL
end