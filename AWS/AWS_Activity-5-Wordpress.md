# Instalación de WordPress en EC2 con RDS y EFS

## 1. Configuración Inicial de Red

### 1.1 Requisitos de VPC

- 2 subredes públicas
- 2 subredes privadas
- Configuración mediante asistente VPC

## 2. Implementación de Instancia EC2

### 2.1 Parámetros Clave

| Parámetro        | Valor                  |
|------------------|------------------------|
| AMI              | Debian 12/Ubuntu 22.04 |
| Nombre           | `servidorwordpress`    |
| Tipo de Instancia| t4g.micro              |
| Grupo de Seguridad| `seguridadwordpress`   |

### 2.2 Reglas de Seguridad

```bash
# Conexión SSH
ssh -i <clave> <user>@<instance>
```

## 3. Instalación de Servicios Web

### 3.1 Apache

```bash
sudo apt update && sudo apt upgrade -y
sudo apt install apache2 -y
sudo systemctl enable --now apache2
```

![Instalación Apache2](/AWS/.imgs/Act-5/Fig1.png)

### 3.2 PHP y Extensiones

```bash
sudo apt install php libapache2-mod-php php-mysql php-cli -y
sudo systemctl restart apache2
```

![Instalación Apache2](/AWS/.imgs/Act-5/Fig2.png)

## 4. Configuración de RDS (MySQL)

### 4.1 Parámetros de Base de Datos

| Parámetro         | Valor                |
|-------------------|----------------------|
| Motor             | MySQL 8.0            |
| Identificador     | `bdwordpress`        |
| Clase             | db.t3.micro          |
| Almacenamiento    | 20 GB gp3            |
| Usuario           | admin                |

![Conexión RDS a EC2](/AWS/.imgs/Act-5/Fig3.png)

### 4.2 Conectividad

```bash
mysql -u admin -h <> -p
```

```sql
CREATE DATABASE wordpress;
CREATE USER 'wpuser'@'%' IDENTIFIED BY 'password123';
GRANT ALL PRIVILEGES ON wordpress.* TO 'wpuser'@'%';
FLUSH PRIVILEGES;
```

![Conexión del RDS y creación de la DB](/AWS/.imgs/Act-5/Fig4.png)

## 5. Configuración de EFS

### 5.1 Creación de Sistema de Archivos

| Parámetro         | Valor                |
|-------------------|----------------------|
| Nombre            | `almacenwordpress`   |
| VPC               | Misma que EC2        |
| Protocolo         | NFSv4                |

### 5.2 Montaje en EC2

```bash
sudo apt install nfs-common -y
sudo mkdir /mnt/efs
sudo mount -t nfs4 :/ /mnt/efs
```

## 6. Instalación de WordPress

### 6.1 Descarga y Configuración

```bash
cd /var/www/html
sudo wget https://wordpress.org/latest.tar.gz
sudo tar -xzf latest.tar.gz
sudo mv wordpress/* .
sudo rm -rf wordpress latest.tar.gz
```

![Descarga Wordpress](/AWS/.imgs/Act-5/Fig5.png)

### 6.2 Vinculación con EFS

```bash
sudo mv wp-content wp-content-old
sudo ln -s /mnt/efs/wp-content wp-content
sudo chown -R www-data:www-data /mnt/efs/wp-content
```

### 6.3 Configuración Final

1. Acceder a `http://<aws-ec2-instance>/[wordpress]`
2. Completar formulario con:
   - **Database Name**: `wordpress`
   - **Username**: `wpuser`
   - **Password**: `password123`
   - **Host**: ``

![Instalación Wordpress](/AWS/.imgs/Act-5/Fig6.png)
![Instalación Wordpress](/AWS/.imgs/Act-5/Fig7.png)

## 7. Gestión de Seguridad

### 7.1 Grupos de Seguridad

| Recurso          | Reglas Entrada               |
|------------------|------------------------------|
| EC2              | SSH (22), HTTP (80)          |
| RDS              | MySQL (3306) desde grupo EC2 |
| EFS              | NFS (2049) desde grupo EC2   |

## 8. Limpieza de Recursos

1. Terminar instancia EC2
2. Eliminar instancia RDS
3. Eliminar sistema EFS
4. Eliminar grupos de seguridad

---
**Autor:** Alejandro Mateo - [@AlexM2Media](https://github.com/AlexM2Media)  
**Repositorio:** [SREI-2ASIR](https://github.com/AlexM2Media/SREI-2ASIR)  
**Web/Portfolio:** [alexm2.media](https://alexm2.media)
