Vagrant.configure("2") do |config|
  # Cambiamos a una box de Debian 13 muy estable
  config.vm.box = "alvistack/debian-13"

  # Aumentamos el tiempo de espera para el arranque
  config.vm.boot_timeout = 600 

  # Redirección de puertos
  config.vm.network "forwarded_port", guest: 8080, host: 8080, auto_correct: true

  config.vm.provider "virtualbox" do |vb|
    vb.memory = "1024"
    vb.cpus = 1
    # ACTIVAMOS LA GUI: Si falla el arranque, verás la pantalla de la VM
    vb.gui = true
    # Solución común para errores de red en VirtualBox
    vb.customize ["modifyvm", :id, "--natdnshostresolver1", "on"]
    vb.customize ["modifyvm", :id, "--graphicscontroller", "vmsvga"]
  end

  config.vm.provision "shell", inline: <<-SHELL
    export DEBIAN_FRONTEND=noninteractive
    apt-get update
    apt-get install -y apache2 git

    # Configurar puerto 8080
    sed -i 's/Listen 80/Listen 8080/' /etc/apache2/ports.conf
    sed -i 's/<VirtualHost \*:80>/<VirtualHost *:8080>/' /etc/apache2/sites-available/000-default.conf

    # Clonar proyecto de Git
    rm -rf /var/www/html/*
    git clone https://github.com/PabloCarrai/html.git /var/www/html/

    systemctl restart apache2
  SHELL
end
