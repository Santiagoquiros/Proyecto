# Arquitectura del SOC Educativo - Wazuh + ELK

## Diagrama de Arquitectura General

```
┌─────────────────────────────────────────────────────────────────┐
│                    LABORATORIO EDUCATIVO                         │
├─────────────────────────────────────────────────────────────────┤
│                                                                   │
│  ┌──────────────┐  ┌──────────────┐  ┌──────────────┐           │
│  │  Linux Host  │  │  Linux Host  │  │ Windows Host │           │
│  │    Agent 1   │  │    Agent 2   │  │    Agent 3   │           │
│  └──────┬───────┘  └──────┬───────┘  └──────┬───────┘           │
│         │                 │                  │                   │
│         └─────────────────┼──────────────────┘                   │
│                           │                                       │
│                   ┌───────▼─────────┐                            │
│                   │  Firewall/Red   │                            │
│                   │    Privada      │                            │
│                   └───────┬─────────┘                            │
│                           │                                      │
│         ┌─────────────────┘                                      │
│         │                                                        │
│    ┌────▼──────────────────────────────────────────────────┐     │
│    │         SERVIDOR CENTRAL - SOC                        │     │
│    ├───────────────────────────────────────────────────────┤     │
│    │                                                       │     │
│    │  ┌──────────────────┐         ┌──────────────────┐    │     │
│    │  │  Wazuh Manager   │◄────────┤  Wazuh DB        │    │     │
│    │  │  (Recepción,     │         │  (sqlite3)       │    │     │
│    │  │   Análisis,      │         └──────────────────┘    │     │
│    │  │   Alertas)       │                                 │     │
│    │  └────────┬─────────┘                                 │     │
│    │           │                                           │     │
│    │           │ Logs JSON                                 │     │
│    │           │                                           │     │
│    │    ┌──────▼──────────────────────────────────────┐    │     │
│    │    │        ELK STACK                            │    │     │
│    │    ├─────────────────────────────────────────────┤    │     │
│    │    │                                             │    │     │
│    │    │  ┌─────────────┐   ┌───────────────────┐    │    │     │
│    │    │  │  Logstash   │   │  Elasticsearch    │    │    │     │
│    │    │  │ (Procesa    │──▶│  (Almacena Logs)  │    │    │    │     
│    │    │  │  Logs)      │   └────────┬──────────┘    │    │     │
│    │    │  └─────────────┘            │               │    │     │
│    │    │                             │               │    │     │
│    │    │              ┌──────────────▼────────────┐  │    │     │
│    │    │              │   Kibana                  │  │    │     │
│    │    │              │ (Visualización,           │  │    │     │
│    │    │              │  Dashboards,              │  │    │     │
│    │    │              │  Análisis)                │  │    │     │
│    │    │              └──────────────────────────┘│  │    │     │
│    │    └──────────────────────────────────────────┘  │    │     │
│    │                                                  │    │     │
│    └──────────────────────────────────────────────────┘    │     │
│         (Ubuntu Server 24.04)                              │     │
│                                                            │     │
└──────────────────────────────────────────────────────────────────┘    
```

## Especificación de Máquinas Virtuales

### VM1: Servidor SOC (Wazuh + Elastic Stack)

| Recurso | Mínimo | Recomendado |
|---------|--------|-------------|
| Sistema Operativo | Ubuntu Server 24.04 LTS |
| CPU | 4 vCPU | 6-8 vCPU |
| RAM | 8 GB | 16 GB |
| Almacenamiento | 100 GB SSD | 200-300 GB SSD |

**Software instalado**:
- Wazuh Manager (gestor central)
- Elasticsearch / OpenSearch (almacenamiento indexado)
- Kibana (visualización)
- Logstash (procesamiento de logs)
- Filebeat (recolector ligero)
- Java 11+
- NTP

**Funciones principales**:
- Centralización de logs
- Correlación de eventos
- Detección de incidentes (SIEM)
- Visualización y análisis
- API para integración

---

### VM2: Cliente Vulnerable Linux

