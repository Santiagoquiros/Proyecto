# ACTUALIZACIÓN - INFRAESTRUCTURA DE RED INTEGRADA

## Cambios Realizados

### 1. Carpetas de Configuración de Red Creadas

```
agents/
├── server/
│   └── config_red.md          ← Servidor SOC Central (10.68.0.158)
├── linux/
│   └── config_red.md          ← Ubuntu-Cliente (10.68.0.161)
├── windows/
│   └── config_red.md          ← Windows-Cliente (10.68.0.163)
└── kali_linux.md              ← Kali-Atacante (10.68.0.165)
```

### 2. README Principal Actualizado

Se agregó sección de acceso con:
- URL del Dashboard Wazuh
- Credenciales de acceso
- Información de cada máquina cliente
- Configuración de red completa
- Instrucciones de acceso SSH

### 3. Infraestructura Documentada

**Servidor SOC Central**
- Dirección IP: 10.68.0.158
- Puertos: 1514, 9200, 5601, 5000, 55000
- Servicios: Wazuh Manager, Elasticsearch, Kibana, Logstash
- URL Web: https://wazuh.soc.informatica.iesgrancapitan.org/

**Ubuntu-Cliente**
- Dirección IP: 10.68.0.161
- Usuario SSH: usuario / Root1234$
- Agente Wazuh: Instalado
- Monitorización: Logs, FIM, Rootkits, Vulnerabilidades

**Windows-Cliente**
- Dirección IP: 10.68.0.163
- Usuario SSH: usuario / Root1234$
- Agente Wazuh: Instalado
- Monitorización: Event Logs, FIM, Rootkits, Vulnerabilidades

**Kali-Atacante**
- Dirección IP: 10.68.0.165
- Usuario SSH: kali / Root1234$
- Agente Wazuh: NO instalado
- Propósito: Máquina de pruebas y ataques
- Herramientas: nmap, hydra, tcpdump, wireshark, metasploit

---

## Archivos Creados

### server/config_red.md
**Contenido**:
- Información del servidor SOC
- Configuración de red cloud-init
- Especificaciones técnicas
- Servicios corriendo
- Puertos y direcciones

### linux/config_red.md
**Contenido**:
- Configuración de red Ubuntu
- Configuración YAML
- Credenciales SSH
- Configuración agente Wazuh
- Eventos monitorizados
- Troubleshooting

### windows/config_red.md
**Contenido**:
- Configuración de red Windows
- Configuración PowerShell
- Credenciales SSH
- Configuración agente Wazuh
- Event Logs monitorizados
- Troubleshooting

### kali_linux.md
**Contenido**:
- Configuración de red Kali
- Credenciales SSH
- Herramientas disponibles
- Escenarios de prueba
- Consideraciones éticas
- Tipos de pruebas

---

## Topología de Red

```
┌─────────────────────────────────────────────────────────┐
│         LABORATORIO EDUCATIVO (10.68.0.0/8)             │
├─────────────────────────────────────────────────────────┤
│                                                         │
│  UBUNTU (10.68.0.161)    WINDOWS (10.68.0.163)          │
│  + Agente Wazuh          + Agente Wazuh                 │
│          │                        │                     │
│          └────────────┬───────────┘                     │
│                       │                                 │
│          ┌────────────▼───────────┐                     │
│          │  SERVIDOR SOC (10.68.0.158)                  │
│          │  • Wazuh Manager       │                     │
│          │  • Elasticsearch       │                     │
│          │  • Kibana              │                     │
│          │  • Logstash            │                     │
│          └────────────┬───────────┘                     │
│                       │                                 │
│          KALI (10.68.0.165)                             │
│          (SIN Agente - Pruebas)                         │
│                                                         │
│  Puerta de enlace: 10.0.0.8                             │
│  DNS: 8.8.8.8 / 1.1.1.1                                 │
│                                                         │
└─────────────────────────────────────────────────────────┘
```

---

## Acceso y Credenciales

### Dashboard Wazuh
```
URL: https://wazuh.soc.informatica.iesgrancapitan.org/
Usuario: admin
Contraseña: bgCt6c9pHkPg+Z2OHL*Z.MCiEWo9rTWy
```

### SSH desde Servidor SOC

