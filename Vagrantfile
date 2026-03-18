Vagrant.configure("2") do |config|
  # Usar una imagen base de Debian 13 (Trixie)
  config.vm.box = "debian/trixie64"

  # Configuración de red: Mapear puerto 8080 de la VM al 8080 del anfitrión
  config.vm.network "forwarded_port", guest: 8080, host: 8080

  # Aprovisionamiento: Instalar Apache, Git y configurar puerto
  config.vm.provision "shell", inline: <<-SHELL
    apt-get update
    apt-get install -y apache2 git

    # Cambiar el puerto de Apache a 8080
    sed -i 's/Listen 80/Listen 8080/' /etc/apache2/ports.conf
    sed -i 's/\*:80/\*:8080/' /etc/apache2/sites-available/000-default.conf

    # Clonar un proyecto de ejemplo (reemplaza con tu repo)
    rm -rf /var/www/html/*
    git clone https://github.com/PabloCarrai/html.git /var/www/html/proyecto
    
    # Reiniciar Apache para aplicar cambios
    systemctl restart apache2
  SHELL
end
