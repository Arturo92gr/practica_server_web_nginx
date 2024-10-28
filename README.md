# Servidor Web con Nginx
 Práctica: Instalación y configuración de servidor web Nginx

Tras instalar y configurar Nginx, como se puede ver en Vagrantfile, se sigue con los siguientes pasos:

1. Al no tener dns,se asocia la dirección IP a la web en el host Windows.  
    Se edita el archivo /etc/hosts para que asocie la IP de la máquina virtual a nginx_server.  
    En Windows está en el siguiente directorio: `C:\Windows\System32\drivers\etc\hosts`  
    Se añade la línea: `192.168.0.2 nginx_server`  

