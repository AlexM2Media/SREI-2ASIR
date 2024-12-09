# Actividad #10 - SSL

## Cifrado asimétrico

El cifrado asimétrico, también conocido como criptografía de clave pública, es un método de cifrado que utiliza dos claves diferentes:

- Una clave pública que se puede compartir libremente

- Una clave privada que debe mantenerse en secreto

Características principales:

- Proporciona confidencialidad, integridad y autenticación

- Más seguro pero menos eficiente que el cifrado simétrico

- Se usa comúnmente para intercambio seguro de claves y firmas digitales

- Algoritmos populares incluyen RSA y Curvas Elípticas

## Creación de certificado SSL autofirmado para Apache

1\. Habilitar el módulo SSL:

```bash
   sudo a2enmod ssl

   sudo systemctl restart apache2
```

2\. Generar el certificado y la clave:

```bash
   sudo openssl req -x509 -nodes -days 365 -newkey rsa:2048 -keyout /etc/ssl/private/apache-selfsigned.key -out /etc/ssl/certs/apache-selfsigned.crt
```

3\. Configurar el VirtualHost SSL:

```apache
   <VirtualHost *:443>

     ServerName your_domain_or_ip

     DocumentRoot /var/www/your_domain_or_ip

     SSLEngine on

     SSLCertificateFile /etc/ssl/certs/apache-selfsigned.crt

     SSLCertificateKeyFile /etc/ssl/private/apache-selfsigned.key

   </VirtualHost>
```

4\. Habilitar el sitio y reiniciar Apache:

```bash
   sudo a2ensite your_domain_or_ip.conf

   sudo systemctl restart apache2
```

## DNS dinámico

Para configurar DNS dinámico con No-IP en Ubuntu:

1\. Instalar el cliente DUC de No-IP

2\. Configurar el cliente con tus credenciales

3\. Habilitar el servicio para que se inicie automáticamente

## Ejercicio

Sigue las indicaciones de los artículos para:

1\. Crear un certificado SSL autofirmado

2\. Activar el módulo SSL en Apache

3\. Configurar un VirtualHost SSL

4\. Comprobar que todo funciona correctamente accediendo por HTTPS

---
**Autor:** Alejandro Mateo - [@AlexM2Media](https://github.com/AlexM2Media)  
**Repositorio:** [SREI-2ASIR](https://github.com/AlexM2Media/SREI-2ASIR)  
**Web/Portfolio:** [alexm2.media](https://alexm2.media)
