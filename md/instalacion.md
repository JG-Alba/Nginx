# Balanceo de carga con Nginx

![image](https://quiksite.com/wp-content/uploads/2016/09/Nginx-Logo-02.png)


## 1.- Instalación del Servidor Nginx

Primero, deberás instalar Nginx en todos los servidores. Puedes hacerlo con el siguiente comando:

```bash
sudo apt-get install nginx -y
```
Una vez instalado, inicia el servicio Nginx y habilítalo para que se inicie al reiniciar el sistema:

```bash
sudo systemctl start nginx
sudo systemctl enable nginx
```
## 2.- Configuración de los Servidores de Aplicaciones

A continuación, es necesario ajustar la configuración en ambos servidores de aplicaciones, 
tanto del servidor principal como ambos de carga.

* En el primer servidor de aplicaciones, procede a eliminar el archivo `index.html` predeterminado y crear uno nuevo:

```bash
# rm /var/www/html/index*
# nano /var/www/html/index.html
```
Dentro del editor, agrega las siguientes líneas:

```bash
html>
<title>nginx</title>
<body>
Soy el servidor nginx
</body>
</html>
```
Guarda los cambios y cierra el archivo.
* Ahora, en el segundo servidor de aplicaciones, realiza el mismo proceso:

```bash
# rm /var/www/html/index*
# nano /var/www/html/index.html
```
Dentro del editor, agrega las siguientes líneas:

```bash
html>
<title>nginx</title>
<body>
Soy el servidor nginx de balanceo
</body>
</html>
```
Guarda los cambios y cierra el archivo.

## 3.- Configuración del Servidor de Balanceo de Carga

Para continuar, deberás configurar un servidor balanceador de carga que distribuya la carga entre ambos servidores de aplicaciones. 

Primero, elimina el archivo de configuración predeterminado de Nginx y crea un nuevo archivo de configuración del balanceador de carga:

```bash
# rm -rf /etc/nginx/sites-enabled/default 
# nano /etc/nginx/conf.d/balancing.conf
```

Añade las siguientes líneas:

```bash
upstream backend {
    server 44.201.225.0; # Ip del servidor primario desplegado en AWS
    server 3.83.128.15;  # Ip del servidor secundario desplegado en AWS
}

server {
    listen      80;
    server_name jgalba.ddns.net; # Dominio registrado con la IP del servidor de balanceo de carga

    location / {
        proxy_redirect      off;
        proxy_set_header    X-Real-IP $remote_addr;
        proxy_set_header    X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header    Host $http_host;
        proxy_pass http://backend;
    }
}
```

## 4.- Protección de Nginx con Let´s Encrypt
### Paso 1: Instalar Certbot

El primer paso para utilizar Let’s Encrypt y obtener un certificado SSL es instalar el software Certbot en tu servidor.

Instala Certbot y su complemento de Nginx utilizando el siguiente comando:

```bash
sudo apt install certbot python3-certbot-nginx
```
### Paso 2: Habilitar HTTPS a través del firewall

Verifique el estado actual del firewall:

```bash
sudo ufw status
```
Permita el tráfico HTTPS, active el perfil de Nginx Full y elimine el permiso redundante del perfil HTTP de Nginx:

```bash
sudo ufw allow 'Nginx Full'
sudo ufw delete allow 'Nginx HTTP'
```
### Paso 3: Obtener un certificado SSL

Ejecute Certbot con el complemento --nginx y especifique los nombres de dominio para los cuales desea que el certificado sea válido

```bash
sudo certbot --nginx -d jgalba.ddns.net
```

Si es la primera vez que ejecuta Certbot, se le pedirá que ingrese una dirección de correo electrónico y acepte las condiciones de servicio. Posteriormente, Certbot se comunicará con el servidor de Let’s Encrypt y realizará una verificación para confirmar que usted controla el dominio solicitado.

Una vez que se haya verificado el certificado, Certbot configurará automáticamente los archivos para que su dominio sea accesible a través de HTTPS.

Recuerde que este proceso puede variar dependiendo de la configuración específica de su servidor y sistema operativo. Asegúrese de seguir las recomendaciones y prácticas de seguridad pertinentes para su entorno.

## 5.- Comprobación
![image](/img/nginx.png)

![image](/img/nginx1.png)