| Recurso | Mínimo | Recomendado |
|---------|--------|-------------|
| Sistema Operativo | Ubuntu Server 22.04 |
| CPU | 2 vCPU | 2-4 vCPU |
| RAM | 2 GB | 4 GB |
| Almacenamiento | 30 GB | 40-60 GB |

**Objetivo**: Simular vulnerabilidades comunes y generar eventos de seguridad detectables

**Servicios y puertos vulnerables configurados**:

| Servicio | Puerto | Vulnerabilidad Educativa |
|----------|--------|-------------------------|
| SSH | 22 | Fuerza bruta |
| FTP (vsftpd) | 21 | Acceso anónimo |
| Apache2 | 80 | Directorios expuestos |
| MySQL | 3306 | Credenciales débiles |
| Samba | 445 | Shares inseguros |

**Software instalado**:
- Wazuh Agent
- Apache2
- vsftpd
- MySQL
- Samba

---

### VM3: Cliente Vulnerable Windows

| Recurso | Mínimo | Recomendado |
|---------|--------|-------------|
| Sistema Operativo | Windows 10 |
| CPU | 2 vCPU | 4 vCPU |
| RAM | 4 GB | 8 GB |
| Almacenamiento | 50 GB | 80-100 GB |

**Objetivo**: Simular un endpoint corporativo con malas configuraciones

**Servicios y puertos vulnerables configurados**:

| Servicio | Puerto | Vulnerabilidad Educativa |
|----------|--------|-------------------------|
| RDP | 3389 | Fuerza bruta |
| SMB | 445 | Recursos inseguros |
| WinRM | 5985 | Mala configuración |
| IIS | 80 | Aplicación vulnerable |

**Software instalado**:
- Wazuh Agent
- Sysmon (telemetría avanzada)
- IIS (Internet Information Services)
- PowerShell (scripting y automatización)



## Flujo de Datos

```
Agentes Wazuh                  Servidor Central                Visualización
════════════════              ══════════════════              ═══════════════

Evento del SO  ─┐
                ├─► Agente Wazuh ──(JSON)──► Wazuh Manager ──(JSON)──► Logstash
Cambios de     │               (Puerto 1514)                    (Puerto 5000)
archivo        │
               │
Logs de auth   │
               │
Malware scan   │
               
                              Reglas & Alertas
                              ════════════════
                              - Intentos fallidos
                              - Cambios críticos
                              - Anomalías
                              - Eventos sospechosos
                              
                              Elasticsearch
                              (Índices:
                               wazuh-alerts-*
                               wazuh-events-*)
                              
                                                           Kibana Dashboard
                                                           - Gráficos en tiempo real
                                                           - Mapa de alertas
                                                           - Timeline de eventos
                                                           - Análisis forense
```

## Componentes Detallados

### 1. Wazuh Manager

**Función**: Núcleo de recolección, análisis y alerting de seguridad.

**Características**:
- Gestión centralizada de agentes
- Base de datos de reglas y decodificadores
- Motor de alertas con correlación de eventos
- API para integración externa
- Auditoría y logging interno

**Servicios principales**:
- `wazuh-manager` (puerto 1514 UDP/TCP)
- `wazuh-indexer` (puerto 9200)
- `wazuh-dashboard` (puerto 443)

### 2. Agentes Wazuh

**Ubicación**: Instalados en cada servidor/estación monitorizada

**Funciones**:
- Recolección de logs del sistema
- Monitorización de integridad de archivos (FIM)
- Detección de rootkits
- Análisis de vulnerabilidades
- Envío cifrado de datos al Manager

**Sistemas soportados**:
- Linux (Ubuntu, CentOS, Debian, Red Hat)
- Windows (Server 2008+, Desktop 7+)
- macOS

### 3. ELK Stack

#### Elasticsearch
- **Puerto**: 9200 (HTTP), 9300 (node communication)
- **Función**: Base de datos de búsqueda y análisis
- **Indices**: `wazuh-alerts-*`, `wazuh-events-*`
- **Almacenamiento**: Mínimo 100GB para laboratorio

#### Logstash
- **Puerto**: 5000 (entrada), 5001 (salida)
- **Función**: Procesa y enriquece logs de Wazuh
- **Pipelines**:
  - Entrada: JSON desde Wazuh Manager
  - Filtros: Parsing, enriquecimiento, clasificación
  - Salida: Elasticsearch

