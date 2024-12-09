# Actividad #9 - Autenticación mediante módulo dbd

## Configuración de autenticación con MySQL y mod_dbd

Para configurar un directorio con autenticación usando MySQL y mod_dbd, siga estos pasos:

1\. Instale los módulos necesarios:

```bash
   sudo apt-get install libapache2-mod-auth-mysql

   sudo a2enmod auth_mysql

   sudo a2enmod dbd
```

2\. Configure la conexión a la base de datos en el archivo de configuración de Apache:

```apache
   DBDriver mysql

   DBDParams "host=localhost,dbname=nombre_base_datos,user=usuario_mysql,pass=contraseña_mysql"
```

3\. Cree una tabla en MySQL para almacenar los usuarios:

```sql
   CREATE TABLE usuarios (

     username VARCHAR(50) NOT NULL PRIMARY KEY,

     password VARCHAR(255) NOT NULL

   );
```

4\. Inserte algunos usuarios de prueba:

```sql
   INSERT INTO usuarios (username, password) VALUES ('usuario1', PASSWORD('contraseña1'));

   INSERT INTO usuarios (username, password) VALUES ('usuario2', PASSWORD('contraseña2'));
```

5\. Configure la autenticación para un directorio específico en el archivo de configuración del sitio:

```apache
   <Directory "/var/www/html/directorio_protegido">

       AuthName "Área Restringida"

       AuthType Basic

       AuthBasicProvider dbd

       AuthDBDUserPWQuery "SELECT password FROM usuarios WHERE username = %s"

       Require valid-user

   </Directory>
```

6\. Reinicie Apache para aplicar los cambios:

```bash
   sudo systemctl restart apache2
```

Asegúrese de reemplazar "nombre_base_datos", "usuario_mysql", "contraseña_mysql" y "/var/www/html/directorio_protegido" con los valores apropiados para su configuración.

---
**Autor:** Alejandro Mateo - [@AlexM2Media](https://github.com/AlexM2Media)  
**Repositorio:** [SREI-2ASIR](https://github.com/AlexM2Media/SREI-2ASIR)  
**Web/Portfolio:** [alexm2.media](https://alexm2.media)
