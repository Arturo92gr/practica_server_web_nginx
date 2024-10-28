# Servidor Web con Nginx
 Práctica: Instalación y configuración de servidor web Nginx

Tras instalar y configurar Nginx, como se puede ver en Vagrantfile, se sigue con los siguientes pasos:

1. Al no tener dns, se asocia la dirección IP a la web en el host Windows.  
    Se edita el archivo /etc/hosts para que asocie la IP de la máquina virtual a nginx_server.  
    En Windows está en el siguiente directorio:  
    `C:\Windows\System32\drivers\etc\hosts`  
    Se añade la línea:  
    `192.168.0.2 nginx_server.es`  

2. Instalar vsftpd. Se pedirán los siguientes datos:  
    `If you enter '.', the field will be left blank.`  
    `Country Name (2 letter code) [AU]:`  
    `State or Province Name (full name) [Some-State]:`  
    `Locality Name (eg, city) []:`  
    `Organization Name (eg, company) [Internet Widgits Pty Ltd]:`  
    `Organizational Unit Name (eg, section) []:`  
    `Common Name (e.g. server FQDN or YOUR name) []:`  

3. Crear certificados de seguridad:  
    `sudo openssl req -x509 -nodes -days 365 -newkey rsa:2048 -keyout /etc/ssl/private/vsftpd.key -out /etc/ssl/certs/vsftpd.cr`

4. En vsftpd.conf:
    - Borrar líneas:  

            rsa_cert_file=/etc/ssl/certs/ssl-cert-snakeoil.pem
            rsa_private_key_file=/etc/ssl/private/ssl-cert-snakeoil.key
            ssl_enable=NO

    - En su lugar añadir líneas:  

            rsa_cert_file=/etc/ssl/certs/vsftpd.crt
            rsa_private_key_file=/etc/ssl/private/vsftpd.key
            ssl_enable=YES
            allow_anon_ssl=NO
            force_local_data_ssl=YES
            force_local_logins_ssl=YES
            ssl_tlsv1=YES
            ssl_sslv2=NO
            ssl_sslv3=NO
            require_ssl_reuse=NO
            ssl_ciphers=HIGH

            local_root=/home/vagrant/ftp

5. Reiniciar el servicio:  
    `sudo systemctl restart vsftpd`