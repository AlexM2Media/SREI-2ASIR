# Actividad #2 - DNS Teoría

## 1. Servidor DNS autoritativo

**Ventajas:**

- Fuente definitiva de información para un dominio específico
- Control total sobre los registros DNS
- Esencial para organizaciones que gestionan sus propios dominios

**Inconvenientes:**

- Requiere mantenimiento y configuración constante
- Mayor responsabilidad en la gestión de la infraestructura DNS
- Puede ser un objetivo para ataques DDoS

## 2. Servidor DNS recursivo

**Ventajas:**

- Mejora el rendimiento al almacenar en caché las respuestas DNS
- Reduce el tráfico de red hacia servidores DNS externos
- Puede proporcionar filtrado y seguridad adicional

**Inconvenientes:**

- Requiere más recursos de hardware
- Puede ser vulnerable a ataques de envenenamiento de caché
- Necesita actualizaciones regulares para mantener la seguridad

## 3. Servidor DNS de reenvío (Forwarder)

**Ventajas:**

- Reduce la carga en los servidores DNS recursivos internos
- Puede mejorar la velocidad de resolución para consultas externas
- Útil para implementar políticas de seguridad y filtrado

**Inconvenientes:**

- Introduce un punto adicional de fallo en la cadena de resolución DNS
- Puede aumentar la latencia en algunas situaciones
- Dependencia de servidores DNS externos

## 4. Servidor DNS raíz

**Ventajas:**

- Fundamental para el funcionamiento global de Internet
- Proporciona la base para la resolución de nombres de dominio

**Inconvenientes:**

- No es una configuración práctica para la mayoría de las organizaciones
- Requiere una infraestructura y seguridad de alto nivel

## 5. Servidor DNS esclavo (Secondary)

**Ventajas:**

- Proporciona redundancia y balanceo de carga
- Mejora la disponibilidad y el rendimiento del servicio DNS
- Útil para distribuir geográficamente la carga DNS

**Inconvenientes:**

- Requiere sincronización regular con el servidor maestro
- Puede haber retrasos en la propagación de cambios
- Aumenta la complejidad de la gestión DNS

La elección de la configuración DNS dependerá de las necesidades específicas de la organización, considerando factores como el tamaño de la red, los requisitos de rendimiento, la seguridad y los recursos disponibles.

---
**Autor:** Alejandro Mateo - [@AlexM2Media](https://github.com/AlexM2Media)  
**Repositorio:** [SREI-2ASIR](https://github.com/AlexM2Media/SREI-2ASIR)  
**Web/Portfolio:** [alexm2.media](https://alexm2.media)
