# Configuración del Agente Wazuh - Cliente Ubuntu

## Información del Cliente

**IP**: 10.68.0.161
**SO**: Ubuntu Server 20.04 LTS
**Agente Wazuh**: Instalado

## Configuración de Red

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

## Credenciales SSH

```
Usuario: usuario
Contraseña: Root1234$
```

## Conexión desde Servidor SOC

```bash
ssh usuario@10.68.0.161
```


## Eventos Monitorizados

### Logs del Sistema
- `/var/log/auth.log` - Eventos de autenticación
- `/var/log/syslog` - Logs generales del sistema
- `/var/log/sudo.log` - Comandos ejecutados con sudo

### Integridad de Archivos (FIM)
- `/etc` - Configuración del sistema
- `/usr/bin` - Binarios del sistema
- `/usr/sbin` - Binarios de administración
- `/root` - Directorio home del root
- `/home` - Directorios de usuarios

### Detección de Anomalías
- Rootkits
- Vulnerabilidades de sistema
- Cambios no autorizados

## Verificación de Conexión

Para verificar que el agente está conectado:

```bash
# Desde el servidor SOC
sudo /var/ossec/bin/agent_control -l

Wazuh agent_control. List of available agents:
   ID: 000, Name: wazuh-soc (server), IP: 127.0.0.1, Active/Local
   ID: 003, Name: Cliente-UBU, IP: any, Active
   ID: 004, Name: ClienteWIN, IP: any, Active


### El agente no se conecta
```bash
# Verificar servicio
sudo systemctl status wazuh-agent

# Ver logs
tail -f /var/ossec/logs/ossec.log

# Reiniciar agente
sudo systemctl restart wazuh-agent
```

### Validar conectividad
```bash
# Ping al servidor SOC
ping 10.68.0.158

# Verificar puerto 1514
nc -zv 10.68.0.158 1514
```

## Notas Importantes

1. El cliente está en la misma subred que el servidor (10.68.0.0/8)
2. La conectividad se establece a través del puerto 1514 (TCP/UDP)
3. Los logs se procesan cada 43200 segundos (12 horas) para FIM
4. El agente se reinicia automáticamente si se detiene
5. Todos los eventos se envían cifrados al servidor SOC
