# Actividad AWS 1

## Preparación del entorno

1. Inicie sesión en su instancia AWS y actualice el sistema:

    ```bash
    sudo apt update && sudo apt upgrade -y
    ```

    ![Fig_1](/AWS/.imgs/Fig_1.png)

2. Instale Apache2 si aún no está instalado:

    ```bash
    sudo apt install apache2 -y
    ```

    ![Fig_2](/AWS/.imgs/Fig_2.png)

## Configuración de autenticación con MySQL

### Instalación de módulos necesarios

1. Instale los módulos requeridos:

    ```bash
    sudo apt-get install libapache2-mod-auth-mysql
    sudo a2enmod auth_mysql
    sudo a2enmod dbd
    ```

    ![Fig_3](/AWS/.imgs/Fig_3.png)

### Configuración de la base de datos

1. Instale MySQL si aún no está instalado:

    ```bash
    sudo apt install mysql-server -y
    ```

    ![Fig_4](/AWS/.imgs/Fig_4.png)

2. Acceda a MySQL y cree una base de datos y un usuario:

    ```sql
    CREATE DATABASE apache_auth;
    CREATE USER 'apache_user'@'localhost' IDENTIFIED BY 'contraseña_segura';
    GRANT ALL PRIVILEGES ON apache_auth.* TO 'apache_user'@'localhost';
    FLUSH PRIVILEGES;
    ```

    ![Fig_5](/AWS/.imgs/Fig_5.png)
    ![Fig_6](/AWS/.imgs/Fig_6.png)

3. Cree una tabla para almacenar los usuarios:

    ```sql
    USE apache_auth;
    CREATE TABLE usuarios (
    username VARCHAR(50) NOT NULL PRIMARY KEY,
    password CHAR(64) NOT NULL
    );
    ```

    ![Fig_7](/AWS/.imgs/Fig_7.png)

4. Inserte algunos usuarios de prueba:

    ```sql
    INSERT INTO usuarios (username, password) VALUES ('usuario1', SHA2('contraseña1', 256));
    INSERT INTO usuarios (username, password) VALUES ('usuario2', SHA2('contraseña2', 256));
    ```

### Configuración de Apache

1. Edite el archivo de configuración de Apache:

    ```bash
    sudo nano /etc/apache2/apache2.conf
    ```

    ![Fig_8](/AWS/.imgs/Fig_8.png)

2. Agregue la siguiente configuración para la conexión a la base de datos:

    ```apache
    DBDriver mysql
    DBDParams "host=localhost,dbname=apache_auth,user=apache_user,pass=contraseña_segura"
    ```

    ![Fig_9](/AWS/.imgs/Fig_9.png)

3. Configure la autenticación para un directorio específico:

    ```apache
    <Directory /var/www/html/area_restringida>
        AuthName "Área Restringida"
        AuthType Basic
        AuthBasicProvider dbd
        AuthDBDUserPWQuery "SELECT password FROM usuarios WHERE username = %s"
        Require valid-user
    </Directory>
    ```

    ![Fig_10](/AWS/.imgs/Fig_10.png)

4. Cree el directorio protegido:

    ```bash
    sudo mkdir /var/www/html/area_restringida
    sudo echo "Contenido protegido" > /var/www/html/area_restringida/index.html
    ```

    ![Fig_11](/AWS/.imgs/Fig_11.png)

## Creación de certificado SSL autofirmado

1. Habilite el módulo SSL:

    ```bash
    sudo a2enmod ssl
    sudo systemctl restart apache2
    ```

    ![Fig_12](/AWS/.imgs/Fig_12.png)

2. Genere el certificado y la clave:

    ```bash
    sudo openssl req -x509 -nodes -days 365 -newkey rsa:2048 -keyout /etc/ssl/private/apache-selfsigned.key -out /etc/ssl/certs/apache-selfsigned.crt
    ```

    ![Fig_13](/AWS/.imgs/Fig_13.png)

3. Configure el VirtualHost SSL editando el archivo de configuración:

    ```bash
    sudo nano /etc/apache2/sites-available/default-ssl.conf
    ```

    ![Fig_14](/AWS/.imgs/Fig_14.png)

4. Modifique la configuración del VirtualHost:

    ```apache
    <VirtualHost *:443>
        ServerName your_aws_instance_ip
        DocumentRoot /var/www/html

        SSLEngine on
        SSLCertificateFile /etc/ssl/certs/apache-selfsigned.crt
        SSLCertificateKeyFile /etc/ssl/private/apache-selfsigned.key
    </VirtualHost>
    ```

    ![Fig_15](/AWS/.imgs/Fig_15.png)

5. Habilite el sitio SSL y reinicie Apache

    ```bash
    sudo a2ensite default-ssl.conf
    sudo systemctl restart apache2
    ```

    ![Fig_16](/AWS/.imgs/Fig_16.png)

## Verificación

1. Compruebe que la autenticación funciona accediendo a `http://your_aws_instance_ip/area_restringida`

2. Verifique que HTTPS está funcionando accediendo a `https://your_aws_instance_ip`

---
**Autor:** Alejandro Mateo - [@AlexM2Media](https://github.com/AlexM2Media)  
**Repositorio:** [SREI-2ASIR](https://github.com/AlexM2Media/SREI-2ASIR)  
**Web/Portfolio:** [alexm2.media](https://alexm2.media)
