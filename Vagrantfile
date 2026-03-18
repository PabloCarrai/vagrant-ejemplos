Vagrant.configure("2") do |config|
  config.vm.box = "alvistack/debian-13"

  # Usar una IP fija ayuda a evitar errores de colisión de puertos
  config.vm.network "private_network", ip: "192.168.56.10"
  
  # Reintento de mapeo de puertos
  config.vm.network "forwarded_port", guest: 8080, host: 8080, auto_correct: true

  config.vm.provider "virtualbox" do |vb|
    vb.memory = "1024"
    vb.cpus = 1
    # Esto desactiva el audio y otros periféricos que a veces dan error
    vb.customize ["modifyvm", :id, "--audio", "none"]
  end

  config.vm.provision "shell", inline: <<-SHELL
    apt-get update
    apt-get install -y apache2 git
    
    # Configurar puerto 8080
    sed -i 's/Listen 80/Listen 8080/' /etc/apache2/ports.conf
    sed -i 's/<VirtualHost \*:80>/<VirtualHost *:8080>/' /etc/apache2/sites-available/000-default.conf
    
    # Clonar repo
    rm -rf /var/www/html/*
    git clone https://github.com/PabloCarrai/html.git /var/www/html/
    
    systemctl restart apache2
  SHELL
end

