# 1. Instalación de Wazuh Manager

## Introducción

Wazuh Manager es el componente central que recibe, procesa y analiza los eventos de seguridad de todos los agentes. Esta guía describe la instalación en Ubuntu Server 24.04 LTS.

## Requisitos Previos

### Sistema Operativo
- **OS**: Ubuntu Server 24.04 LTS 
- **Memoria RAM**: Mínimo 16 GB 
- **CPU**: 4 cores
- **Almacenamiento**: 60 GB
- **Red**: IP estática con acceso a internet

### Paquetes Requeridos
```bash
sudo apt-get update
sudo apt-get install curl apt-transport-https lsb-release gnupg2
```

## Instalación de Wazuh Manager

### 1. Agregar Repositorio de Wazuh
```bash
# Añadir clave GPG
curl -s https://packages.wazuh.com/key/GPG-KEY-WAZUH | apt-key add -

# Agregar repositorio
echo "deb https://packages.wazuh.com/4.x/apt/ stable main" | \
  tee /etc/apt/sources.list.d/wazuh.list

# Actualizar repositorios
sudo apt-get update
```

### 2. Instalar Wazuh Manager
```bash
# Instalar wazuh-manager
sudo apt-get install -y wazuh-manager

# Iniciar el servicio
sudo systemctl daemon-reload
sudo systemctl enable wazuh-manager
sudo systemctl start wazuh-manager

# Verificar estado
sudo systemctl status wazuh-manager
```

### 3. Verificar Instalación
```bash
# Ver versión
/var/ossec/bin/wazuh-control status

# Ver logs de Wazuh
tail -f /var/ossec/logs/ossec.log
```

## Configuración Básica

### Archivo de Configuración Principal
Ubicación: `/var/ossec/etc/ossec.conf`


### 4. Configuración de Reglas

Las reglas determinan qué eventos generar alertas. Se encuentran en:
- `/var/ossec/ruleset/rules/` (reglas compiladas)
- `/var/ossec/etc/rules/` (reglas customizadas)


## Configuración de Seguridad

### Habilitar HTTPS para API
```bash
# Generar certificados (si no existen)
sudo /var/ossec/bin/wazuh-certs-tool.sh -a

# Copiar certificados
sudo cp /var/ossec/etc/ssl/certs/* /etc/ssl/certs/
sudo cp /var/ossec/etc/ssl/keys/* /etc/ssl/private/
```

## Base de Datos de Wazuh

### Ubicación
```
/var/ossec/queue/db/
```

### Ver Agents
```bash
# Listar agentes conectados
sudo /var/ossec/bin/agent_control -l

# Ver estado de agente específico
sudo /var/ossec/bin/agent_control -i <agent_id>
```

## Monitorización

### Ver Logs en Tiempo Real
```bash
# Logs principales
tail -f /var/ossec/logs/ossec.log

# Logs de alertas
tail -f /var/ossec/logs/alerts/alerts.log

# Logs de eventos JSON
tail -f /var/ossec/logs/alerts/alerts.json
```

### Comandos Útiles
```bash
# Ver estadísticas
/var/ossec/bin/wazuh-control info

# Ver reglas cargadas
/var/ossec/bin/wazuh-control rule-info

# Comprobar configuración
/var/ossec/bin/wazuh-control config-test
```

## Troubleshooting

### Wazuh Manager no inicia
```bash
# Ver logs de error
sudo /var/ossec/bin/wazuh-control start
tail -f /var/ossec/logs/ossec.log

# Validar configuración
sudo /var/ossec/bin/wazuh-control config-test

# Reiniciar servicio
sudo systemctl restart wazuh-manager
```

### Agentes no se conectan
```bash
# Verificar que puerto 1514 esté abierto
sudo netstat -tlnp | grep 1514

# Permitir puerto en firewall
sudo ufw allow 1514/tcp
sudo ufw allow 1514/udp
sudo ufw allow 1515/tcp  # Auth

# Comprobar conectividad
telnet <ip-manager> 1514
```

### Alto uso de recursos
```bash
# Limitar logs guardados
sed -i 's/<keep>1<\/keep>/<keep>5<\/keep>/' /var/ossec/etc/ossec.conf

# Reducir nivel de log
sed -i 's/<logall_json>yes<\/logall_json>/<logall_json>no<\/logall_json>/' \
  /var/ossec/etc/ossec.conf
```

## Siguiente Paso

Continúa con [Instalación de ELK Stack](02_instalacion_elk.md)

---

**Última actualización**: Mayo 2026
