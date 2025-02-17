# Actividad #1 - DNS Teoría

## Preguntas y Respuestas

### 1. ¿Qué es TLD? ¿Cómo se clasifican los dominios de nivel superior? Pon algunos ejemplos

TLD significa Top-Level Domain (Dominio de Nivel Superior). Es la última parte de un nombre de dominio, ubicada después del último punto.

Los dominios de nivel superior se clasifican principalmente en:

- gTLD (Generic Top-Level Domains): Dominios genéricos como .com, .org, .net, .media, ...
- ccTLD (Country Code Top-Level Domains): Dominios de países como .es (España), .fr (Francia), .uk (Reino Unido), .de (Alemania), ...
- sTLD (Sponsored Top-Level Domains): Dominios patrocinados como .edu, .gov, .mil, .cat, ...
- New gTLD: Nuevos dominios genéricos como .blog, .app, .shop, ...
- Infrastructure: .arpa

Ejemplos:

- gTLD: .com, .org, .net
- ccTLD: .es, .fr, .de
- sTLD: .edu, .gov, .mil
- New gTLD: .blog, .app, .shop

### 2. ¿Qué es FQDN? Pon algún ejemplo de FQDN

FQDN significa Fully Qualified Domain Name (Nombre de Dominio Completamente Calificado). Es el nombre de dominio completo que especifica la ubicación exacta en la jerarquía del DNS.

Ejemplos de FQDN:

- www.ejemplo.com
- mail.google.com
- es.wikipedia.org

### 3. ¿Qué son los root servers? ¿Cuántos root servers hay? ¿Cuántos servidores raíz físicos existen y dónde se encuentran? ¿Qué es anycast?

Los root servers (servidores raíz) son servidores de nombres que operan en la zona raíz del Sistema de Nombres de Dominio (DNS). Son el primer paso en la traducción de nombres de dominio legibles por humanos a direcciones IP.

Hay 13 root servers lógicos, etiquetados de la A a la M. Sin embargo, existen cientos de servidores físicos distribuidos por todo el mundo que utilizan la tecnología anycast.

Anycast es una técnica de enrutamiento de red donde los datos se envían al nodo más cercano en un grupo de receptores potenciales que se identifican por el mismo destino.

### 4. ¿Qué es un archivo de zona (zone file)? Indica para qué sirven los registros de un archivo de zona. Pon un ejemplo de un archivo de zona e interpreta la información almacenada

Un archivo de zona es un archivo de texto que describe un dominio DNS y contiene todos los registros para ese dominio. Los registros en un archivo de zona sirven para mapear nombres de dominio a direcciones IP y proporcionar información sobre el dominio.

Ejemplo de un archivo de zona simplificado:

``` dns
$TTL 86400
@ IN SOA ns1.ejemplo.com. admin.ejemplo.com. (
2023010101 ; Serial
3600 ; Refresh
1800 ; Retry
604800 ; Expire
86400 ) ; Minimum TTL

@ IN NS ns1.ejemplo.com.
@ IN NS ns2.ejemplo.com.
@ IN A 192.168.1.10
www IN A 192.168.1.20
mail IN A 192.168.1.30
@ IN MX 10 mail.ejemplo.com.
```

Interpretación:

- La primera línea define el TTL (Time To Live) predeterminado.
- El registro SOA (Start of Authority) define la información autoritativa sobre la zona.
- Los registros NS definen los servidores de nombres para el dominio.
- Los registros A mapean nombres de host a direcciones IP.
- El registro MX define el servidor de correo para el dominio.

---
**Autor:** Alejandro Mateo - [@AlexM2Media](https://github.com/AlexM2Media)  
**Repositorio:** [SREI-2ASIR](https://github.com/AlexM2Media/SREI-2ASIR)  
**Web/Portfolio:** [alexm2.media](https://alexm2.media)
