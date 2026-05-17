# Guía de Usuario del Entorno SOC con Wazuh

## Introducción

Este documento describe la infraestructura del entorno SOC (Security Operations Center) desplegado para prácticas de monitorización, administración y análisis de seguridad utilizando la plataforma Wazuh.

**Orientado a**:
- Administradores de infraestructura
- Docentes y formadores
- Estudiantes y practicantes
- Especialistas en seguridad

**Necesidades cubiertas**:
- Acceso al panel de monitorización
- Conexión remota al servidor
- Administración de equipos del laboratorio
- Configuración de red
- Verificación de conectividad
- Operación y mantenimiento

---

## Arquitectura General del Laboratorio

| Equipo | Función | Dirección IP | SO |
|--------|---------|-------------|-----|
| **Servidor SOC/Wazuh** | Monitorización y gestión centralizada | 10.68.0.158 | Ubuntu 22.04 LTS |
| **Ubuntu-cliente** | Equipo Linux monitorizado | 10.68.0.161 | Ubuntu 20.04/22.04 |
| **Windows-cliente** | Equipo Windows monitorizado | 10.68.0.163 | Windows 10/Server 2019 |
| **Kali-atacante** | Máquina de pruebas ofensivas | 10.68.0.165 | Kali Linux |

**Topología**: Red interna aislada 10.68.0.0/8 con gateway centralizado 10.0.0.8

---

## Acceso al Dashboard de Wazuh

### URL de Acceso

```
https://wazuh.soc.informatica.iesgrancapitan.org/
```

### Credenciales de Acceso

| Campo | Valor |
|-------|-------|
| **Usuario** | admin |
| **Contraseña** | bgCt6c9pHkPg+Z2OHL*Z.MCiEWo9rTWy |

### Funciones Principales del Dashboard

**Visualización de alertas** - Alertas en tiempo real clasificadas por severidad
**Gestión de agentes** - Estado, conexión y configuración de agentes
**Revisión de logs** - Búsqueda y análisis de eventos
**Detección de amenazas** - Alertas automáticas por patrones sospechosos
**Monitorización en tiempo real** - Métricas y estadísticas activas
**Correlación de eventos** - Análisis multi-evento de incidentes

### Proceso de Acceso

1. Abrir navegador web
2. Ir a: `https://wazuh.soc.informatica.iesgrancapitan.org/`
3. Ingresar usuario: `admin`
4. Ingresar contraseña: `bgCt6c9pHkPg+Z2OHL*Z.MCiEWo9rTWy`
5. Hacer clic en "Login"
6. Dashboard disponible

---

## Acceso Remoto al Servidor SOC

### Conexión SSH Directa

```bash
ssh -p 9313 administrador@cpd.informatica.iesgrancapitan.org
```

### Credenciales de SSH

| Campo | Valor |
|-------|-------|
| **Usuario** | administrador |
| **Contraseña** | Root1234$ |
| **Puerto** | 9313 (personalizado) |
| **Servidor** | cpd.informatica.iesgrancapitan.org |

### Desglose del Comando SSH

| Elemento | Descripción |
|----------|-------------|
| `ssh` | Cliente SSH (Secure Shell) |
| `-p 9313` | Puerto personalizado (no es el puerto 22 estándar) |
| `administrador` | Usuario remoto en el servidor |
| `cpd.informatica.iesgrancapitan.org` | Servidor remoto |

### Alternativa: Conexión Directa a Servidor Local

Si está en la misma red:

```bash
ssh administrador@10.68.0.158
```

### Ejemplo de Sesión SSH

```bash
$ ssh -p 9313 administrador@cpd.informatica.iesgrancapitan.org
administrador@cpd.informatica.iesgrancapitan.org's password: 
Welcome to Ubuntu 22.04 LTS
...
administrador@servidor-soc:~$
```

---

## Acceso desde el Servidor a las Máquinas Internas

### Ubuntu-cliente (10.68.0.161)

#### Conexión SSH

```bash
ssh usuario@10.68.0.161
```

#### Credenciales

| Campo | Valor |
|-------|-------|
| **Usuario** | usuario |
| **Contraseña** | Root1234$ |

#### Función

Sistema Linux monitorizado por Wazuh para:
- Pruebas de seguridad
- Generación de logs
- Simulación de eventos
- Prácticas educativas

#### Servicios Disponibles

| Servicio | Puerto | Estado |
|----------|--------|--------|
| SSH | 22 | Activo (monitorizado) |
| Apache2 | 80 | Activo (vulnerable) |
| MySQL | 3306 | Activo (credenciales débiles) |
| FTP | 21 | Activo (acceso anónimo) |
| Samba | 445 | Activo (shares inseguros) |

---