#### Kibana
- **Puerto**: 5601 (HTTP)
- **URL**: `http://localhost:5601`
- **Función**: Visualización de datos y creación de dashboards
- **Dashboards temáticos**:
  - Seguridad del SO
  - Autenticación y acceso
  - Eventos críticos
  - Timeline de incidentes

## Canales de Comunicación

| Origen | Destino | Puerto | Protocolo | Datos |
|--------|---------|--------|-----------|-------|
| Agente Wazuh | Wazuh Manager | 1514 | TCP/UDP | Logs del agente |
| Wazuh Manager | Elasticsearch | 9200 | HTTP | Índices |
| Logstash | Elasticsearch | 9200 | HTTP | Logs procesados |
| Kibana | Elasticsearch | 9200 | HTTP | Consultas |
| Admin | Kibana | 5601 | HTTP | Visualización |
| Admin | Wazuh API | 55000 | HTTPS | Gestión |

## Tipos de Eventos Monitorizados

### Sistema Operativo
- Inicio/cierre de sesión
- Cambios de usuario y permisos
- Instalación/desinstalación de software
- Cambios en configuración del sistema

### Seguridad
- Intentos de acceso fallidos
- Actividad de firewall
- Cambios en policies de seguridad
- Detección de rootkits

### Archivos y Cambios
- Modificación de archivos críticos (/etc, /bin, C:\Windows)
- Cambios en permisos
- Creación de nuevos usuarios
- Cambios en sudoers

### Red
- Conexiones de red inusuales
- Puertos abiertos nuevos
- Tráfico sospechoso (opcional con Suricata/Snort)

### Aplicaciones
- Errores críticos
- Acceso denegado
- Eventos de auditoría de aplicaciones

## Infraestructura Mínima

### Servidor Central
- **OS**: Ubuntu Server 24.04 LTS / CentOS 8+
- **RAM**: 8-16 GB
- **CPU**: 4+ cores
- **Almacenamiento**: 100-200 GB
- **Red**: IP estática, acceso a internet

### Agentes
- **RAM**: 512 MB - 2 GB
- **Almacenamiento**: 500 MB
- **Ancho de banda**: 100 Kbps - 1 Mbps por agente

### Máquinas de Prueba
- 2-3 servidores Linux (Ubuntu/CentOS)
- 1 servidor Windows (Server 2019+)
- 1 firewall virtual (pfSense, OPNsense)

## Integración de Componentes

```
┌──────────────────────────────────────────────────┐
│         FLUJO DE PROCESAMIENTO DE LOGS            │
└──────────────────────────────────────────────────┘

Agente Wazuh (Sistema)
    │ Recolecta eventos
    ▼
Agente Wazuh (Cliente)
    │ Cifra y comprime
    ▼
Wazuh Manager (Receptor)
    │ Decodifica y aplica reglas
    ▼
Base de datos Wazuh
    │ Almacena eventos
    ▼
JSON a puertos 1514/TCP
    │ Envía a Logstash
    ▼
Logstash
    │ Parsea, enriquece, clasifica
    ▼
Elasticsearch
    │ Indexa logs
    ▼
Kibana
    │ Visualiza información
    ▼
Dashboard en Tiempo Real
```

## Escalabilidad

Para ambiente de producción:

- **Cluster Elasticsearch**: Múltiples nodos
- **Logstash múltiple**: Balanceo de carga
- **Wazuh Cluster**: Redundancia y failover
- **ELK Stack redundante**: Alta disponibilidad
- **Almacenamiento externo**: NAS/SAN

## Notas de Diseño

1. **Aislamiento**: Los agentes se comunican solo con Wazuh Manager
2. **Seguridad**: Comunicación cifrada entre componentes
3. **Escalabilidad**: Arquitectura modular permite añadir agentes
4. **Educativa**: Diseñada para claridad y entendimiento
5. **Monitorizable**: Logs de todos los componentes

---

Para más información técnica, consulta la documentación en `/docs`
**Última actualización**: Mayo 2026
