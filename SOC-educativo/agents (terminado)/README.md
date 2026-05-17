# Configuración de Agentes Wazuh e Infraestructura de Red

## Introducción

Este directorio contiene la configuración de red y los archivos de configuración para todos los componentes de la infraestructura del SOC educativo.

## Topología de Red

```
┌─────────────────────────────────────────────────────────────┐
│                    LABORATORIO EDUCATIVO                    │
│              Subred: 10.68.0.0/8 (255.0.0.0)                │
├─────────────────────────────────────────────────────────────┤
│                                                             │
│  ┌──────────────────┐  ┌──────────────────┐                 │
│  │  Ubuntu-Cliente  │  │ Windows-Cliente  │                 │
│  │  IP: 10.68.0.161 │  │ IP: 10.68.0.163  │                 │
│  │  Agente Wazuh    │  │ Agente Wazuh     │                 │
│  └────────┬─────────┘  └────────┬─────────┘                 │
│           │                     │                           │
│  ┌────────┴─────────────────────┴────────┐                  │
│  │                                       │                  │
│  │    ┌──────────────────────────────┐   │                  │
│  │    │   SERVIDOR SOC CENTRAL       │   │                  │
│  │    │   IP: 10.68.0.158            │   │                  │
│  │    │                              │   │                  │
│  │    │  ├─ Wazuh Manager            │   │                  │
│  │    │  ├─ Elasticsearch            │   │                  │
│  │    │  ├─ Logstash                 │   │                  │
│  │    │  └─ Kibana                   │   │                  │
│  │    └──────────────────────────────┘   │                  │
│  │                                       │                  │
│  └────────────────────┬──────────────────┘                  │
│                       │                                     │
│  ┌────────────────────┴────────────────┐                    │
│  │                                     │                    │
│  │   Kali-Atacante (Pruebas)           │                    │
│  │   IP: 10.68.0.165                   │                    │
│  │   (SIN Agente Wazuh)                │                    │
│  │                                     │                    │
│  └─────────────────────────────────────┘                    │
│                                                             │
│            Puerta de enlace: 10.0.0.8                       │
│            DNS: 8.8.8.8 / 1.1.1.1                           │
│                                                             │
└─────────────────────────────────────────────────────────────┘
```

## Estructura de Archivos

```
agents/
├── README.md                    ← Este archivo
├── server/
│   └── config_red.md            ← Configuración Servidor SOC
├── linux/
│   └── config_red.md            ← Configuración Ubuntu-Cliente
├── windows/
│   └── config_red.md            ← Configuración Windows-Cliente
└── kali_linux.md                ← Configuración Kali-Atacante
```

## Direcciones IP y Puertos

### Servidor SOC Central
- **IP**: 10.68.0.158
- **Servicios**:
  - Wazuh Manager: Puerto 1514 (TCP/UDP)
  - Elasticsearch: Puerto 9200
  - Kibana: Puerto 5601
  - Logstash: Puerto 5000
  - API Wazuh: Puerto 55000

### Ubuntu-Cliente
- **IP**: 10.68.0.161
- **SO**: Ubuntu Server 20.04 LTS
- **Agente**: Wazuh instalado
- **Acceso SSH**: usuario@10.68.0.161

### Windows-Cliente
- **IP**: 10.68.0.163
- **SO**: Windows Server 2019+
- **Agente**: Wazuh instalado
- **Acceso SSH**: usuario@10.68.0.163

### Kali-Atacante
- **IP**: 10.68.0.165
- **SO**: Kali Linux
- **Propósito**: Pruebas de seguridad
- **Acceso SSH**: kali@10.68.0.165
- **Nota**: NO ejecuta agente Wazuh

## Credenciales de Acceso

### SSH General (Todos los clientes)
```
Contraseña: Root1234$
```

### Wazuh Dashboard
```
Usuario: admin
Contraseña: bgCt6c9pHkPg+Z2OHL*Z.MCiEWo9rTWy
```

### Acceso Remoto Exterior
```
SSH: ssh -p 9313 administrador@cpd.informatica.iesgrancapitan.org
Contraseña: Root1234$
```

## Configuración de Red Detallada

### Configuración Común
```yaml
version: 2
renderer: networkd
nameservers:
  addresses:
    - 8.8.8.8
    - 1.1.1.1
gateway4: 10.0.0.8
```

### Red por Máquina

**Servidor SOC**:
- IP: 10.68.0.158/8
- Ver: [server/config_red.md](server/config_red.md)

