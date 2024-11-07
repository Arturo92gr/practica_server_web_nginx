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
    sudo chmod 755 /etc/ssl/private
    sudo systemctl restart vsftpd
  SHELL
  # Una vez se haya transferido el contenido del sitio web al servidor mediante FTPES, se podrá hacer la siguiente provisión:
  config.vm.provision "shell", name: "foo_fighters", run: "never", inline: <<-SHELL
    sudo rm /etc/nginx/sites-enabled/nginx_server
    sudo mkdir -p /var/www/foo_fighters/html
    sudo cp -r /home/vagrant/ftp/* /var/www/foo_fighters/html
    sudo chown -R www-data:www-data /var/www/foo_fighters/html
    sudo chmod -R 755 /var/www/foo_fighters
    cp -v /vagrant/foo_fighters /etc/nginx/sites-available/
    sudo ln -s /etc/nginx/sites-available/foo_fighters /etc/nginx/sites-enabled/
    sudo systemctl restart nginx
  SHELL
end
