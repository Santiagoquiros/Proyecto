# 3. Configuración de Agentes Wazuh

## Introducción

Los agentes Wazuh se instalan en los servidores y estaciones de trabajo para recolectar eventos de seguridad. Esta guía cubre la instalación en Linux y Windows.

## Instalación de Agentes en Linux

### 1. Obtener Clave de Agente del Manager

En el Wazuh Manager:
```bash
# Usar utilidad de autenticación
sudo /var/ossec/bin/agent-auth -m <IP-WAZUH-MANAGER> -A <NOMBRE-AGENTE>

# Ejemplo
sudo /var/ossec/bin/agent-auth -m 192.168.1.100 -A linux-server-01

# El agente recibirá una clave de cifrado
```

### 2. Instalar Wazuh Agent en Ubuntu/Debian

```bash
# Agregar clave GPG
curl -s https://packages.wazuh.com/key/GPG-KEY-WAZUH | apt-key add -

# Agregar repositorio
echo "deb https://packages.wazuh.com/4.x/apt/ stable main" | \
  tee /etc/apt/sources.list.d/wazuh.list

# Actualizar e instalar
sudo apt-get update
sudo apt-get install -y wazuh-agent

# Habilitar e iniciar servicio
sudo systemctl daemon-reload
sudo systemctl enable wazuh-agent
sudo systemctl start wazuh-agent

# Verificar estado
sudo systemctl status wazuh-agent
```

### 3. Instalar Wazuh Agent en CentOS/RHEL

```bash
# Agregar clave GPG
rpm --import https://packages.wazuh.com/key/GPG-KEY-WAZUH

# Agregar repositorio
cat > /etc/yum.repos.d/wazuh.repo << 'EOF'
[wazuh]
gpgcheck=1
gpgkey=https://packages.wazuh.com/key/GPG-KEY-WAZUH
enabled=1
name=EL-$releasever - Wazuh
baseurl=https://packages.wazuh.com/4.x/yum/
protect=1
EOF

# Instalar
sudo yum install -y wazuh-agent

# Habilitar e iniciar
sudo systemctl daemon-reload
sudo systemctl enable wazuh-agent
sudo systemctl start wazuh-agent
```

### Reiniciar Agente
```bash
sudo systemctl restart wazuh-agent

# Ver logs
tail -f /var/ossec/logs/ossec.log
```

## Instalación de Agentes en Windows

### 1. Descargar Instalador
```powershell
# Descargar desde PowerShell
$url = "https://packages.wazuh.com/4.x/windows/wazuh-agent-4.x.x-1.msi"
$output = "C:\tmp\wazuh-agent.msi"
Invoke-WebRequest -Uri $url -OutFile $output
```

### 2. Instalar Agente
```powershell
# Ejecutar instalador (reemplazar IP y nombre de agente)
msiexec.exe /i "C:\tmp\wazuh-agent.msi" ^
  /q ^
  WAZUH_MANAGER="192.168.1.100" ^
  WAZUH_AGENT_NAME="windows-server-01" ^
  WAZUH_AGENT_GROUP="windows"

# Reiniciar para aplicar cambios
Restart-Computer -Force
```

## Verificar Conexión de Agentes

### En el Wazuh Manager
```bash
# Listar todos los agentes
sudo /var/ossec/bin/agent_control -l

# Ver estado detallado de agente específico
sudo /var/ossec/bin/agent_control -i 001

# Ver agentes conectados
sudo /var/ossec/bin/agent_control -s

# Resultado esperado: "Connected" o "Active"
```

## Siguiente Paso

Continúa con [Configuración de Alertas](04_configuracion_alertas.md)

---

**Última actualización**: Mayo 2026
