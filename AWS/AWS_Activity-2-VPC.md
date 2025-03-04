# Amazon VPC: Creación de una infraestructura de red PERSONALIZADA

## Introducción

Amazon Virtual Private Cloud (VPC) permite crear redes virtuales privadas en AWS. Esta práctica muestra cómo configurar una VPC con subredes públicas/privadas y comunicación segura entre instancias EC2.

## Requerimientos

- Acceso a AWS Academy Sandbox

## Configuración de la infraestructura de red

### Creación mediante Wizard

1. Acceder a VPC > Create VPC
![VPS Creation](/AWS/.imgs/Act-2/Fig1.png)
2. Configurar parámetros:

   ```text
   Name tag: wizard
   IPv4 CIDR: 10.2.0.0/16
   2 AZs (us-east-1a y us-east-1b)
   2 subredes públicas (10.2.0.0/24 y 10.2.1.0/24)
   2 subredes privadas (10.2.2.0/24 y 10.2.3.0/24)
   ```

   ![Configuration Wizard](/AWS/.imgs/Act-2/Fig2.png)
   ![Configuration Wizard](/AWS/.imgs/Act-2/Fig3.png)
   ![Configuration Wizard](/AWS/.imgs/Act-2/Fig4.png)

## Lanzamiento de instancias EC2

| Instancia | Subred                     | Security Groups |
|-----------|----------------------------|-----------------|
| Publica   | custom-subnet-public1-1a   | ssh-sg, default |
| Privada   | custom-subnet-private2-1b  | default         |

### Configuración Security Groups

```text
ssh-sg:
  - Entrada: TCP 22 desde MyIP
  - Salida: Todo tráfico

default:
  - Entrada: Todo desde ssh-sg
  - Salida: Todo tráfico
```

## Conexión SSH

1. Conectar a instancia pública:

   ```bash
   ssh -i <key> <usuario>@<ip>
   ```

   ![SSH to Public Instance](/AWS/.imgs/Act-2/Fig5.png)

2. Usando la instancia pública como túnel:

   ```bash
   ssh -i <key> -L <puerto>:<ip_privada>:22 <usuario>@<instancia_pública>
   ```

   ![SSH Tunneling](/AWS/.imgs/Act-2/Fig6.png)

   ```bash
   ssh -i <key> <usuario>@localhost -p <puerto>
   ```

   ![SSH Access via Tunnel](/AWS/.imgs/Act-2/Fig7.png)

## Limpieza

1. Terminar instancias EC2
2. Eliminar VPCs y recursos asociados

---
**Autor:** Alejandro Mateo - [@AlexM2Media](https://github.com/AlexM2Media)  
**Repositorio:** [SREI-2ASIR](https://github.com/AlexM2Media/SREI-2ASIR)  
**Web/Portfolio:** [alexm2.media](https://alexm2.media)
