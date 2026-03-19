Vagrant.configure("2") do |config|
  config.vm.box = "generic/debian13"

  config.vm.network "forwarded_port", guest: 8080, host: 8080, auto_correct: true

  config.vm.provider "virtualbox" do |vb|
    vb.gui = false # Cambiamos a false para evitar errores de ventana
    vb.memory = "1024"
    # Forzamos configuraciones de estabilidad
    vb.customize ["modifyvm", :id, "--hwvirtex", "on"]
    vb.customize ["modifyvm", :id, "--audio", "none"]
    vb.customize ["modifyvm", :id, "--graphicscontroller", "vboxvga"]
  end

  config.vm.provision "shell", inline: <<-SHELL
    apt-get update
    apt-get install -y apache2 git
    sed -i 's/Listen 80/Listen 8080/' /etc/apache2/ports.conf
    sed -i 's/<VirtualHost \*:80>/<VirtualHost *:8080>/' /etc/apache2/sites-available/000-default.conf
    rm -rf /var/www/html/*
    git clone https://github.com/PabloCarrai/html.git /var/www/html/
    systemctl restart apache2
  SHELL
end
