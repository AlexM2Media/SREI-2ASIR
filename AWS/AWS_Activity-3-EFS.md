# Actividad #4 - Configuración de Amazon EFS

## Objetivo

Montar un sistema de archivos EFS (`wordpress_files`) en instancias EC2 dentro de una VPC preconfigurada y compartir datos entre instancias.

---

## Pasos realizados

### 1. **Creación del sistema de archivos EFS**

   -**Nombre del EFS:** `wordpress_files`
   -**VPC:** Existente (configurada en actividades anteriores)
   -**Subredes:** Pública y privada (montado en todas las AZs)
   -**Security Group:** Habilitar NFS (puerto 2049) desde las instancias EC2.

![Creación EFS](/AWS/.imgs/Act-3/Fig1.png)

---

### 2. **Lanzamiento de instancia Debian**

   -**AMI:** Debian 12
   -**Subnet:** Pública
   -**Security Group:** Permitir SSH (puerto 22) y NFS (puerto 2049).

---

### 3. **Montaje del EFS en la instancia**

   ```bash
   # Instalar cliente NFS
   sudo apt update && sudo apt install nfs-common -y

   # Crear directorios
   mkdir -p /home/wordpress/files
   sudo chown -R admin:admin /home/wordpress

   # Montar EFS (usar DNS del sistema EFS)
   sudo mount -t nfs4 -o nfsvers=4.1,rsize=1048576,wsize=1048576,hard,timeo=600,retrans=2 \ fs-12345678.efs.us-east-1.amazonaws.com:/ /home/wordpress/files
   ```

   ![Montaje EFS](/AWS/.imgs/Act-3/Fig2.png)
   ![Montaje EFS](/AWS/.imgs/Act-3/Fig3.png)

---

#### Comprobaciones finales

- **Acceso SSH:** Conexión exitosa entre instancias.
- **Persistencia de datos:** Archivos creados en una instancia visibles en la otra.
- **Permisos:** Ajustados con `chown/chmod` para permitir escritura.

---
**Autor:** Alejandro Mateo - [@AlexM2Media](https://github.com/AlexM2Media)  
**Repositorio:** [SREI-2ASIR](https://github.com/AlexM2Media/SREI-2ASIR)  
**Web/Portfolio:** [alexm2.media](https://alexm2.media)