### Windows-cliente (10.68.0.163)

#### Conexión SSH (vía WSL/Terminal)

```bash
ssh usuario@10.68.0.163
```

#### Conexión RDP (Recomendado)

```bash
rdesktop -u usuario 10.68.0.163
# o
xfreerdp /u:usuario /p:Root1234$ /v:10.68.0.163
```

#### Credenciales

| Campo | Valor |
|-------|-------|
| **Usuario** | usuario |
| **Contraseña** | Root1234$ |

#### Función

Equipo Windows destinado a:
- Monitorización de eventos Windows
- Pruebas de seguridad en endpoint
- Análisis de Event ID
- Prácticas de hardening

#### Servicios Disponibles

| Servicio | Puerto | Estado |
|----------|--------|--------|
| RDP | 3389 | Activo (vulnerable) |
| SMB | 445 | Activo (inseguro) |
| WinRM | 5985 | Activo (mal configurado) |
| IIS | 80 | Activo (aplicación vulnerable) |

---

### Kali-atacante (10.68.0.165)

#### Conexión SSH

```bash
ssh kali@10.68.0.165
```

#### Credenciales

| Campo | Valor |
|-------|-------|
| **Usuario** | kali |
| **Contraseña** | Root1234$ |

#### Función

Máquina utilizada para:
- Simulación de ataques controlados
- Pruebas ofensivas educativas
- Generación de eventos de seguridad
- Laboratorios prácticos

#### Herramientas Disponibles

```bash
# Reconocimiento
- nmap      # Escaneo de puertos
- enum4linux # Enumeración de SMB

# Explotación
- hydra     # Fuerza bruta
- hashcat   # Cracking de contraseñas

# Análisis
- wireshark # Captura de tráfico
- tcpdump   # Análisis de red

# Otros
- metasploit # Framework de penetración testing
```

---

## Configuración de Red

### Servidor SOC

```yaml
network:
    ethernets:
        ens18:
            dhcp4: no
            addresses:
              - 10.68.0.158/8
            gateway4: 10.0.0.8
            nameservers:
               addresses: [8.8.8.8, 1.1.1.1]
    version: 2
```

**Explicación**:
- **IP estática** (10.68.0.158) para asegurar conectividad estable
- **Máscara /8** permite conectividad con toda la red 10.x.x.x
- **Gateway centralizado** (10.0.0.8) para enrutamiento
- **DNS públicos** (Google y Cloudflare) para resolución de nombres
- **Configuración mediante Netplan** y cloud-init

---

### Ubuntu-cliente

```yaml
network:
  version: 2
  renderer: networkd
  ethernets:
    ens18:
      dhcp4: false
      addresses:
        - 10.68.0.161/8
      gateway4: 10.0.0.8
      nameservers:
        addresses:
          - 8.8.8.8
          - 1.1.1.1
```

**Características**:
- IP estática asignada
- Connectividad con servidor SOC garantizada
- DNS configurado para resolución
- Compatible con netplan

---

### Windows-cliente

| Parámetro | Valor |
|-----------|-------|
| **IP** | 10.68.0.163 |
| **Máscara** | 255.0.0.0 (equivalente a /8) |
| **Gateway** | 10.0.0.8 |
| **DNS 1** | 8.8.8.8 (Google) |
| **DNS 2** | 1.1.1.1 (Cloudflare) |

**Configuración en Windows**:
1. Panel de Control → Red e Internet → Cambiar configuración de adaptador
2. Propiedades de la conexión
3. Propiedades de IPv4
4. Marcar "Usar la siguiente dirección IP"
5. Ingresar valores de la tabla

---

### Kali Linux

| Parámetro | Valor |
|-----------|-------|
| **IP** | 10.68.0.165 |
| **Máscara** | 255.0.0.0 (/8) |
| **Gateway** | 10.0.0.8 |
| **DNS 1** | 8.8.8.8 |
| **DNS 2** | 1.1.1.1 |

**Configuración con Netplan** (similar a Ubuntu):
```yaml
network:
  version: 2
  renderer: networkd
  ethernets:
    ens18:
      dhcp4: false
      addresses:
        - 10.68.0.165/8
      gateway4: 10.0.0.8
      nameservers:
        addresses: [8.8.8.8, 1.1.1.1]
```

---

## Comprobaciones Básicas

### Verificar Conectividad Interna

```bash
# Desde servidor SOC o cualquier máquina

# Ping a Ubuntu-cliente
ping 10.68.0.161

# Ping a Windows-cliente
ping 10.68.0.163

# Ping a Kali-atacante
ping 10.68.0.165

# Ping a Servidor SOC (desde otra máquina)
ping 10.68.0.158
```

