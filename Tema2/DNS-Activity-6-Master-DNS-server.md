# Actividad #6 - Servidor DNS Maestro con BIND9

## Ejercicio

Crear un fichero de zona para "`marisma.intranet`", además de la zona de resolución inversa. En la zona se definirán los siguientes FQDN:

* Servidor DNS: ns1
* Servidor FTP: ftp1
* Servidores de correo: mail1, mail2
* Servidores web: www, departamentos

Se pide:

1. Configurar el servidor DNS con los registros necesarios.
2. Cambiar la configuración del cliente para que emplee el nuevo servidor DNS: `/etc/resolv.conf`.
3. Hacer las consultas necesarias para comprobar el correcto funcionamiento del servidor DNS: (SOA, MX, A, NS). Comprobar además la resolución inversa.

**Referencias:**

* [Cómo configurar BIND como un servidor DNS autoritativo en Ubuntu 14.04](https://www.digitalocean.com/community/tutorials/how-to-configure-bind-as-an-authoritative-only-dns-server-on-ubuntu-14-04)
* [zytrax.com - DNS](http://www.zytrax.com/books/dns/ch6/)
* [BIND9ServerHowto - Ubuntu Community Help Wiki](https://help.ubuntu.com/community/BIND9ServerHowto)
* [Libro Pro DNS and BIND (Chapter 4 DNS types)](http://it-ebooks.info/book/5022/)

### Pasos

1. **Instalación de BIND9 (si no está instalado):**

    ```bash
    sudo apt update
    sudo apt install bind9 bind9utils bind9-doc
    ```

2. **Configuración de la Zona Directa (marisma.intranet):**

    * Crear el archivo de zona directa:

        ```bash
        sudo nano /etc/bind/db.marisma.intranet
        ```

        Ejemplo de contenido de `db.marisma.intranet`:

        ```dns
        $TTL    86400
        @       IN      SOA     ns1.marisma.intranet. admin.marisma.intranet. (
                              2024022701  ; Serial
                              3600        ; Refresh
                              1800        ; Retry
                              604800      ; Expire
                              86400 )     ; Minimum TTL
        @       IN      NS      ns1.marisma.intranet.
        ns1     IN      A       192.168.1.10
        ftp1    IN      A       192.168.1.20
        mail1   IN      A       192.168.1.30
        mail2   IN      A       192.168.1.31
        www     IN      A       192.168.1.40
        departamentos IN      A       192.168.1.41
        ```

        ![db.marisma.intranet](/Tema2/.imgs/Act-6/Fig1.png)

3. **Configuración de la Zona Inversa:**

    * Determinar la red para la zona inversa (ej: 192.168.1.0/24).
    * Crear el archivo de zona inversa:

        ```bash
        sudo nano /etc/bind/db.192.168.1
        ```

        Ejemplo de contenido de `db.192.168.1`:

        ```dns
        $TTL    86400
        @       IN      SOA     ns1.marisma.intranet. admin.marisma.intranet. (
                              2024022701  ; Serial
                              3600        ; Refresh
                              1800        ; Retry
                              604800      ; Expire
                              86400 )     ; Minimum TTL
        @       IN      NS      ns1.marisma.intranet.
        10      IN      PTR     ns1.marisma.intranet.
        20      IN      PTR     ftp1.marisma.intranet.
        30      IN      PTR     mail1.marisma.intranet.
        31      IN      PTR     mail2.marisma.intranet.
        40      IN      PTR     www.marisma.intranet.
        41      IN      PTR     departamentos.marisma.intranet.
        ```

        ![db.10.5.0](/Tema2/.imgs/Act-6/Fig2.png)

4. **Configuración de BIND para las Zonas:**

    * Editar el archivo `/etc/bind/named.conf.local`:

        ```bash
        sudo nano /etc/bind/named.conf.local
        ```

        Añadir las siguientes zonas:

        ```text
        zone "marisma.intranet" {
            type master;
            file "/etc/bind/db.marisma.intranet";
        };

        zone "1.168.192.in-addr.arpa" {
            type master;
            file "/etc/bind/db.192.168.1";
        };
        ```

        ![named.conf.local](/Tema2/.imgs/Act-6/Fig3.png)

5. **Reiniciar BIND9:**

    ```bash
    sudo systemctl restart bind9
    ```

6. **Configuración del Cliente DNS (temporal):**

    * Editar el archivo `/etc/resolv.conf`:

        ```bash
        sudo nano /etc/resolv.conf
        ```

        Añadir o modificar la línea `nameserver` para que apunte al servidor DNS:

        ```text
        nameserver 127.0.0.1
        ```

    * **Nota importante:** Los cambios en `/etc/resolv.conf` son temporales y se perderán al reiniciar.

7. **Configuración del Cliente DNS (persistente):**

    ```bash
    sudo nano /etc/nsswitch.conf
    ```

    Cambiar la línea:

    ```text
    hosts: files mdns4_minimal [NOTFOUND=return] dns mdns4
    ```

    A:

    ```text
    hosts: files dns
    ```

8. **Comprobación del correcto funcionamiento del servidor DNS:**

    * Usar `dig` para comprobar los registros:

        ```bash
        dig SOA marisma.intranet
        dig NS marisma.intranet
        dig MX marisma.intranet
        dig A www.marisma.intranet
        dig -x 192.168.1.10
        ```

    * Si `ping` o el navegador no resuelven los nombres, seguir las indicaciones de la "Nota" en el enunciado para modificar `/etc/nsswitch.conf`.

---
**Autor:** Alejandro Mateo - [@AlexM2Media](https://github.com/AlexM2Media)  
**Repositorio:** [SREI-2ASIR](https://github.com/AlexM2Media/SREI-2ASIR)  
**Web/Portfolio:** [alexm2.media](https://alexm2.media)
