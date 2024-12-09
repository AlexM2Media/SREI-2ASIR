# Actividad #3 - Directivas básicas

## Busca información sobre las siguientes directivas, el valor que toman por defecto y el lugar donde se encuentran definidas

### **Directivas de identificación**

1. **ServerName**:
   - **Descripción**: Define el nombre del servidor o dominio principal asociado al servidor o virtual host.
   - **Valor por defecto**: Ninguno (debe ser configurado manualmente).
   - **Ubicación**: Generalmente en `/etc/apache2/apache2.conf` o en los archivos de configuración de virtual hosts en `/etc/apache2/sites-available/`.

2. **ServerAdmin**:
   - **Descripción**: Dirección de correo electrónico del administrador del servidor.
   - **Valor por defecto**: `webmaster@localhost`.
   - **Ubicación**: En los archivos de configuración de virtual hosts, como `/etc/apache2/sites-available/000-default.conf`.

3. **ServerTokens**:
   - **Descripción**: Controla qué información sobre el servidor se muestra en las respuestas HTTP.
   - **Valor por defecto**: `Full` (muestra información completa del servidor, incluyendo la versión y módulos instalados).
   - **Ubicación**: En `/etc/apache2/conf-enabled/security.conf`.

### **Directivas de localización de ficheros**

1. **DocumentRoot**:
   - **Descripción**: Especifica el directorio raíz desde donde Apache sirve los archivos.
   - **Valor por defecto**: `/var/www/html`.
   - **Ubicación**: En los archivos de configuración de virtual hosts, como `/etc/apache2/sites-available/000-default.conf`.

2. **ErrorLog**:
   - **Descripción**: Define la ubicación del archivo donde se registran los errores del servidor.
   - **Valor por defecto**: `/var/log/apache2/error.log`.
   - **Ubicación**: En los archivos de configuración principal o virtual hosts.

3. **CustomLog**:
   - **Descripción**: Especifica la ubicación y formato del archivo donde se registran las solicitudes HTTP.
   - **Valor por defecto**: No tiene un valor predeterminado; debe configurarse explícitamente.
   - **Ubicación**: En los archivos de configuración principal o virtual hosts.

4. **ServerRoot**:
   - **Descripción**: Define el directorio base donde están ubicados los archivos de configuración y binarios del servidor.
   - **Valor por defecto**: `/etc/apache2` en Ubuntu/Debian.
   - **Ubicación**: En `/etc/apache2/apache2.conf`.

### **Directivas de control de la conexión**

1. **Timeout**:
   - **Descripción**: Tiempo máximo que Apache espera para completar una solicitud.
   - **Valor por defecto**: `300` segundos.
   - **Ubicación**: En `/etc/apache2/apache2.conf`.

2. **KeepAlive**:
   - **Descripción**: Permite mantener conexiones persistentes para reducir la latencia en solicitudes múltiples.
   - **Valor por defecto**: `On`.
   - **Ubicación**: En `/etc/apache2/apache2.conf`.

3. **MaxKeepAliveRequests**:
   - **Descripción**: Número máximo de solicitudes permitidas por conexión persistente.
   - **Valor por defecto**: `100`.
   - **Ubicación**: En `/etc/apache2/apache2.conf`.

4. **KeepAliveTimeout**:
   - **Descripción**: Tiempo que Apache espera entre solicitudes en una conexión persistente antes de cerrarla.
   - **Valor por defecto**: `5` segundos.
   - **Ubicación**: En `/etc/apache2/apache2.conf`.

### **Otras directivas**

1. **LogLevel**:
   - **Descripción**: Define el nivel de detalle para los mensajes registrados en el archivo de errores.
   - **Valores posibles (por orden ascendente)**:
     `debug`, `info`, `notice`, `warn`, `error`, `crit`, `alert`, `emerg`.
   - **Valor por defecto**: `warn`.
   - **Ubicación**: En `/etc/apache2/apache2.conf` o en configuraciones específicas.

2. **LogFormat**:
   - **Descripción**: Especifica el formato utilizado para registrar las solicitudes HTTP en los archivos de log.
   - **Valor por defecto (Common Log Format)**:
     ```text
     "%h %l %u %t \"%r\" %>s %b"
     ```
     Donde:
     - `%h`: Dirección IP del cliente
     - `%l`: Nombre del usuario remoto
     - `%u`: Usuario autenticado
     - `%t`: Fecha y hora
     - `%r`: Solicitud realizada
     - `%>s`: Código HTTP devuelto
     - `%b`: Tamaño del cuerpo de la respuesta
   - **Ubicación**: En `/etc/apache2/apache2.conf` o configuraciones específicas.

---
**Autor:** Alejandro Mateo - [@AlexM2Media](https://github.com/AlexM2Media)  
**Repositorio:** [SREI-2ASIR](https://github.com/AlexM2Media/SREI-2ASIR)  
**Web/Portfolio:** [alexm2.media](https://alexm2.media)
