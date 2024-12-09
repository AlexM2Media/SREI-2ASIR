# Actividad #9 - Autenticación

## Ejercicios adicionales de autenticación básica

### 1. Crea cinco usuarios: usuario1, usuario2, usuario3, usuario4, usuario5.

Para crear los usuarios con contraseñas, utiliza el comando `htpasswd`:

```bash
sudo htpasswd -c /etc/apache2/.htpasswd usuario1

sudo htpasswd /etc/apache2/.htpasswd usuario2

sudo htpasswd /etc/apache2/.htpasswd usuario3

sudo htpasswd /etc/apache2/.htpasswd usuario4

sudo htpasswd /etc/apache2/.htpasswd usuario5
```

El archivo `.htpasswd` se encuentra en `/etc/apache2/` y contiene las credenciales de los usuarios.

---

### 2. Crea dos grupos de usuarios:

   - **Grupo 1**: formado por usuario1 y usuario2.

   - **Grupo 2**: formado por usuario3, usuario4 y usuario5.

Crea un archivo de grupos llamado `groups` en `/etc/apache2/`:

```bash
sudo nano /etc/apache2/groups
```

Añade el siguiente contenido:

```
grupo1: usuario1 usuario2

grupo2: usuario3 usuario4 usuario5
```

Guarda y cierra el archivo.

---

### 3. Crea un directorio llamado `privado1` que permita el acceso a todos los usuarios.

1\. Crea el directorio:

```bash
sudo mkdir /var/www/html/privado1
```

2\. Crea un archivo `index.html` dentro del directorio:

```bash
echo "Acceso permitido a todos los usuarios" | sudo tee /var/www/html/privado1/index.html
```

3\. Configura la autenticación en el archivo de configuración del sitio:

```bash
sudo nano /etc/apache2/sites-available/000-default.conf
```

Añade lo siguiente dentro del bloque `<VirtualHost>`:

```apache
<Directory "/var/www/html/privado1">

    AuthType Basic

    AuthName "Área Restringida"

    AuthUserFile /etc/apache2/.htpasswd

    Require valid-user

</Directory>
```

4\. Reinicia Apache:

```bash
sudo systemctl restart apache2
```

---

### 4. Crea un directorio llamado `privado2` que permita el acceso sólo a los usuarios del grupo1.

1\. Crea el directorio:

```bash
sudo mkdir /var/www/html/privado2
```

2\. Crea un archivo `index.html` dentro del directorio:

```bash
echo "Acceso permitido solo al grupo1" | sudo tee /var/www/html/privado2/index.html
```

3\. Configura la autenticación en el archivo de configuración del sitio:

```bash
sudo nano /etc/apache2/sites-available/000-default.conf
```

Añade lo siguiente dentro del bloque `<VirtualHost>`:

```apache
<Directory "/var/www/html/privado2">

    AuthType Basic

    AuthName "Área Restringida"

    AuthUserFile /etc/apache2/.htpasswd

    AuthGroupFile /etc/apache2/groups

    Require group grupo1

</Directory>
```

4\. Reinicia Apache:

```bash
sudo systemctl restart apache2
```

---

### 5. La directiva `Satisfy` controla cómo se debe comportar el servidor cuando tenemos autorizaciones a nivel de host (order, allow, deny) y autorizaciones de usuarios (require).

Por ejemplo, para combinar autorización por IP y autenticación básica, puedes usar la directiva `Satisfy`.

#### Ejemplo con `Satisfy any`:

Permite acceso si se cumple **cualquiera** de las condiciones (IP o autenticación).

```apache
<Directory "/var/www/html/privado2">

    AuthType Basic

    AuthName "Área Restringida"

    AuthUserFile /etc/apache2/.htpasswd

    Require valid-user

    Order deny,allow

    Deny from all

    Allow from 127.0.0.1

    Satisfy any

</Directory>
```

#### Ejemplo con `Satisfy all`:

Requiere que se cumplan **todas** las condiciones (IP y autenticación).

```apache
<Directory "/var/www/html/privado2">

    AuthType Basic

    AuthName "Área Restringida"

    AuthUserFile /etc/apache2/.htpasswd

    Require valid-user

    Order deny,allow

    Deny from all

    Allow from 127.0.0.1

    Satisfy all

</Directory>
```

---

### 6. En el directorio `privado2`, haz que sólo sea accesible desde localhost y estudia cómo se comporta la autorización con `Satisfy any` y `Satisfy all`.

#### Configuración para acceso desde localhost:

Edita el archivo de configuración del sitio:

```bash
sudo nano /etc/apache2/sites-available/000-default.conf
```

Añade lo siguiente dentro del bloque `<VirtualHost>` para el directorio `privado2`:

```apache
<Directory "/var/www/html/privado2">

    AuthType Basic

    AuthName "Área Restringida"

    AuthUserFile /etc/apache2/.htpasswd

    Require valid-user

    Order deny,allow

    Deny from all

    Allow from 127.0.0.1

    Satisfy any  # Cambiar a 'all' para probar ambos comportamientos.

</Directory>
```

Reinicia Apache para aplicar los cambios:

```bash
sudo systemctl restart apache2
```

#### Comportamiento:

- Con `Satisfy any`: El acceso será permitido si se cumple **alguna** condición (autenticación o IP permitida).

- Con `Satisfy all`: El acceso será permitido solo si se cumplen **todas** las condiciones (autenticación y IP permitida).

---
**Autor:** Alejandro Mateo - [@AlexM2Media](https://github.com/AlexM2Media)  
**Repositorio:** [SREI-2ASIR](https://github.com/AlexM2Media/SREI-2ASIR)  
**Web/Portfolio:** [alexm2.media](https://alexm2.media)