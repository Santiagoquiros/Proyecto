# SOC EDUCATIVO CON WAZUH Y ELASTIC STACK

## 1. Introducción

Este proyecto corresponde a un **TFG** cuyo objetivo es el diseño e implementación de un **SOC (Security Operations Center) educativo**, orientado a ciclos formativos de **Grado Superior (ASIR / DAW / DAM)** con especialización en **seguridad informática**.

El entorno permite **simular ataques controlados**, **recoger evidencias**, **analizarlas mediante Wazuh + Elastic Stack** y **formar al alumnado en detección, análisis y respuesta ante incidentes**, replicando el funcionamiento de un SOC real.

---

## 2. Arquitectura general

### Máquinas virtuales

| Máquina | Rol | Sistema Operativo |
|------|----|------------------|
| VM1 | Servidor SOC | Ubuntu Server 22.04 LTS |
| VM2 | Cliente vulnerable Linux | Ubuntu Server 20.04 / 22.04 |
| VM3 | Cliente vulnerable Windows | Windows 10 / Windows Server 2019 |

### Topología de red
- Red interna aislada (NAT o Host-only)
- El servidor Wazuh no expuesto a Internet
- Comunicación segura agente → servidor

---

## 3. Servidor principal Wazuh + Elastic

### Funciones
- Centralización de logs
- Correlación de eventos
- Detección de incidentes (SIEM)
- Visualización y análisis

### Requisitos hardware

| Recurso | Mínimo | Recomendado |
|------|------|-------------|
| CPU | 4 vCPU | 6–8 vCPU |
| RAM | 8 GB | 16 GB |
| Almacenamiento | 100 GB SSD | 200–300 GB SSD |

### Puertos utilizados

| Puerto | Protocolo | Uso |
|-----|---------|----|
| 1514 | UDP/TCP | Recepción de logs |
| 1515 | TCP | Registro de agentes |
| 55000 | TCP | API Wazuh |
| 9200 | TCP | ElasticSearch |
| 5601 | TCP | Kibana |
| 22 | TCP | Administración SSH |

### Software
- Wazuh Manager
- ElasticSearch / OpenSearch
- Filebeat
- Kibana
- Java 11+
- NTP

---

## 4. Cliente vulnerable Linux

### Objetivo
Simular vulnerabilidades comunes y generar eventos de seguridad detectables.

### Requisitos hardware

| Recurso | Mínimo | Recomendado |
|------|------|-------------|
| CPU | 2 vCPU | 2–4 vCPU |
| RAM | 2 GB | 4 GB |
| Almacenamiento | 30 GB | 40–60 GB |

### Servicios y puertos vulnerables

| Servicio | Puerto | Vulnerabilidad educativa |
|------|------|--------------------------|
| SSH | 22 | Fuerza bruta |
| FTP | 21 | Acceso anónimo |
| Apache | 80 | Directorios expuestos |
| MySQL | 3306 | Credenciales débiles |
| Samba | 445 | Shares inseguros |

### Software
- Wazuh Agent
- Apache2
- vsftpd
- MySQL
- Samba

---

## 5. Cliente vulnerable Windows

### Objetivo
Simular un endpoint corporativo con malas configuraciones.

### Requisitos hardware

| Recurso | Mínimo | Recomendado |
|------|------|-------------|
| CPU | 2 vCPU | 4 vCPU |
| RAM | 4 GB | 8 GB |
| Almacenamiento | 50 GB | 80–100 GB |

### Servicios y puertos vulnerables

| Servicio | Puerto | Vulnerabilidad educativa |
|------|------|--------------------------|
| RDP | 3389 | Fuerza bruta |
| SMB | 445 | Recursos inseguros |
| WinRM | 5985 | Mala configuración |
| IIS | 80 | Aplicación vulnerable |

### Software
- Wazuh Agent
- Sysmon
- IIS
- PowerShell

---

## 6. Escenarios de ataque

Los ataques son **simulados y controlados**, con enfoque defensivo.

1. Reconocimiento y escaneo de red
2. Fuerza bruta sobre SSH y RDP
3. Acceso inicial con credenciales válidas
4. Escalada de privilegios
5. Persistencia
6. Movimiento lateral
7. Ejecución de malware simulado
8. Exfiltración de información simulada

Todos los escenarios están alineados con **MITRE ATT&CK**.

---

## 7. Prácticas para el alumnado

### Práctica 1 – Reconocimiento
- Análisis de conexiones múltiples
- Identificación de escaneos

### Práctica 2 – Fuerza bruta
- Detección de intentos fallidos
- Análisis de eventos 4625 y logs SSH

### Práctica 3 – Accesos sospechosos
- Correlación login fallido → exitoso
- Detección de anomalías horarias

---

## 8. Relación Ataque – Wazuh – MITRE

| Escenario | Técnica MITRE | Evento |
|--------|-------------|--------|
| Escaneo | T1046 | Network scan |
| Fuerza bruta | T1110 | Auth fail |
| Login sospechoso | T1078 | Valid accounts |
| Escalada | T1068 | Privilege escalation |
| Persistencia | T1053 | Scheduled task |
| Movimiento lateral | T1021 | Remote services |
| Ejecución | T1059 | Script execution |

---

## 9. Dashboards Kibana

- Visión general del SOC
- Autenticación y accesos
- Actividad sospechosa en endpoints
- Persistencia y cambios
- MITRE ATT&CK

---

## 10. Justificación académica

Este SOC educativo:
- Replica un SOC profesional
- Fomenta mentalidad defensiva
- Permite análisis forense y correlación
- Cumple principios éticos y legales
- Prepara al alumnado para entornos reales

---

## 11. Escalabilidad futura

- Integración con Suricata
- Alertas por correo
- Dashboards personalizados
- Simulación de ransomware sin cifrado
- Ampliación a más clientes

---

## 12. Conclusión

El proyecto demuestra la viabilidad de un **SOC educativo funcional**, alineado con estándares profesionales, y constituye una base sólida para la formación en **ciberseguridad defensiva** dentro de la Formación Profesional.

