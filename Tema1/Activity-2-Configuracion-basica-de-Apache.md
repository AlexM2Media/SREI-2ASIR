# Actividad #2 - Configuración básica de Apache

Pon en marcha el servidor Apache y lleva a cabo los siguientes cambios en el archivo de configuración:

## 1. Apache utilizará el puerto 81 además del 80

Para configurar Apache para que escuche en el puerto 81 además del 80:

1\. Abre el archivo de configuración principal de Apache:

```
   sudo nano /etc/apache2/ports.conf
```

2\. Añade la siguiente línea debajo de "Listen 80":

```
   Listen 81
```

3\. Guarda el archivo y cierra el editor.

## 2. Añadir el dominio "marisma.intranet" en el fichero "hosts"

Para añadir el dominio al archivo hosts:

1\. Abre el archivo hosts:

```
   sudo nano /etc/hosts
```

2\. Añade la siguiente línea al final del archivo:

```
   127.0.0.1    marisma.intranet
```

3\. Guarda y cierra el archivo.

## 3. Cambia la directiva "ServerTokens" para mostrar el nombre del producto

1\. Abre el archivo de configuración de Apache:

```
   sudo nano /etc/apache2/apache2.conf
```

2\. Busca la línea "ServerTokens" y cámbiala a:

```
   ServerTokens Prod
```

3\. Guarda y cierra el archivo.

## 4. Haz que se visualice el pie de página de Apache en tu navegador

1\. Abre el archivo de configuración:

```
   sudo nano /etc/apache2/apache2.conf
```

2\. Busca la directiva "ServerSignature" y cámbiala a:

```
   ServerSignature On
```

3\. Guarda y cierra el archivo.

## 5. Crea un directorio "prueba" y otro directorio "prueba2". Incluye un par de páginas en cada una de ellas.

1\. Crea los directorios:

```
   sudo mkdir /var/www/html/prueba /var/www/html/prueba2
```

2\. Crea páginas de ejemplo en cada directorio:

```
   echo "<h1>Página de prueba 1</h1>" | sudo tee /var/www/html/prueba/index.html

   echo "<h1>Página de prueba 2</h1>" | sudo tee /var/www/html/prueba2/index.html
```

## 6. Redirecciona el contenido de la carpeta "prueba" hacia "prueba2"

1\. Abre el archivo de configuración del sitio por defecto:

```
   sudo nano /etc/apache2/sites-available/000-default.conf
```

2\. Añade las siguientes líneas dentro de la sección <VirtualHost>:

```
   Redirect /prueba /prueba2
```

3\. Guarda y cierra el archivo.

## 7. Es posible redireccionar tan solo una página en lugar de toda la carpeta. Pruébalo.

Para redireccionar solo una página específica:

1\. Abre el archivo de configuración del sitio:

```
   sudo nano /etc/apache2/sites-available/000-default.conf
```

2\. Añade la siguiente línea dentro de <VirtualHost>:

```
   Redirect /prueba/pagina.html /prueba2/pagina.html
```

3\. Guarda y cierra el archivo.

## 8. Usa la directiva userdir

1\. Habilita el módulo userdir:

```
   sudo a2enmod userdir
```

2\. Reinicia Apache:

```
   sudo systemctl restart apache2
```

## 9. Usa la directiva alias para redireccionar a una carpeta dentro del directorio de usuario.

1\. Abre el archivo de configuración del sitio:

```
   sudo nano /etc/apache2/sites-available/000-default.conf
```

2\. Añade la siguiente línea dentro de <VirtualHost>:

```
   Alias /usuario /home/usuario/public_html
```

3\. Guarda y cierra el archivo.

## 10. ¿Para qué sirve la directiva Options y dónde aparece. Comprueba si apache indexa los directorios. Si es así, ¿cómo lo desactivamos?

La directiva Options controla las características disponibles en un directorio específico. Aparece típicamente en bloques <Directory> en los archivos de configuración de Apache.

Para comprobar si Apache indexa los directorios:

1\. Crea un directorio sin un archivo index:

```
   sudo mkdir /var/www/html/test
```

2\. Accede a http://localhost/test en tu navegador.

Si ves una lista de archivos, el indexado está activado.

Para desactivar el indexado:

1\. Abre el archivo de configuración:

```
   sudo nano /etc/apache2/apache2.conf
```

2\. Busca la sección <Directory /var/www/> y cambia la línea Options a:

```
   Options FollowSymLinks
```

3\. Guarda, cierra el archivo y reinicia Apache:

```
   sudo systemctl restart apache2
```

---
**Autor:** Alejandro Mateo - [@AlexM2Media](https://github.com/AlexM2Media)  
**Repositorio:** [SREI-2ASIR](https://github.com/AlexM2Media/SREI-2ASIR)  
**Web/Portfolio:** [alexm2.media](https://alexm2.media)