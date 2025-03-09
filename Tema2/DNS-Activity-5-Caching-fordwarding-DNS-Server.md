# Actividad #5 - Servidor DNS Caching/Forwarding con BIND9

## Ejercicio

Sigue las instrucciones del artículo proporcionado para instalar y configurar BIND9 en Ubuntu, primero como un servidor caché y luego como un servidor forwarding.

**Referencias:**

* [Cómo configurar BIND como servidor DNS caché o forwarding en Ubuntu 16.04](https://www.digitalocean.com/community/tutorials/how-to-configure-bind-as-a-caching-or-forwarding-dns-server-on-ubuntu-16-04)
* [Pro DNS and BIND (Chapter 4 DNS types)](http://it-ebooks.info/book/5022/)

### Pasos

1. **Instalación de BIND9:**

    ```bash
    sudo apt update
    sudo apt install bind9 bind9utils bind9-doc
    ```

    ![BIND9 Installation](/Tema2/.imgs/Act-5/Fig1.png)

2. **Configuración como Servidor Caché:**

    * Edita el archivo `/etc/bind/named.conf.options`:

        ```bash
        sudo nano /etc/bind/named.conf.options
        ```

        Asegúrate de que la sección `options` tenga lo siguiente:

        ```text
        options {
            directory "/var/cache/bind";
            recursion yes;
            allow-recursion { any; }; # Cambiar en entornos productivos
            forwarders {
                1.1.1.1;
                1.0.0.1;
            };
            dnssec-validation auto;

            auth-nxdomain no;    # conform to RFC1035
            listen-on-v6 { any; };
        };
        ```

    * El parámetro `recursion yes;` permite las consultas recursivas. En un entorno de producción, es recomendable restringir las IPs que pueden realizar consultas recursivas con `allow-recursion`.
    * El parámetro `forwarders` indica los servidores DNS a los que se reenviarán las consultas en caso de que el servidor no tenga la respuesta en caché. En este ejemplo se usan los servidores de Cloudflare (1.1.1.1 y 1.0.0.1).

3. **Configuración como Servidor Forwarding:**

    * Para configurarlo como servidor forwarding, edita el mismo archivo `/etc/bind/named.conf.options` y asegúrate de que la sección `options` tenga lo siguiente:

        ```text
        options {
            directory "/var/cache/bind";
            recursion no;
            forward only;
            forwarders {
                8.8.8.8;
                1.0.0.1;
            };
            dnssec-validation auto;

            auth-nxdomain no;    # conform to RFC1035
            listen-on-v6 { any; };
        };
        ```

        ![named.conf.options](/Tema2/.imgs/Act-5/Fig2.png)

    * El parámetro `recursion no;` deshabilita las consultas recursivas.
    * El parámetro `forward only;` fuerza a BIND a reenviar todas las consultas a los servidores especificados en `forwarders`.

4. **Comprobación de la sintaxis del archivo de configuración:**

    ```bash
    sudo named-checkconf
    ```

    Si no se muestra ningún error, la sintaxis es correcta.

5. **Reinicio del servicio BIND9:**

    ```bash
    sudo systemctl restart bind9
    ```

6. **Visualización del archivo de registro:**

    ```bash
    sudo tail -f /var/log/syslog
    ```

    Comprueba que el servicio se inicia correctamente y que responde a las consultas DNS.

7. **Comprobación de la resolución DNS:**

    ```bash
    dig @localhost www.example.com
    ```

    Verifica que la respuesta provenga de los servidores configurados en `forwarders`.

---
**Autor:** Alejandro Mateo - [@AlexM2Media](https://github.com/AlexM2Media)  
**Repositorio:** [SREI-2ASIR](https://github.com/AlexM2Media/SREI-2ASIR)  
**Web/Portfolio:** [alexm2.media](https://alexm2.media)
