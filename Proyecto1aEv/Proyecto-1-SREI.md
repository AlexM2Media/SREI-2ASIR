# Práctica del Primer Trimestre - Servidor Web Interno para un Instituto

## 1. Instalación del servidor web Apache

```bash
sudo apt update

sudo apt install apache2
```

## 2. Configuración de dominios en el archivo hosts

Editar el archivo `/etc/hosts`:

```bash
sudo nano /etc/hosts
```

Añadir las siguientes líneas:

```text
127.0.0.1   centro.intranet

127.0.0.1   departamentos.centro.intranet
```

## 3. Activación de módulos para PHP y MySQL

```bash
sudo apt install php libapache2-mod-php php-mysql

sudo apt install mysql-server

sudo a2enmod php

sudo systemctl restart apache2
```

## 4. Instalación y configuración de WordPress

```bash
sudo mysql -u root -p
```

En MySQL:

```sql
CREATE DATABASE wordpress;

CREATE USER 'wordpressuser'@'localhost' IDENTIFIED BY 'password';

GRANT ALL PRIVILEGES ON wordpress.* TO 'wordpressuser'@'localhost';

FLUSH PRIVILEGES;

EXIT;
```

Descargar e instalar WordPress:

```bash
cd /tmp

curl -O https://wordpress.org/latest.tar.gz

tar xzvf latest.tar.gz

sudo mv wordpress /var/www/html/centro.intranet

sudo chown -R www-data:www-data /var/www/html/centro.intranet
```

Configurar Apache para centro.intranet:

```bash
sudo nano /etc/apache2/sites-available/centro.intranet.conf
```

Añadir:

```apache
<VirtualHost *:80>

    ServerName centro.intranet

    DocumentRoot /var/www/html/centro.intranet

    <Directory /var/www/html/centro.intranet>

        AllowOverride All

    </Directory>

</VirtualHost>
```

Activar el sitio:

```bash
sudo a2ensite centro.intranet.conf

sudo systemctl reload apache2
```

## 5. Activación del módulo wsgi

```bash
sudo apt install libapache2-mod-wsgi-py3

sudo a2enmod wsgi

sudo systemctl restart apache2
```

## 6. Creación y despliegue de una aplicación Python

Crear el directorio y la aplicación:

```bash
sudo mkdir /var/www/html/departamentos.centro.intranet

sudo nano /var/www/html/departamentos.centro.intranet/app.py
```

Contenido de app.py:

```python
def application(environ, start_response):

    status = '200 OK'

    output = b'Hello World from Python!'

    response_headers = [('Content-type', 'text/plain'),

                        ('Content-Length', str(len(output)))]

    start_response(status, response_headers)

    return [output]
```

Configurar Apache para departamentos.centro.intranet:

```bash

sudo nano /etc/apache2/sites-available/departamentos.centro.intranet.conf
```

Añadir:

```apache
<VirtualHost *:80>

    ServerName departamentos.centro.intranet

    WSGIScriptAlias / /var/www/html/departamentos.centro.intranet/app.py

    <Directory /var/www/html/departamentos.centro.intranet>

        Require all granted

    </Directory>

</VirtualHost>
```

Activar el sitio:

```bash
sudo a2ensite departamentos.centro.intranet.conf

sudo systemctl reload apache2
```

## 7. Protección del acceso a la aplicación Python mediante autenticación

```bash
sudo htpasswd -c /etc/apache2/.htpasswd usuario
```

Modificar la configuración de Apache:

```bash
sudo nano /etc/apache2/sites-available/departamentos.centro.intranet.conf
```

Añadir dentro de la sección `<Directory>`:

```apache
AuthType Basic

AuthName "Área Restringida"

AuthUserFile /etc/apache2/.htpasswd

Require valid-user
```

Reiniciar Apache:

```bash
sudo systemctl restart apache2
```

## 8. Instalación y configuración de AWStats

```bash
sudo apt install awstats

sudo a2enmod cgi

sudo cp /etc/awstats/awstats.conf /etc/awstats/awstats.centro.intranet.conf

sudo nano /etc/awstats/awstats.centro.intranet.conf
```

Modificar en el archivo:

```text
LogFile="/var/log/apache2/access.log"

SiteDomain="centro.intranet"
```

Configurar cron para actualizar estadísticas:

```bash
sudo nano /etc/cron.d/awstats
```

Añadir:

```text
*/10 * * * * www-data /usr/lib/cgi-bin/awstats.pl -config=centro.intranet -update > /dev/null
```

## 9. Instalación del segundo servidor web (nginx)

```bash
sudo apt install nginx

sudo nano /etc/nginx/sites-available/servidor2.centro.intranet
```

Configuración de nginx:

```nginx
server {

    listen 8080;

    server_name servidor2.centro.intranet;

    root /var/www/html/servidor2.centro.intranet;

    index index.php index.html index.htm;

    location / {

        try_files $uri $uri/ =404;

    }

    location ~ \.php$ {

        fastcgi_pass unix:/var/run/php/php7.4-fpm.sock;

        fastcgi_index index.php;

        include fastcgi_params;

    }

}
```

Activar el sitio:

```bash
sudo ln -s /etc/nginx/sites-available/servidor2.centro.intranet /etc/nginx/sites-enabled/

sudo nginx -t

sudo systemctl restart nginx
```

Instalar PHP-FPM y phpMyAdmin:

```bash
sudo apt install php-fpm

sudo apt install phpmyadmin
```

Configurar phpMyAdmin para nginx:

```bash
sudo ln -s /usr/share/phpmyadmin /var/www/html/servidor2.centro.intranet/phpmyadmin
```

---
**Autor:** Alejandro Mateo - [@AlexM2Media](https://github.com/AlexM2Media)  
**Repositorio:** [SREI-2ASIR](https://github.com/AlexM2Media/SREI-2ASIR)  
**Web/Portfolio:** [alexm2.media](https://alexm2.media)
