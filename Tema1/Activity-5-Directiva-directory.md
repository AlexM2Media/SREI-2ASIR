# Actividad #5 - Directiva directory

## Ejercicios

### 1. Crea un directorio llamado "dir1" y otro llamado "dir2"

Para crear los directorios:

```bash
sudo mkdir /var/www/html/dir1 /var/www/html/dir2
```

### 2. Explica qué diferencia existe entre ambos:

```apache
<Directory /var/www/example1>

    Order Deny,Allow

    Deny from All

    Allow from 192.168.1.100

</Directory>

<Directory /var/www/example1>

    Order Allow,Deny

    Deny from All

    Allow from 192.168.1.100

</Directory>
```

La diferencia principal está en el orden de evaluación de las reglas:

- En el primer caso (`Order Deny,Allow`), se deniega primero todo y luego se permite específicamente a 192.168.1.100.

- En el segundo caso (`Order Allow,Deny`), se permite primero y luego se deniega todo, pero la regla específica de Allow prevalece para 192.168.1.100.

En la práctica, ambas configuraciones permitirán el acceso solo desde 192.168.1.100, pero el segundo enfoque es más restrictivo por defecto.

### 3. Para dir1

a. Permite el acceso de las peticiones provenientes de 10.3.0.100:

```apache
<Directory /var/www/html/dir1>

    Order Deny,Allow

    Deny from All

    Allow from 10.3.0.100

</Directory>
```

b. Permite el acceso desde "marisma.intranet":

```apache
<Directory /var/www/html/dir1>

    Order Deny,Allow

    Deny from All

    Allow from marisma.intranet

</Directory>
```

c. Permite el acceso desde cualquier subdominio de "marisma.intranet":

```apache
<Directory /var/www/html/dir1>

    Order Deny,Allow

    Deny from All

    Allow from .marisma.intranet

</Directory>
```

d. Permite el acceso de las peticiones provenientes de "10.3.0.100" con máscara "255.255.0.0":

```apache
<Directory /var/www/html/dir1>

    Order Deny,Allow

    Deny from All

    Allow from 10.3.0.100/255.255.0.0

</Directory>
```

### 4. Modifica la configuración de forma que el acceso a dir1:

a. Se permita a "marisma.intranet" y no se permita desde 10.3.0.101:

```apache
<Directory /var/www/html/dir1>

    Order Allow,Deny

    Allow from marisma.intranet

    Deny from 10.3.0.101

</Directory>
```

### 5. Modifica la configuración de forma que el acceso a dir2:

a. Se permita a "10.3.0.100/8" y no a "marisma.intranet":

```apache
<Directory /var/www/html/dir2>

    Order Allow,Deny

    Allow from 10.3.0.100/8

    Deny from marisma.intranet

</Directory>
```

---
**Autor:** Alejandro Mateo - [@AlexM2Media](https://github.com/AlexM2Media)  
**Repositorio:** [SREI-2ASIR](https://github.com/AlexM2Media/SREI-2ASIR)  
**Web/Portfolio:** [alexm2.media](https://alexm2.media)