# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure("2") do |config|
  config.vm.box = "debian/bookworm64"
  config.vm.network "forwarded_port", guest: 80, host: 8081
  config.vm.network "private_network", ip: "192.168.0.2"
  #config.vm.synced_folder "html_docs", "/var/www/html_docs"
  config.vm.provision "shell", inline: <<-SHELL
    apt-get update
    apt-get install -y nginx git
    mkdir -p /var/www/nginx_server/html
    cd /var/www/nginx_server/html
    git clone https://github.com/cloudacademy/static-website-example
    chown -R www-data:www-data /var/www/nginx_server/html
    chmod -R 755 /var/www/nginx_server
    cp -v /vagrant/nginx_server /etc/nginx/sites-available/
    ln -s /etc/nginx/sites-available/nginx_server /etc/nginx/sites-enabled
    systemctl restart nginx
  SHELL
  config.vm.provision "shell", inline: <<-SHELL
    apt-get update
    apt-get install -y vsftpd
    mkdir /home/vagrant/ftp
    openssl req -x509 -nodes -days 365 -newkey rsa:2048 -keyout /etc/ssl/private/vsftpd.key -out /etc/ssl/certs/vsftpd.crt
    cp -v /vagrant/vsftpd.conf /etc/
    systemctl restart vsftpd
  SHELL
end
