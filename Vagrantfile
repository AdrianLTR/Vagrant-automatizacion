Vagrant.configure("2") do |config|

    # Servidor Apache 1
    config.vm.define "apache1" do |apache1|
      apache1.vm.box = "ubuntu/bionic64"
      apache1.vm.network "private_network", ip: "192.168.50.4"
      apache1.vm.provider "virtualbox" do |vb|
        vb.memory = "512"
        vb.cpus = 1
      end
      apache1.vm.provision "shell", inline: <<-SHELL
        sudo apt-get update
        sudo apt-get install -y apache2
        echo "<h1>Apache Server 1</h1>" | sudo tee /var/www/html/index.html
      SHELL
    end
  
    # Servidor Apache 2
    config.vm.define "apache2" do |apache2|
      apache2.vm.box = "ubuntu/bionic64"
      apache2.vm.network "private_network", ip: "192.168.50.5"
      apache2.vm.provider "virtualbox" do |vb|
        vb.memory = "512"
        vb.cpus = 1
      end
      apache2.vm.provision "shell", inline: <<-SHELL
        sudo apt-get update
        sudo apt-get install -y apache2
        echo "<h1>Apache Server 2</h1>" | sudo tee /var/www/html/index.html
      SHELL
    end
  
    # Servidor NGINX con balanceo de carga
    config.vm.define "nginx" do |nginx|
      nginx.vm.box = "ubuntu/bionic64"
      nginx.vm.network "private_network", ip: "192.168.50.6"
      nginx.vm.provider "virtualbox" do |vb|
        vb.memory = "512"
        vb.cpus = 1
      end
      nginx.vm.provision "shell", inline: <<-SHELL
        sudo apt-get update
        sudo apt-get install -y nginx
        sudo tee /etc/nginx/conf.d/load_balancer.conf <<EOF
        upstream backend {
          server 192.168.50.4;
          server 192.168.50.5;
        }
        server {
          listen 80;
          location / {
            proxy_pass http://backend;
          }
        }
        EOF
        sudo nginx -t
        sudo systemctl restart nginx
      SHELL
    end
  
  end