**Salida esperada**:
```
PING 10.68.0.161 (10.68.0.161) 56(84) bytes of data.
64 bytes from 10.68.0.161: icmp_seq=1 ttl=64 time=2.45 ms
64 bytes from 10.68.0.161: icmp_seq=2 ttl=64 time=2.34 ms
```

### Verificar Conectividad a Internet

```bash
# Verificar conectividad con DNS público de Google
ping 8.8.8.8

# Verificar conectividad con DNS de Cloudflare
ping 1.1.1.1
```

### Verificar Resolución de Nombres

```bash
# Verificar DNS funcionando
ping google.com
ping github.com

# Consulta DNS directa
nslookup google.com
dig google.com
```

---

## Verificación de Agentes Wazuh

### Estado Esperado

Los agentes deben aparecer en el dashboard en estado:

```
Active (Activo)
```

### Aspectos a Revisar en Dashboard

1. **Última conexión** - Debe ser reciente (minutos)
2. **Estado del agente** - Debe ser "Active"
3. **Logs recibidos** - Debe haber eventos recientes
4. **Alertas generadas** - Al menos alertas de prueba
5. **Inventario del sistema** - Información del SO, procesos, etc.

### Comando para Verificar Agentes (SSH en servidor)

```bash
# Ver estado de agentes
/var/ossec/bin/agent_control -l

# Salida esperada:
Wazuh agent_control. List of agents.
   ID: 000, Name: servidor-soc, IP: Any, Status: Active
   ID: 001, Name: ubuntu-cliente, IP: 10.68.0.161, Status: Active
   ID: 002, Name: windows-cliente, IP: 10.68.0.163, Status: Active
   ID: 003, Name: kali-atacante, IP: 10.68.0.165, Status: Active
```

### Troubleshooting de Agentes

Si un agente no aparece como **Active**:

```bash
# 1. Verificar servicio en el cliente
systemctl status wazuh-agent

# 2. Reiniciar agente si está inactivo
systemctl restart wazuh-agent

# 3. Revisar logs del agente
tail -f /var/ossec/logs/active-responses.log

# 4. Verificar conectividad con servidor
ping 10.68.0.158

# 5. Revisar puerto 1514 (Wazuh)
telnet 10.68.0.158 1514
```

---

## Flujo de Funcionamiento del Laboratorio

```
┌─────────────────────────────────────────────────────────────┐
│              FLUJO OPERATIVO DEL LABORATORIO                │
└─────────────────────────────────────────────────────────────┘

1. GENERACIÓN DE ACTIVIDAD
   ↓
   Kali Linux → Genera actividad ofensiva (ataques simulados)
   Ubuntu-cliente → Genera eventos del sistema
   Windows-cliente → Registra eventos de Windows

2. CAPTURA DE EVENTOS
   ↓
   Los clientes registran eventos en sus logs locales

3. ENVÍO DE LOGS
   ↓
   Los agentes Wazuh envían logs al servidor (puerto 1514)
   Conexión cifrada y autenticada

4. ANÁLISIS
   ↓
   Wazuh Manager analiza los eventos
   Aplica reglas de detección
   Correlaciona eventos relacionados
   Genera alertas

5. VISUALIZACIÓN
   ↓
   ELK Stack (Elasticsearch, Logstash, Kibana) procesa datos
   Dashboard muestra alertas y eventos en tiempo real

6. REVISIÓN
   ↓
   Administrador/Estudiante revisa incidentes en dashboard
   Investiga eventos sospechosos
   Implementa acciones de respuesta
```

---

## Resolución de Problemas

### Problema: SSH no funciona

**Síntomas**:
- No se puede conectar al servidor
- Timeout en la conexión
- Acceso denegado

**Verificaciones**:

```bash
# 1. Ver estado del servicio SSH
systemctl status ssh

# 2. Verificar si SSH escucha en puerto personalizado
sudo netstat -tlnp | grep 9313

# 3. Revisar configuración SSH
sudo cat /etc/ssh/sshd_config | grep -i port

# 4. Comprobar firewall
sudo ufw status

# 5. Permitir puerto en firewall si es necesario
sudo ufw allow 9313/tcp

# 6. Reiniciar servicio SSH
sudo systemctl restart ssh
```

**Checklist**:
- ☐ Firewall permite puerto SSH (9313)
- ☐ Credenciales correctas (administrador/Root1234$)
- ☐ Conectividad de red hacia servidor (ping)
- ☐ Puerto correcto (-p 9313)

---

### Problema: El agente no aparece en dashboard

**Síntomas**:
- Agente no aparece en lista
- Estado muestra "Disconnected"
- No hay eventos recibidos

**Verificaciones en el Cliente**:

