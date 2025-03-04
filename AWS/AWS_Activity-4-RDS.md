# Práctica: Configuración de Amazon RDS con MySQL

## Objetivo

Configurar una base de datos privada en Amazon RDS con MariaDB y establecer una conexión segura desde una instancia EC2 pública.

---

## 1. Creación de Infraestructura de Red con CloudFormation

### 1.1 Descargar plantilla de VPC

```url
https://github.com/jose-emilio/aws-academy-fp-daw/blob/main/resources/rds/vpc.yaml
```

### 1.2 Desplegar stack en CloudFormation

| Parámetro               | Valor                   |
|-------------------------|-------------------------|
| Stack name              | `rds-vpc-stack`         |
| VPC Name                | `rds`                   |
| CIDR VPC                | `10.2.0.0/16` (default) |
| Crear NAT Gateways      | NO                      |
| Subredes Públicas       | 10.2.0.0/24, 10.2.1.0/24 |
| Subredes Privadas       | 10.2.2.0/24, 10.2.3.0/24 |

![CloudFormation Stack](/AWS/.imgs/Act-4/Fig1.png)

---

## 2. Configuración de Grupos de Seguridad

### 2.1 Grupo para instancia EC2 (`ec2-sg`)

| Regla                  | Tipo    | Protocolo | Rango de Puertos | Origen       |
|------------------------|---------|-----------|------------------|--------------|
| Entrada                | SSH     | TCP       | 22               | Mi IP        |
| Salida                 | All     | All       | All              | 0.0.0.0/0    |

### 2.2 Grupo para RDS (`rds-sg`)

| Regla                  | Tipo    | Protocolo | Rango de Puertos | Origen       |
|------------------------|---------|-----------|------------------|--------------|
| Entrada                | MySQL   | TCP       | 3306             | `ec2-sg`     |

---

## 3. Creación de Instancia RDS (MariaDB)

### 3.1 Parámetros clave

```text
Engine: MariaDB
Template: Free tier
DB Instance Identifier: db-practica
Credentials:
  Master username: admin
  Master password: adminadmin
Instance Configuration:
  DB Instance Class: db.t4g.small
Storage:
  Type: gp3
  Size: 20 GiB
Connectivity:
  VPC: vpc-rds
  Subnet Group: rds-subnet-group
  Security Group: rds-sg
  Availability Zone: us-east-1a
```

### 3.2 Endpoint de conexión

```url
db-practica.xxxxxxxxx.us-east-1.rds.amazonaws.com:3306
```

![RDS Dashboard](/AWS/.imgs/Act-4/Fig2.png)

---

## 4. Configuración de Instancia EC2

### 4.1 Conexión a RDS

```bash
mysql -u admin -h db-practica.xxxxxxxxx.us-east-1.rds.amazonaws.com -p
# Contraseña: <contraseña>
```

---

## 5. Limpieza de Recursos

1. **Eliminar instancia RDS**
   - Desactivar snapshots finales
2. **Terminar instancia EC2**
3. **Eliminar grupos de seguridad**
4. **Borrar stack de CloudFormation**

---
**Autor:** Alejandro Mateo - [@AlexM2Media](https://github.com/AlexM2Media)  
**Repositorio:** [SREI-2ASIR](https://github.com/AlexM2Media/SREI-2ASIR)  
**Web/Portfolio:** [alexm2.media](https://alexm2.media)
