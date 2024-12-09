# Actividad #9 - Autenticación Digest

## Ejercicio:

Crea dos subdirectorios en el host virtual default que se llamen grupo1 y grupo2. Crea varios usuarios con la utilidad htdigest, asignando a cada uno un dominio distinto (dominio1 y dominio2). Configura el directorio grupo1 para que sólo puedan acceder los usuarios del dominio dominio1; y el directorio grupo2 para que sólo puedan acceder los usuarios del dominio dominio2.

### Pasos para realizar el ejercicio:

1\. Crear los subdirectorios:

```bash
   sudo mkdir /var/www/html/grupo1 /var/www/html/grupo2
```

2\. Crear usuarios con htdigest:

```bash
   sudo htdigest -c /etc/apache2/.htdigest_dominio1 dominio1 usuario1

   sudo htdigest /etc/apache2/.htdigest_dominio1 dominio1 usuario2

   sudo htdigest -c /etc/apache2/.htdigest_dominio2 dominio2 usuario3

   sudo htdigest /etc/apache2/.htdigest_dominio2 dominio2 usuario4
```

3\. Configurar Apache para usar autenticación digest:

   Edita el archivo de configuración del sitio por defecto:

```bash
   sudo nano /etc/apache2/sites-available/000-default.conf
```

   Añade las siguientes directivas dentro del bloque `<VirtualHost>`:

```apache
   <Directory "/var/www/html/grupo1">

       AuthType Digest

       AuthName "dominio1"

       AuthDigestDomain /grupo1/

       AuthDigestProvider file

       AuthUserFile /etc/apache2/.htdigest_dominio1

       Require valid-user

   </Directory>

   <Directory "/var/www/html/grupo2">

       AuthType Digest

       AuthName "dominio2"

       AuthDigestDomain /grupo2/

       AuthDigestProvider file

       AuthUserFile /etc/apache2/.htdigest_dominio2

       Require valid-user

   </Directory>
```

4\. Habilita el módulo de autenticación digest:

```bash
   sudo a2enmod auth_digest
```

5\. Reinicia Apache para aplicar los cambios:

```bash
   sudo systemctl restart apache2
```

Ahora, los usuarios del dominio1 solo podrán acceder al directorio grupo1, y los usuarios del dominio2 solo podrán acceder al directorio grupo2.

---
**Autor:** Alejandro Mateo - [@AlexM2Media](https://github.com/AlexM2Media)  
**Repositorio:** [SREI-2ASIR](https://github.com/AlexM2Media/SREI-2ASIR)  
**Web/Portfolio:** [alexm2.media](https://alexm2.media)