# Práctica 2 — Incorporación de agentes y visualización de alertas

> **Proyecto:** SOC Educativo  
> **Nivel:** Principiante / CFGS  
> **Duración estimada:** 3–4 horas  
> **Prerequisito:** Práctica 1 — Servidor Wazuh operativo

---

## Índice

- [Práctica 2 — Incorporación de agentes y visualización de alertas](#práctica-2--incorporación-de-agentes-y-visualización-de-alertas)
  - [Índice](#índice)
  - [Objetivo](#objetivo)
  - [Infraestructura necesaria](#infraestructura-necesaria)
  - [Instalación del agente Linux](#instalación-del-agente-linux)
  - [Instalación del agente Windows](#instalación-del-agente-windows)
  - [Verificación de agentes conectados](#verificación-de-agentes-conectados)
  - [Generación de ataques y eventos](#generación-de-ataques-y-eventos)
    - [Fuerza bruta SSH contra VM-LINUX](#fuerza-bruta-ssh-contra-vm-linux)
    - [Escaneo de puertos con Nmap](#escaneo-de-puertos-con-nmap)
    - [Acceso FTP anónimo contra VM-LINUX](#acceso-ftp-anónimo-contra-vm-linux)
    - [Intentos de RDP contra VM-WINDOWS](#intentos-de-rdp-contra-vm-windows)
    - [Modificación de ficheros críticos (Integrity Monitoring)](#modificación-de-ficheros-críticos-integrity-monitoring)
  - [Visualización de alertas en el dashboard](#visualización-de-alertas-en-el-dashboard)
  - [Tabla de rule levels](#tabla-de-rule-levels)
  - [Ejercicios propuestos](#ejercicios-propuestos)

---

## Objetivo

En esta práctica el alumnado aprenderá a:

- Conectar agentes Wazuh sobre máquinas Linux y Windows al servidor SOC.
- Generar tráfico y ataques controlados contra servicios vulnerables.
- Observar y clasificar las alertas resultantes en el dashboard según su nivel de severidad (`rule.level`).

---

## Infraestructura necesaria

| Máquina | Sistema operativo | IP | Rol |
|---|---|---|---|
| VM-SOC | Ubuntu Server 22.04 | 192.168.XX.10 | Servidor Wazuh + Dashboard |
| VM-LINUX | Ubuntu Server 20/22.04 | 192.168.XX.20 | Agente Linux |
| VM-WINDOWS | Windows 10 / Server 2019 | 192.168.XX.30 | Agente Windows |

> Todas las máquinas deben estar en la misma red host-only o red interna, sin acceso a Internet.

---

## Instalación del agente Linux

Ejecuta los siguientes comandos en **VM-LINUX**:

```bash
# Añadir repositorio de Wazuh
curl -s https://packages.wazuh.com/key/GPG-KEY-WAZUH | sudo apt-key add -
echo "deb https://packages.wazuh.com/4.x/apt/ stable main" | \
  sudo tee /etc/apt/sources.list.d/wazuh.list

# Instalar agente
sudo apt update && sudo apt install wazuh-agent -y
```

Configura la IP del manager en `/var/ossec/etc/ossec.conf`:

```xml
<server>
  <address>192.168.XX.10</address>
</server>
```

Inicia el agente:

```bash
sudo systemctl enable wazuh-agent
sudo systemctl start wazuh-agent
```

---

## Instalación del agente Windows

1. Descarga el instalador MSI desde el servidor SOC o desde [packages.wazuh.com](https://packages.wazuh.com/4.x/windows/).
2. Ejecuta el instalador como Administrador.
3. Cuando solicite la IP del manager introduce: `192.168.XX.10`
4. Finaliza la instalación y verifica que el servicio **Wazuh Agent** aparece como *En ejecución* en `services.msc`.

> **Alternativa silenciosa** (PowerShell como Administrador):
> ```powershell
> msiexec /i wazuh-agent.msi /q WAZUH_MANAGER="192.168.XX.10"
> net start WazuhSvc
> ```

---

## Verificación de agentes conectados

Desde **VM-SOC**, comprueba que ambos agentes aparecen como `Active`:

```bash
/var/ossec/bin/agent_control -l
```

Salida esperada:

```
ID: 001, Name: vm-linux,  IP: 192.168.XX.20, Status: Active
ID: 002, Name: vm-windows, IP: 192.168.XX.30, Status: Active
```

Si algún agente aparece como `Disconnected`, revisa conectividad de red y que el firewall del servidor permite los puertos `1514` y `1515`.

---

## Generación de ataques y eventos

Los siguientes ataques son **controlados y seguros** dentro del entorno de laboratorio aislado.

### Fuerza bruta SSH contra VM-LINUX

Desde cualquier máquina de la red:

```bash
# Instalar hydra si no está disponible
sudo apt install hydra -y

# Ataque de fuerza bruta SSH
hydra -l alumno -P /usr/share/wordlists/rockyou.txt \
  ssh://192.168.XX.20 -t 4
```

**Reglas esperadas:** `5710`, `5712`, `5763` — niveles 5, 8 y 10.

---

### Escaneo de puertos con Nmap

```bash
sudo apt install nmap -y

# Escaneo básico
nmap -sS 192.168.XX.20
nmap -sS 192.168.XX.30

# Escaneo agresivo
nmap -A -T4 192.168.XX.20
```

**Reglas esperadas:** `40101` — nivel 6.

---

### Acceso FTP anónimo contra VM-LINUX

```bash
ftp 192.168.XX.20
# Usuario: anonymous
# Contraseña: (vacía)
```

**Reglas esperadas:** `11204` — nivel 5.

---

### Intentos de RDP contra VM-WINDOWS

```bash
# Desde Linux con xfreerdp
xfreerdp /u:alumno /p:wrongpass /v:192.168.XX.30

# O con hydra
hydra -l alumno -P /usr/share/wordlists/rockyou.txt \
  rdp://192.168.XX.30
```

**Reglas esperadas:** `60106`, `60137` — niveles 5 y 10.

---

### Modificación de ficheros críticos (Integrity Monitoring)

En **VM-LINUX**:

```bash
sudo nano /etc/passwd
# Añade un espacio al final y guarda
```

**Reglas esperadas:** `550`, `554` — niveles 7 y 5.

---

## Visualización de alertas en el dashboard

1. Accede al dashboard: `https://192.168.XX.10` (o la URL de tu entorno).
2. Ve a **Wazuh → Security Events**.
3. Ajusta el rango temporal a **Last 24 hours**.
4. Usa los siguientes filtros para explorar las alertas:

| Filtro | Descripción |
|---|---|
| `agent.name: vm-linux` | Alertas del agente Linux |
| `agent.name: vm-windows` | Alertas del agente Windows |
| `rule.level >= 10` | Solo alertas críticas |
| `rule.groups: authentication_failed` | Fallos de autenticación |
| `data.srcip: 192.168.XX.X` | Por IP origen del ataque |

---

## Tabla de rule levels

Wazuh clasifica las alertas con niveles del 0 al 15:

| Nivel | Severidad | Descripción |
|---|---|---|
| 0–3 | Informativo | Eventos normales del sistema |
| 4–6 | Low | Actividad sospechosa leve |
| 7–9 | Medium | Intentos de acceso, errores repetidos |
| 10–12 | High | Ataques detectados, fuerza bruta exitosa |
| 13–15 | Critical | Compromisos graves, rootkits, intrusiones |

---

## Ejercicios propuestos

1. **Identifica** qué ataque generó el nivel más alto de alerta en tu sesión.
2. **Compara** las alertas generadas en el agente Linux vs Windows para el mismo tipo de ataque.
3. **Filtra** en el dashboard solo las alertas de nivel ≥ 10 y documenta las reglas disparadas.
4. **Investiga** en el framework MITRE ATT&CK la técnica asociada a cada regla (campo `rule.mitre.technique`).
5. **Reflexiona:** ¿Qué ataque pasaría desapercibido si no tuvieras el agente instalado en esa máquina?

---

> **Nota de seguridad:** Todos los ataques de esta práctica deben realizarse exclusivamente dentro de la red de laboratorio aislada. Reproducir estas técnicas fuera del entorno controlado puede ser constitutivo de delito.