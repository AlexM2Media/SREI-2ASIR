# Actividad #8 - Subdominios

## Enlaces

* Subdominio virtual: [http://www.zytrax.com/books/dns/ch9/subdomain.html](http://www.zytrax.com/books/dns/ch9/subdomain.html)
* Delegación del subdominio: [http://www.zytrax.com/books/dns/ch9/delegate.html](http://www.zytrax.com/books/dns/ch9/delegate.html)
* Libro Pro DNS and BIND (Chapter 4 DNS types): [http://it-ebooks.info/book/5022/](http://it-ebooks.info/book/5022/)
* Creación mediante script de subdominios:
       [http://bash.cyberciti.biz/domain/create-bind9-domain-zone-configuration-file/](http://bash.cyberciti.biz/domain/create-bind9-domain-zone-configuration-file/)
       [http://www.freeos.com/guides/lsst/scripts/AddDomain](http://www.freeos.com/guides/lsst/scripts/AddDomain)
* Otros: [https://python-for-system-administrators.readthedocs.io/en/latest/](https://python-for-system-administrators.readthedocs.io/en/latest/)

## Ejercicio

Configura un DNS primario para el dominio `iesmarisma.intranet`. Además, queremos crear un subdominio `informatica.iesmarisma.intranet` que resuelva los siguientes nombres:

* Nombre de dominio principal: `iesmarisma.intranet`
* Nombre de hosts en el dominio principal: `www`, `ftp`, `smtp`
* Nombre del subdominio: `informatica.iesmarisma.intranet`
* Nombre de hosts en el subdominio: `www`, `ftp`, `smtp`

Configura el subdominio siguiendo las instrucciones de los apuntes:

1. Creando un subdominio virtual.
2. Delega el subdominio.

## Actividad

1. Utiliza los enlaces anteriores para crear un script que permita crear subdominios.
2. Utiliza la directiva `($)` INCLUDE para incluir un fichero que contenga la descripción del subdominio.
3. Opcionalmente se recomienda leer el enlace "Python for system admin" y tratar de llevar a cabo la misma tarea usando la clase "Popen" del módulo "subprocess" de Python.

## Pasos

### 1. Configurar la zona iesmarisma.intranet

#### Modificar el fichero de configuración de zona directa `/etc/bind/db.iesmarisma.intranet` (zona)

```dns
$TTL    86400
@       IN      SOA     ns1.iesmarisma.intranet. admin.iesmarisma.intranet. (
                              2024022801  ; Serial
                              3600        ; Refresh
                              1800        ; Retry
                              604800      ; Expire
                              86400 )     ; Minimum TTL
@       IN      NS      ns1.iesmarisma.intranet.
ns1     IN      A       192.168.1.10
www     IN      A       192.168.1.20
ftp     IN      A       192.168.1.30
smtp    IN      A       192.168.1.40
```

![db.iesmarisma.intranet](/Tema2/.imgs/Act-8/Fig1.png)

#### Modificar el fichero de configuración de zona inversa `/etc/bind/db.192.168.1`

```dns
$TTL    86400
@       IN      SOA     ns1.iesmarisma.intranet. admin.iesmarisma.intranet. (
                              2024022801  ; Serial
                              3600        ; Refresh
                              1800        ; Retry
                              604800      ; Expire
                              86400 )     ; Minimum TTL
@       IN      NS      ns1.iesmarisma.intranet.
10      IN      PTR     ns1.iesmarisma.intranet.
20      IN      PTR     www.iesmarisma.intranet.
30      IN      PTR     ftp.iesmarisma.intranet.
40      IN      PTR     smtp.iesmarisma.intranet.
```

![db.10.5.0](/Tema2/.imgs/Act-8/Fig2.png)

### 2. Subdominio Virtual

#### Modificar el fichero de configuración de zona directa `/etc/bind/db.iesmarisma.intranet` (subdominio)

```dns
$TTL    86400
@       IN      SOA     ns1.iesmarisma.intranet. admin.iesmarisma.intranet. (
                              2024022802  ; Serial
                              3600        ; Refresh
                              1800        ; Retry
                              604800      ; Expire
                              86400 )     ; Minimum TTL
@       IN      NS      ns1.iesmarisma.intranet.
ns1     IN      A       192.168.1.10
www     IN      A       192.168.1.20
ftp     IN      A       192.168.1.30
smtp    IN      A       192.168.1.40
informatica IN      NS      ns1.iesmarisma.intranet.
www.informatica IN A 192.168.1.50
ftp.informatica IN A 192.168.1.60
smtp.informatica IN A 192.168.1.70
```

![db.iesmarisma.intranet](/Tema2/.imgs/Act-8/Fig3.png)

### 3. Delegación del subdominio

#### Crear fichero de zona para el subdominio `/etc/bind/db.informatica.iesmarisma.intranet` (delegación)

```dns
$TTL    86400
@       IN      SOA     ns1.informatica.iesmarisma.intranet. admin.informatica.iesmarisma.intranet. (
                              2024022801  ; Serial
                              3600        ; Refresh
                              1800        ; Retry
                              604800      ; Expire
                              86400 )     ; Minimum TTL
@       IN      NS      ns1.informatica.iesmarisma.intranet.
ns1     IN      A       192.168.1.10
www     IN      A       192.168.1.50
ftp     IN      A       192.168.1.60
smtp    IN      A       192.168.1.70
```

![db.informatica.iesmarisma.intranet](/Tema2/.imgs/Act-8/Fig4.png)

#### Modificar el fichero de configuración de zona directa `/etc/bind/db.iesmarisma.intranet`

```dns
$TTL    86400
@       IN      SOA     ns1.iesmarisma.intranet. admin.iesmarisma.intranet. (
                              2024022803  ; Serial
                              3600        ; Refresh
                              1800        ; Retry
                              604800      ; Expire
                              86400 )     ; Minimum TTL
@       IN      NS      ns1.iesmarisma.intranet.
ns1     IN      A       192.168.1.10
www     IN      A       192.168.1.20
ftp     IN      A       192.168.1.30
smtp    IN      A       192.168.1.40
informatica IN      NS      ns1.informatica.iesmarisma.intranet.
```

![db.iesmarisma.intranet](/Tema2/.imgs/Act-8/Fig5.png)

#### Modificar el fichero `/etc/bind/named.conf.local` para declarar la zona del subdominio

```dns
zone "informatica.iesmarisma.intranet" {
       type master;
       file "/etc/bind/db.informatica.iesmarisma.intranet";
};
```

![named.conf.local](/Tema2/.imgs/Act-8/Fig6.png)

### 4. Scripting

Ahora, la actividad sugiere la creación de un script para automatizar la creación de los subdominios.

```bash
#!/bin/bash

# Script para crear un subdominio en BIND9

DOMAIN=$1
SUBDOMAIN=$2

# Crear el archivo de zona para el subdominio
cat > /etc/bind/db.$SUBDOMAIN.$DOMAIN > /etc/bind/named.conf.local <<EOF
zone "$SUBDOMAIN.$DOMAIN" {
       type master;
       file "/etc/bind/db.$SUBDOMAIN.$DOMAIN";
};
EOF

# Reiniciar el servicio BIND9
systemctl restart bind9
```

![createsubdomain.sh](/Tema2/.imgs/Act-8/Fig7.png)

Recuerda adaptar las direcciones IP y nombres de dominio a tu configuración.

**Nota:**

Si tras realizar las consultas con `dig` o `nslookup` el servidor DNS resuelve el dominio correctamente, pero `ping` o el navegador no lo hacen, consulta el archivo `/etc/nsswitch.conf` y modifica la línea:

```text
hosts: files mdns4_minimal [NOTFOUND=return] dns mdns4
```

a:

```text
hosts: files dns
```

Para persistir los cambios, puedes eliminar el paquete `libnss-mdns`:

```bash
sudo apt-get remove libnss-mdns
```

---
**Autor:** Alejandro Mateo - [@AlexM2Media](https://github.com/AlexM2Media)  
**Repositorio:** [SREI-2ASIR](https://github.com/AlexM2Media/SREI-2ASIR)  
**Web/Portfolio:** [alexm2.media](https://alexm2.media)
