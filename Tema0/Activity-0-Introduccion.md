# Actividad #0 - Presentación de la asignatura

## Actividad 0.1 - HTTP Introduction

### Videos de referencia:

- [HTTP Introduction (Part 1)](https://www.youtube.com/watch?v=eesqK59rhGA)

- [HTTP Introduction (Part 2)](https://www.youtube.com/watch?v=DuSURHrZG6I)

### Preguntas y respuestas:

1\. **¿Quién, dónde y cuándo se crea el primer servidor web?**

   Tim Berners-Lee en el CERN en 1990.

2\. **¿Qué es pila de protocolos usados por HTTP?**

   La pila de protocolos típica para HTTP incluye:

   - Capa de aplicación: HTTP

   - Capa de transporte: TCP

   - Capa de red: IP

   - Capa de enlace de datos: Ethernet, Wi-Fi, etc.

3\. **¿Componentes de una URL?**

   - Esquema o protocolo: `https://`

   - Nombre del host o dominio: `www.example.com`

   - Puerto (opcional): `:80` (puerto estándar para HTTP) o `:443` (para HTTPS)

   - Ruta: `/path/to/resource`

   - Parámetros de consulta (opcional): `?query=example`

   - Fragmento (opcional): `#section1`

   Ejemplo completo: `https://www.example.com:443/path/to/resource?query=example#section1`

4\. **¿Pasos en la recuperación de una página web mediante HTTP?**

   1. El navegador resuelve el nombre de dominio a una dirección IP a través del DNS.

   2. Establece una conexión TCP con el servidor web usando la dirección IP resuelta.

   3. Envía una solicitud HTTP (request) al servidor web, normalmente un GET para pedir el contenido.

   4. El servidor procesa la solicitud y responde con un response HTTP, que incluye el código de estado y los datos solicitados.

   5. El navegador renderiza la página web en base a los datos recibidos (HTML, CSS, imágenes, etc.).

5\. **Diferencia entre páginas dinámicas y estáticas**

   Una página web dinámica genera contenido en tiempo real en función de la interacción del usuario. Las páginas web estáticas muestran el mismo contenido para todos los usuarios.

6\. **¿Cómo usar telnet para acceder a un servidor web?**

   1. Conexión: En una terminal escribimos: `telnet www.example.com 80`

   2. Luego, enviamos una solicitud GET:

```
      GET / HTTP/1.1
      Host: www.example.com
```

7\. **Request. Métodos principales**

   - GET: Recupera datos del servidor.

   - POST: Envía datos al servidor.

   - PUT: Reemplaza un recurso en el servidor.

   - DELETE: Elimina un recurso en el servidor.

   - HEAD: Similar a GET, pero solo recupera los encabezados de la respuesta, sin el cuerpo.

   - OPTIONS: Muestra los métodos HTTP que soporta el servidor.

8\. **Response. Códigos**

   1. Respuestas informativas (100--199)

   2. Respuestas satisfactorias (200--299)

   3. Redirecciones (300--399)

   4. Errores de los clientes (400--499)

   5. Errores de los servidores (500--599)

9\. **Content type. Tipos principales**

   - text/html: Páginas web

   - text/css: Estilos

   - application/json: Datos en JSON

   - application/xml: Datos en XML

   - image/jpeg e image/png: Imágenes

   - text/plain: Texto sin formato

## Actividad 0.2 - UDP and TCP: Comparison of Transport Protocols

### Video de referencia:

[UDP and TCP: Comparison of Transport Protocols](https://www.youtube.com/watch?v=Vdc8TCESIg8)

### Preguntas y respuestas:

1\. **Diferencias entre UDP y TCP**

   - TCP es orientado a conexión, UDP no.

   - TCP garantiza la entrega y el orden de los paquetes, UDP no.

   - TCP tiene control de flujo y congestión, UDP no.

   - UDP es más rápido y tiene menor overhead.

2\. **¿Qué aplicaciones usan TCP?**

   HTTP, SMTP, POP, IMAP, SSH

3\. **¿Qué aplicaciones usan UDP?**

   DNS, DHCP, VoIP, juegos en línea

4\. **¿Qué capa almacena el puerto?**

   La capa de transporte (TCP/UDP)

5\. **¿Qué capa almacena la dirección IP?**

   La capa de red (IP)

6\. **¿Qué es three-way handshake?**

   Es el proceso de establecimiento de conexión en TCP que consta de tres pasos:

   1. SYN: El cliente envía un paquete SYN al servidor.

   2. SYN-ACK: El servidor responde con un paquete SYN-ACK.

   3. ACK: El cliente envía un paquete ACK para confirmar la conexión.

## Actividad 0.3 - Práctica telnet/http

### Referencias:

- [Video tutorial](https://www.youtube.com/watch?v=xpBpGC08f4Q&t=189s)

- [Artículo de referencia](http://www.profesordeinformatica.com/servicios/http/telnet)

Sigue las instrucciones del artículo y realiza los ejemplos sugeridos.

**Nota**: Para Windows 10, es necesario activar "telnet". [Instrucciones aquí](http://www.lawebdelprogramador.com/foros/Windows-10/1510815-Como-activar-Telnet-en-Windows-10.html)

## Actividad 0.4 - Usando cUrl

### Referencia:

[Manual de cURL](https://curl.se/docs/manual.html)

Busca información sobre el comando curl y muestra al menos cinco ejemplos de uso.

Ejemplos de uso de cURL:

1\. Descargar una página web:

```
   curl https://www.example.com
```

2\. Descargar un archivo:

```
   curl -O https://example.com/file.zip
```

3\. Enviar datos POST:

```
   curl -X POST -d "param1=value1&param2=value2" https://example.com/api
```

4\. Obtener solo los encabezados de respuesta:

```
   curl -I https://www.example.com
```

5\. Usar autenticación básica:

```
   curl -u username:password https://example.com/api
```

## Actividad 0.5 - Práctica servidor web

1\. Visita los siguientes enlaces:

   - [Simple web server (ejemplo 1)](https://docs.python.org/3/library/http.server.html)

   - [HTTP server (ejemplo 2)](https://github.com/python/cpython/blob/main/Lib/http/server.py)

   - [Dummy web server (ejemplo 3)](https://gist.github.com/kabinpokhrel/6fd1275603e9d5f1e284be717cbd1bff)

2\. Instala Python.
    Es tan fácil como descargarlo desde su [web oficial](https://www.python.org/) o desde la Microsoft Store en sistemas Windows 10 o posterior.

3\. Ejecuta los ejemplos mostrados con anterioridad.

4\. Publica en GitHub los ejemplos llevados a cabo. Los ejemplos se acompañarán con capturas de pantalla en las que se muestre su funcionamiento.

## Actividad 0.5 - Repositorio Github

1\. Crea una cuenta en Github, si no la tienes ya.

2\. Crea un repositorio en Github con el nombre del módulo.

3\. Estructura del repositorio:

   - Carpetas: "Tema0", "Tema1", ..., "TemaN"

   - Archivo README.md en la raíz

4\. El README.md debe tener un aspecto similar a:

```markdown

# Nombre del módulo

Este repositorio incluye actividades llevadas a cabo en el módulo [nombre del módulo]

## Tema 0 - [Nombre tema 0]

| Ejercicio | Descripción |

|-----------|-------------|

| Ejercicio 1 | Breve descripción 0.1 |

| Ejercicio 2 | Breve descripción 0.2 |

| ... | ... |

## Tema 1 - [Nombre tema 1]

| Ejercicio | Descripción |

|-----------|-------------|

| Ejercicio 1 | Breve descripción 1.1 |

| Ejercicio 2 | Breve descripción 1.2 |

| ... | ... |

```

**Nota**: Si no has utilizado antes Github, se recomienda crear un repositorio de prueba llamado "prueba" con una página "README.md" que incluya varias cabeceras, texto, una lista, un gráfico y una tabla. Consulta las siguientes guías:

- [Learn Github](https://github.com/Github-Classroom-Cybros/Learn-Github)

- [Mastering Markdown](https://guides.github.com/features/mastering-markdown/)

---
**Autor:** Alejandro Mateo - [@AlexM2Media](https://github.com/AlexM2Media)  
**Repositorio:** [SREI-2ASIR](https://github.com/AlexM2Media/SREI-2ASIR)  
**Web/Portfolio:** [alexm2.media](https://alexm2.media)