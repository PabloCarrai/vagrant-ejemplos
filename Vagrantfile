Vagrant.configure("2") do |config|
  # Imagen de Debian 13 compatible con VirtualBox
  config.vm.box = "alvistack/debian-13"

  # Configuración de red: puerto 8080 en tu PC -> 8080 en la VM
  config.vm.network "forwarded_port", guest: 8080, host: 8080

  config.vm.provision "shell", inline: <<-SHELL
    # Actualizar e instalar Apache y Git
    apt-get update
    apt-get install -y apache2 git

    # Configurar Apache para escuchar en el puerto 8080
    sed -i 's/Listen 80/Listen 8080/' /etc/apache2/ports.conf
    sed -i 's/<VirtualHost \*:80>/<VirtualHost *:8080>/' /etc/apache2/sites-available/000-default.conf

    # Clonar un proyecto de Git al directorio de Apache
    rm -rf /var/www/html/*
    git clone https://github.com/PabloCarrai/html.git /var/www/html/     

    # Reiniciar Apache para aplicar los cambios
    systemctl restart apache2
  SHELL
end