```bash
# 1. Verificar si agente está instalado
sudo ls -la /var/ossec/

# 2. Comprobar estado del servicio
sudo systemctl status wazuh-agent

# 3. Revisar logs del agente
sudo tail -20 /var/ossec/logs/active-responses.log
sudo tail -20 /var/ossec/logs/ossec.log

# 4. Verificar conectividad con servidor
ping 10.68.0.158
telnet 10.68.0.158 1514

# 5. Revisar configuración del agente
sudo cat /var/ossec/etc/ossec.conf | grep -A 5 manager-ip
```

**Verificaciones en el Servidor**:

```bash
# 1. Verificar si agent aparece en manager
sudo /var/ossec/bin/agent_control -l

# 2. Revisar logs del manager
sudo tail -50 /var/ossec/logs/ossec.log

# 3. Reiniciar Wazuh manager
sudo systemctl restart wazuh-manager
```

**Soluciones Comunes**:
- Reiniciar agente: `sudo systemctl restart wazuh-agent`
- Reiniciar manager: `sudo systemctl restart wazuh-manager`
- Verificar IP en configuración del agente
- Verificar firewall en ambos lados

---

## Resumen de Credenciales

### Acceso a Servicios

| Servicio | Usuario | Contraseña |
|----------|---------|-----------|
| **Dashboard Wazuh** | admin | bgCt6c9pHkPg+Z2OHL*Z.MCiEWo9rTWy |
| **SSH servidor SOC** | administrador | Root1234$ |
| **Ubuntu-cliente (SSH)** | usuario | Root1234$ |
| **Windows-cliente (RDP)** | usuario | Root1234$ |
| **Kali-atacante (SSH)** | kali | Root1234$ |

### Credenciales de Sistema Wazuh

| Componente | Usuario | Contraseña | Puerto |
|------------|---------|-----------|--------|
| **API Wazuh** | wazuh | Root1234$ | 55000 |
| **API Dashboard** | wazuh-wui | WazuhWui2026 | Interno |
| **Kibana/Dashboard** | kibanaserver | WazuhDashboard2026Secure | 5601 |

---

## Resumen de Direcciones IP

| Equipo | IP Interna | Función |
|--------|-----------|---------|
| **Servidor SOC/Wazuh** | 10.68.0.158 | Central de monitorización |
| **Ubuntu-cliente** | 10.68.0.161 | Cliente Linux monitorizado |
| **Windows-cliente** | 10.68.0.163 | Cliente Windows monitorizado |
| **Kali-atacante** | 10.68.0.165 | Máquina de pruebas ofensivas |

**Configuración de Red**:
- **Rango de red**: 10.68.0.0/8
- **Gateway**: 10.0.0.8
- **DNS primario**: 8.8.8.8 (Google)
- **DNS secundario**: 1.1.1.1 (Cloudflare)

---

## Tareas Comunes

### Tarea 1: Verificar Salud del Laboratorio

```bash
# En servidor SOC
ssh -p 9313 administrador@cpd.informatica.iesgrancapitan.org

# Una vez conectado:
# 1. Ver agentes
/var/ossec/bin/agent_control -l

# 2. Ver alertas recientes
tail -20 /var/ossec/logs/alerts/alerts.log

# 3. Verificar estado de servicios
systemctl status wazuh-manager
systemctl status elasticsearch
systemctl status kibana
```

### Tarea 2: Generar Evento de Prueba

```bash
# En Ubuntu-cliente
ssh usuario@10.68.0.161

# Generar evento (intento fallido de login)
su nonexistent 2>&1 | grep -i password

# Esperar 1-2 minutos y verificar en dashboard
```

### Tarea 3: Revisar Logs en Kibana

1. Acceder a: https://wazuh.soc.informatica.iesgrancapitan.org/
2. Usuario: `admin`
3. Contraseña: `bgCt6c9pHkPg+Z2OHL*Z.MCiEWo9rTWy`
4. Ir a "Logs" o "Events"
5. Filtrar por IP cliente: `10.68.0.161`
6. Revisar eventos recientes

---

## Conclusión

Este entorno SOC permite realizar prácticas reales de:

**Monitorización** - Seguimiento centralizado de eventos
**Detección de amenazas** - Análisis automático de eventos
**Análisis de logs** - Búsqueda e investigación de datos
**Administración remota** - Gestión desde cualquier ubicación
**Simulación de ataques** - Pruebas ofensivas controladas
**Gestión de incidentes** - Respuesta ante eventos de seguridad

**Todo ello utilizando Wazuh como plataforma centralizada de seguridad**, replicando un entorno profesional real apto para formación educativa de nivel superior.

---

**Documentación de Usuario - SOC Educativo**
**Última actualización**: Mayo 2026
*IES Gran Capitán - Ciclos Formativos de Grado Superior*