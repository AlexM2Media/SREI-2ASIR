# Actividad #4 - Directivas básicas

## 1. Crea un directorio llamado "privado" en la ruta /var/www/html. Incluye un archivo index.html con el texto "Acceso restringido".

Para crear el directorio y el archivo:

```bash

sudo mkdir /var/www/html/privado

echo "Acceso restringido" | sudo tee /var/www/html/privado/index.html
```

## 2. Configura el acceso a este directorio para que solo se pueda acceder desde localhost.

Edita el archivo de configuración del sitio por defecto:

```bash

sudo nano /etc/apache2/sites-available/000-default.conf
```

Añade las siguientes líneas dentro de la sección `<VirtualHost>`:

```apache

<Directory /var/www/html/privado>

    Order deny,allow

    Deny from all

    Allow from 127.0.0.1

</Directory>
```

Guarda el archivo y reinicia Apache:

```bash
sudo systemctl restart apache2
```

## 3. Crea un nuevo host virtual que se acceda a través del dominio "prueba.local"

1\. Crea un nuevo archivo de configuración:

```bash
sudo nano /etc/apache2/sites-available/prueba.local.conf
```

2\. Añade la siguiente configuración:

```apache
<VirtualHost *:80>

    ServerName prueba.local

    DocumentRoot /var/www/prueba.local

    ErrorLog ${APACHE_LOG_DIR}/prueba.local_error.log

    CustomLog ${APACHE_LOG_DIR}/prueba.local_access.log combined

</VirtualHost>
```

3\. Crea el directorio para el nuevo host virtual:

```bash
sudo mkdir /var/www/prueba.local
```

4\. Crea un archivo index.html en el nuevo directorio:

```bash
echo "<h1>Bienvenido a prueba.local</h1>" | sudo tee /var/www/prueba.local/index.html
```

5\. Habilita el nuevo sitio y reinicia Apache:

```bash
sudo a2ensite prueba.local.conf

sudo systemctl restart apache2
```

6\. Añade la entrada al archivo hosts:

```bash
sudo nano /etc/hosts
```

Añade la siguiente línea:

```text
127.0.0.1   prueba.local
```

## 4. Configura el nuevo host virtual para que solo se pueda acceder desde la red local

Edita el archivo de configuración del nuevo host virtual:

```bash
sudo nano /etc/apache2/sites-available/prueba.local.conf
```

Añade las siguientes líneas dentro de la sección `<VirtualHost>`:

```apache
<Directory /var/www/prueba.local>

    Order deny,allow

    Deny from all

    Allow from 192.168.0.0/16 10.0.0.0/8 172.16.0.0/12

</Directory>
```

Guarda el archivo y reinicia Apache:

```bash
sudo systemctl restart apache2
```

## 5. Crea un directorio llamado "privado" dentro del nuevo host virtual y configúralo para que sea necesario introducir un usuario y contraseña para acceder

1\. Crea el directorio:

```bash
sudo mkdir /var/www/prueba.local/privado
```

2\. Crea un archivo index.html en el nuevo directorio:

```bash
echo "<h1>Área privada de prueba.local</h1>" | sudo tee /var/www/prueba.local/privado/index.html
```

3\. Crea un archivo de contraseñas:

```bash
sudo htpasswd -c /etc/apache2/.htpasswd usuario1
```

4\. Edita el archivo de configuración del host virtual:

```bash
sudo nano /etc/apache2/sites-available/prueba.local.conf
```

5\. Añade las siguientes líneas dentro de la sección `<VirtualHost>`:

```apache
<Directory /var/www/prueba.local/privado>

    AuthType Basic

    AuthName "Área Restringida"

    AuthUserFile /etc/apache2/.htpasswd

    Require valid-user

</Directory>
```

6\. Guarda el archivo y reinicia Apache:

```bash
sudo systemctl restart apache2
```

Ahora, cuando intentes acceder a `http://prueba.local/privado`, se te pedirá un usuario y contraseña.

---
**Autor:** Alejandro Mateo - [@AlexM2Media](https://github.com/AlexM2Media)  
**Repositorio:** [SREI-2ASIR](https://github.com/AlexM2Media/SREI-2ASIR)  
**Web/Portfolio:** [alexm2.media](https://alexm2.media)