```bash
# Ubuntu-Cliente
ssh usuario@10.68.0.161
# Contraseña: Root1234$

# Windows-Cliente
ssh usuario@10.68.0.163
# Contraseña: Root1234$

# Kali-Atacante
ssh kali@10.68.0.165
# Contraseña: Root1234$
```

### SSH desde Exterior
```bash
ssh -p 9313 administrador@cpd.informatica.iesgrancapitan.org
# Contraseña: Root1234$
```

---

## Eventos Monitorizados

### Ubuntu-Cliente Monitoriza:
- Autenticación (`/var/log/auth.log`)
- Logs del sistema (`/var/log/syslog`)
- Comandos sudo (`/var/log/sudo.log`)
- Integridad de archivos (FIM)
- Detección de rootkits
- Vulnerabilidades

### Windows-Cliente Monitoriza:
- Event Log Security
- Event Log System
- Event Log Application
- PowerShell Operational
- Integridad de archivos (FIM)
- Detección de rootkits
- Vulnerabilidades

### Kali-Atacante (Genera eventos):
- Escaneos de red (nmap)
- Ataques de fuerza bruta (hydra)
- Captura de tráfico (tcpdump)
- Análisis con Wireshark
- Otros ataques controlados

---

## Casos de Uso

### Caso 1: Detección de Fuerza Bruta
```
1. Kali lanza ataque SSH a Ubuntu (10.68.0.161)
2. Ubuntu agente registra intentos fallidos
3. Servidor SOC recibe logs
4. Elasticsearch indexa datos
5. Kibana muestra alertas en tiempo real
```

### Caso 2: Detección de Cambio de Archivo
```
1. Modificar archivo en Ubuntu o Windows
2. Agente Wazuh detecta cambio (FIM)
3. Servidor recibe evento
4. Alerta se genera automáticamente
5. Visible en dashboard
```

### Caso 3: Escaneo de Red
```
1. Kali ejecuta nmap contra Ubuntu
2. Ubuntu agente registra conexiones
3. Servidor analiza patrones
4. Alerta de escaneo sospechoso
5. Security team notificado
```

---

## Flujo de Datos

```
UBUNTU (10.68.0.161)              WAZUH MANAGER (10.68.0.158)
Agente Wazuh                      │
├─ Auth logs                      ├─► Puerto 1514 TCP/UDP
├─ Syslog                         │
├─ Sudo logs                      ▼
├─ FIM eventos                   ELASTICSEARCH
└─ Rootkit alerts                ├─► Indexa logs
                                  │
WINDOWS (10.68.0.163)            ▼
Agente Wazuh                      LOGSTASH
├─ Security events               ├─► Procesa eventos
├─ System events                 │
├─ Application events            ▼
├─ PowerShell logs               KIBANA
├─ FIM eventos                   ├─► Puerto 5601
└─ Rootkit alerts                ├─► Dashboards
                                  ├─► Alertas en tiempo real
KALI (10.68.0.165)               └─► Análisis forense
(NO agente - Genera eventos)
```

---

## Integración Completa

**Red configurada**: Todas las máquinas pueden comunicarse
**Agentes instalados**: Ubuntu y Windows monitorizan
**Servidor central**: Recibe y procesa todos los eventos
**Dashboards**: Visualización en tiempo real
**Alertas**: Sistema de notificaciones activo
**Pruebas**: Kali lista para generar eventos

---

## Próximos Pasos

1. Verificar conectividad de red (ping)
2. Validar agentes conectados (`agent_control -l`)
3. Acceder a Kibana y revisar dashboards
4. Ejecutar pruebas desde Kali
5. Documentar resultados

---

## Información de Soporte

### Para Problemas de Red:
- Ver archivo `config_red.md` correspondiente
- Verificar IP y puerta de enlace
- Validar DNS (8.8.8.8 / 1.1.1.1)

### Para Problemas de Agente:
- Ver archivo `docs/03_configuracion_agentes.md`
- Verificar estado del servicio
- Revisar logs de agente

### Para Problemas de Acceso:
- Usar credenciales correctas
- Verificar puerto SSH (9313 exterior)
- Validar conectividad a red

---

**Última actualización**: Mayo 2026
**Versión**: 1.1
**Estado**: Infraestructura de red completamente integrada
