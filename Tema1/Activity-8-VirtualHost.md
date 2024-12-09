# Actividad #8 - Virtual Host

## Documentación y práctica recomendada

Se recomienda revisar la siguiente documentación sobre hosting virtual en Apache:

- [Documentación oficial de Apache sobre hosting virtual](https://httpd.apache.org/docs/2.4/es/vhosts/)

Además, se sugiere realizar la siguiente práctica:

- [Cómo configurar Virtual Host de Apache en Ubuntu 14.04 LTS](https://www.digitalocean.com/community/tutorials/como-configurar-virtual-host-de-apache-en-ubuntu-14-04-lts-es)

## Ejercicios de virtual host

Para configurar los VirtualHosts en Apache, sigue estos pasos:

1\. Crea un directorio para tu sitio web:

```bash
   sudo mkdir -p /var/www/example.com/public_html
```

2\. Asigna los permisos adecuados:

```bash
   sudo chown -R $USER:$USER /var/www/example.com/public_html

   sudo chmod -R 755 /var/www
```

3\. Crea un archivo de configuración para el VirtualHost:

```bash
   sudo cp /etc/apache2/sites-available/000-default.conf /etc/apache2/sites-available/example.com.conf
```

4\. Edita el archivo de configuración:
```bash
   sudo nano /etc/apache2/sites-available/example.com.conf
```

5\. Configura el VirtualHost:

```apache
   <VirtualHost *:80>

       ServerAdmin webmaster@example.com

       ServerName example.com

       ServerAlias www.example.com

       DocumentRoot /var/www/example.com/public_html

       ErrorLog ${APACHE_LOG_DIR}/error.log

       CustomLog ${APACHE_LOG_DIR}/access.log combined

   </VirtualHost>
```

6\. Habilita el nuevo VirtualHost:

```bash
   sudo a2ensite example.com.conf
```

7\. Deshabilita el sitio por defecto:

```bash
   sudo a2dissite 000-default.conf
```

8\. Reinicia Apache para aplicar los cambios:

```bash
   sudo systemctl restart apache2
```

Repite estos pasos para cada VirtualHost que desees configurar, cambiando los nombres de dominio y rutas según sea necesario.

## Enlaces adicionales

- [Integrar una máquina virtual en una red local](http://geekland.eu/integrar-maquina-virtual-en-una-red-local/)

- [Configuring virtual network interfaces in Linux](http://linuxconfig.org/configuring-virtual-network-interfaces-in-linux)

- [Adding a second IP address to an existing network adapter on Windows](https://www.oclc.org/support/services/ezproxy/documentation/technote/2w.en.html)

---
**Autor:** Alejandro Mateo - [@AlexM2Media](https://github.com/AlexM2Media)  
**Repositorio:** [SREI-2ASIR](https://github.com/AlexM2Media/SREI-2ASIR)  
**Web/Portfolio:** [alexm2.media](https://alexm2.media)
