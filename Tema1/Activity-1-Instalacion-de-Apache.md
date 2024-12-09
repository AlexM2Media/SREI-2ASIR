# Actividad #1 - Instalación de Apache

## 1. La arquitectura Web es un modelo compuesto de tres capas, ¿cuáles son y cuál es la función de cada una de ellas?

La arquitectura Web está compuesta por tres capas principales:

1. **Capa de Presentación**: 
   - Función: Interfaz de usuario
   - Se encarga de la presentación visual de la información
   - Interactúa directamente con el usuario final

2. **Capa de Lógica de Negocio**:
   - Función: Procesamiento de datos
   - Contiene la lógica y reglas del negocio
   - Procesa las solicitudes del usuario y genera respuestas

3. **Capa de Datos**:
   - Función: Almacenamiento y acceso a datos
   - Gestiona la base de datos
   - Almacena y recupera información según las solicitudes de la capa de lógica

## 2. Una plataforma web es el entorno de desarrollo de software empleado para diseñar y ejecutar un sitio web; destacan dos plataformas web, LAMP y WISA. Explica en qué consiste cada una de ellas.

### LAMP (Linux, Apache, MySQL, PHP/Perl/Python)

- Sistema operativo: Linux
- Servidor web: Apache
- Base de datos: MySQL
- Lenguaje de programación: PHP, Perl o Python

LAMP es una plataforma de código abierto, altamente flexible y ampliamente utilizada para el desarrollo web.

### WISA (Windows, IIS, SQL Server, ASP.NET)

- Sistema operativo: Windows
- Servidor web: Internet Information Services (IIS)
- Base de datos: SQL Server
- Lenguaje de programación: ASP.NET

WISA es una plataforma propietaria de Microsoft, ideal para desarrollos en entornos Windows y .NET.

## 3. Lee el siguiente artículo e instala Apache en Ubuntu:

Para instalar Apache en Ubuntu, siga estos pasos:

1. Actualice el índice de paquetes:
   ```bash
   sudo apt update
   ```

2. Instale Apache:
   ```bash
   sudo apt install apache2
   ```

3. Ajuste el firewall para permitir tráfico web:
   ```bash
   sudo ufw allow in "Apache"
   ```

4. Verifique el estado de Apache:
   ```bash
   sudo systemctl status apache2
   ```

5. Acceda a la página predeterminada de Apache en un navegador:
   ```bash
   http://su_direccion_ip
   ```

Para obtener la dirección IP, puede usar el comando:
```bash
hostname -I
```

Nota: Si no dispone de una partición con Linux, puede instalar una máquina virtual siguiendo la guía proporcionada.

---
**Autor:** Alejandro Mateo - [@AlexM2Media](https://github.com/AlexM2Media)  
**Repositorio:** [SREI-2ASIR](https://github.com/AlexM2Media/SREI-2ASIR)  
**Web/Portfolio:** [alexm2.media](https://alexm2.media)