# Servidor Web con Nginx
### Práctica: Instalación y configuración de servidor web Nginx

Tras instalar y configurar Nginx, como se puede ver en Vagrantfile, se sigue con los siguientes pasos:

1. Al no tener dns, se asocia la dirección IP a la web en el host Windows.  
    Se edita el archivo /etc/hosts para que asocie la IP de la máquina virtual a nginx_server.  
    En Windows está en el siguiente directorio:  
    `C:\Windows\System32\drivers\etc\hosts`  
    Se añade la línea:  
    `192.168.0.2 nginx_server.es`  

2. Instalar vsftpd. Se pedirán los siguientes datos:  

        If you enter '.', the field will be left blank.  
        -----  
        Country Name (2 letter code) [AU]:  
        State or Province Name (full name) [Some-State]:  
        Locality Name (eg, city) []:  
        Organization Name (eg, company) [Internet Widgits Pty Ltd]:  
        Organizational Unit Name (eg, section) []:  
        Common Name (e.g. server FQDN or YOUR name) []:  

3. Crear certificados de seguridad:  

        sudo openssl req -x509 -nodes -days 365 -newkey rsa:2048 -keyout /etc/ssl/private/vsftpd.key -out /etc/ssl/certs/vsftpd.crt

4. En /etc/vsftpd.conf:
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
    
    - También se deberá descomentar la siguiente línea para permitir la escritura y así poder transferir archivos mediante FTPES con WinSCP:

            write_enable=YES

    - Verificar que las rutas del certificado y la clave privada sean correctas:

            ls -l /etc/ssl/certs/vsftpd.crt /etc/ssl/private/vsftpd.key

    - Es posible que haya que ajustar los permisos de la clave privada:

            sudo chmod 755 /etc/ssl/private

5. Reiniciar el servicio:

        sudo systemctl restart vsftpd

6. Comprobar que está funcionando correctamente:
    
        sudo systemctl status vsftpd

### Conectar mediant FTPES

En este caso se ha utilizado el programa <a href="https://winscp.net/eng/download.php">WinSCP</a>.

Para poder conectar se requiere una contraseña de usuario, la cual no tenemos si estamos en nuestra máquina vagrant. Por ello se ha creado y asignado una nueva contraseña al usuario vagrant con `sudo passwd vagrant`.

<img src="./htdocs/1.png">

<br>

Tras conectar, se transfieren los archivos de nuestra web al directorio ftp del servidor:

<img src="./htdocs/2.png">