# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure("2") do |config|
  config.vm.box = "debian/bookworm64"
  config.vm.network "forwarded_port", guest: 80, host: 8081
  config.vm.network "private_network", ip: "192.168.0.2"
  config.vm.provision "shell", name: "nginx", inline: <<-SHELL
    apt-get update
    apt-get install -y nginx git
    mkdir -p /var/www/nginx_server/html
    cd /var/www/nginx_server/html
    git clone https://github.com/cloudacademy/static-website-example
    chown -R www-data:www-data /var/www/nginx_server/html
    chmod -R 755 /var/www/nginx_server
    cp -v /vagrant/nginx_server /etc/nginx/sites-available/
    ln -s /etc/nginx/sites-available/nginx_server /etc/nginx/sites-enabled
    rm /etc/nginx/sites-enabled/default
    systemctl restart nginx
  SHELL
  config.vm.provision "shell", name: "set_password", inline: <<-SHELL
    echo "vagrant:vagrant" | sudo chpasswd
  SHELL
  config.vm.provision "shell", name: "vsftpd", inline: <<-SHELL
    mkdir /home/vagrant/ftp
    chown vagrant:vagrant /home/vagrant/ftp
    chmod -R 755 /home/vagrant/ftp
    apt-get update
    apt-get install -y vsftpd
    openssl req -x509 -nodes -days 365 -newkey rsa:2048 -keyout /etc/ssl/private/vsftpd.key -out /etc/ssl/certs/vsftpd.crt -subj "/C=ES/ST=./L=./O=./OU=./CN=nginx_server/emailAddress=."
    cp -v /vagrant/vsftpd.conf /etc/
    chmod 755 /etc/ssl/private
    systemctl restart vsftpd
  SHELL
  # Una vez se haya transferido el contenido del sitio web al servidor mediante FTPES, se podrán hacer las siguientes provisiones:
  config.vm.provision "shell", name: "foo_fighters", run: "never", inline: <<-SHELL
    rm /etc/nginx/sites-enabled/*
    mkdir -p /var/www/foo_fighters/html
    cp -r /home/vagrant/ftp/foo_fighters /var/www/foo_fighters/html
    chown -R www-data:www-data /var/www/foo_fighters/html
    chmod -R 755 /var/www/foo_fighters
    cp -v /vagrant/foo_fighters /etc/nginx/sites-available/
    ln -s /etc/nginx/sites-available/foo_fighters /etc/nginx/sites-enabled/
    systemctl restart nginx
  SHELL
  config.vm.provision "shell", name: "perfect_learn", run: "never", inline: <<-SHELL
    rm /etc/nginx/sites-enabled/*
    mkdir -p /var/www/perfect_learn/html
    cp -r /home/vagrant/ftp/perfect_learn /var/www/perfect_learn/html
    chown -R www-data:www-data /var/www/perfect_learn/html
    chmod -R 755 /var/www/perfect_learn
    cp -v /vagrant/perfect_learn /etc/nginx/sites-available/
    ln -s /etc/nginx/sites-available/perfect_learn /etc/nginx/sites-enabled/
    systemctl restart nginx
  SHELL
  config.vm.provision "shell", name: "example", run: "never", inline: <<-SHELL
    rm /etc/nginx/sites-enabled/*
    mkdir -p /var/www/example.com/html
    cp -r /home/vagrant/ftp/example/* /var/www/example.com/html
    chown -R www-data:www-data /var/www/example.com/html
    chmod -R 755 /var/www/example.com
    cp -v /vagrant/example.com /etc/nginx/sites-available/
    ln -s /etc/nginx/sites-available/example.com /etc/nginx/sites-enabled/
    systemctl restart nginx
  SHELL
  config.vm.provision "shell", name: "ufw", run: "never", inline: <<-SHELL
    apt install ufw
    ufw allow ssh
    ufw allow 'Nginx Full'
    ufw delete allow 'Nginx HTTP'
    ufw --force enable
  SHELL
  config.vm.provision "shell", name: "example_openssl", run: "never", inline: <<-SHELL
    openssl req -x509 -nodes -days 365 -newkey rsa:2048 -keyout /etc/ssl/private/example.com.key -out /etc/ssl/certs/example.com.crt -subj "/C=ES/ST=Andalucía/L=Granada/O=IZV/OU=Despliegue/CN=example.com/emailAddress=webmaster@example.com"
  SHELL
end