**Ubuntu-Cliente**:
- IP: 10.68.0.161/8
- Ver: [linux/config_red.md](linux/config_red.md)

**Windows-Cliente**:
- IP: 10.68.0.163/8
- Ver: [windows/config_red.md](windows/config_red.md)

**Kali-Atacante**:
- IP: 10.68.0.165/8
- Ver: [kali_linux.md](kali_linux.md)

## Eventos Monitorizados por Agente

### Ubuntu-Cliente Monitoriza:
- Logs de autenticación (`/var/log/auth.log`)
- Logs del sistema (`/var/log/syslog`)
- Logs de sudo (`/var/log/sudo.log`)
- Integridad de archivos (FIM)
- Detección de rootkits
- Vulnerabilidades del sistema

### Windows-Cliente Monitoriza:
- Event Log Security
- Event Log System
- Event Log Application
- PowerShell Operational Log
- Integridad de archivos (FIM)
- Detección de rootkits
- Vulnerabilidades del sistema

### Kali-Atacante:
- **NO ejecuta agente Wazuh**
- Se usa para generar eventos que otros agentes detectan
- Herramientas para pruebas: nmap, hydra, tcpdump, etc.

## Tareas Comunes

### Conectarse a Ubuntu-Cliente
```bash
ssh usuario@10.68.0.161
# Contraseña: Root1234$
```

### Conectarse a Windows-Cliente
```bash
ssh usuario@10.68.0.163
# Contraseña: Root1234$
```

### Conectarse a Kali-Atacante
```bash
ssh kali@10.68.0.165
# Contraseña: Root1234$
```

### Ver Estado de Agentes (desde Servidor SOC)
```bash
sudo /var/ossec/bin/agent_control -l
```

### Verificar Conectividad
```bash
# Ping desde el servidor SOC
ping 10.68.0.161  # Ubuntu
ping 10.68.0.163  # Windows
ping 10.68.0.165  # Kali
```

## Archivos de Configuración

Cada máquina tiene un archivo de configuración específico:

| Máquina | Archivo | Descripción |
|---------|---------|-------------|
| Servidor SOC | `server/config_red.md` | Wazuh Manager, ELK, servicios |
| Ubuntu | `linux/config_red.md` | Agente Wazuh, monitorización Linux |
| Windows | `windows/config_red.md` | Agente Wazuh, monitorización Windows |
| Kali | `kali_linux.md` | Herramientas de prueba, ataques |

## Validación de Configuración

### Validar Red
```bash
# Desde cualquier máquina
ip addr show          # Ver configuración IP
ip route show         # Ver rutas
ping 10.68.0.158     # Ping al servidor
```

### Validar Agentes
```bash
# Desde servidor SOC
sudo /var/ossec/bin/agent_control -l     # Listar agentes
sudo /var/ossec/bin/agent_control -i 001 # Info agente 1
```

### Validar Servicios
```bash
# Desde servidor SOC
sudo systemctl status wazuh-manager
sudo systemctl status elasticsearch
sudo systemctl status kibana
sudo systemctl status logstash
```

## Consideraciones de Seguridad

1. **Todas las máquinas están en la misma subred (10.68.0.0/8)**
   - Facilita el monitoreo
   - Simula un laboratorio educativo aislado

2. **Kali-Atacante NO tiene agente Wazuh**
   - Es la máquina atacante
   - Sus acciones son monitorizadas por otros agentes
   - Se usa solo para pruebas controladas

3. **Credenciales de prueba**
   - Usar contraseña Root1234$ en el laboratorio
   - Cambiar en producción
   - Usar claves SSH en ambientes reales

4. **Acceso Remoto Controlado**
   - Puerto SSH 9313 desde exterior (seguridad por oscuridad)
   - Restringir a IPs autorizadas en producción

## Referencias

- [Documentación Wazuh](docs/01_instalacion_wazuh.md)
- [Documentación ELK](docs/02_instalacion_elk.md)
- [Guía de Agentes](docs/03_configuracion_agentes.md)
- [Pruebas de Seguridad](docs/06_pruebas_simulacion.md)

## Soporte

Para problemas de configuración:

1. Revisar el archivo `config_red.md` correspondiente
2. Validar conectividad con `ping`
3. Revisar logs con `journalctl`
4. Contactar a los responsables del proyecto

---

**Proyecto**: Implantación de SOC Educativo con Wazuh + ELK
**Fecha**: Enero 2025
**Versión**: 1.0

