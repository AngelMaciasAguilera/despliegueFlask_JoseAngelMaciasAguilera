Vagrant.configure("2") do |config|
  config.vm.box = "debian/bullseye64"

  config.vm.define "nginx_machine" do |nginx_machine|
    # Configuración de red
    nginx_machine.vm.network "forwarded_port", guest: 80, host: 8080
    nginx_machine.vm.network "forwarded_port", guest: 5000, host: 5000
    nginx_machine.vm.network "private_network", ip: "192.168.56.10"

    # Actualización de la máquina
    nginx_machine.vm.provision "shell", name: "general_provisions", inline: <<-SHELL
      sudo apt-get -y update
      sudo apt-get install -y python3-pip
      sudo mkdir -p /var/www/app
      sudo chown -R vagrant:www-data /var/www/app
      sudo chmod -R 775 /var/www/app
      sudo cp -vr /vagrant/app_provision/.env /var/www/app
      sudo cp -vr /vagrant/app_provision/application.py /var/www/app
      sudo cp -vr /vagrant/app_provision/wsgi.py /var/www/app
      sudo cp -vr /vagrant/app_provision/flask_app.service  /etc/systemd/system/
    SHELL

    # Instalación de NGINX
    nginx_machine.vm.provision "shell", name: "nginx_installation", inline: <<-SHELL
      sudo apt-get -y install nginx
      sudo rm /etc/nginx/sites-available/default
      sudo rm /etc/nginx/sites-enabled/default
      sudo systemctl restart nginx
    SHELL

    nginx_machine.vm.provision "shell", privileged:false , inline: <<-SHELL
      pip3 install pipenv
      PATH=$PATH:/home/vagrant/.local/bin
      pip3 install python-dotenv
      cd /var/www/app
      pipenv install flask gunicorn
    SHELL
  end
end
