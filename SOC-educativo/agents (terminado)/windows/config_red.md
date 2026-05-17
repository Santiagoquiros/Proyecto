# Configuración del Agente Wazuh - Cliente Windows

## Información del Cliente

**IP**: 10.68.0.163
**SO**: Windows Server 2019+
**Agente Wazuh**: Instalado

## Configuración de Red

```
Dirección IP: 10.68.0.163
Máscara de subred: 255.0.0.0
Puerta de enlace: 10.0.0.8
DNS primario: 8.8.8.8
DNS secundario: 1.1.1.1
```

### Configuración en PowerShell
```powershell
# Establecer IP estática
New-NetIPAddress -IPAddress 10.68.0.163 -PrefixLength 8 -InterfaceAlias "Ethernet"

# Establecer puerta de enlace
New-NetRoute -DestinationPrefix 0.0.0.0/0 -NextHop 10.0.0.8 -InterfaceAlias "Ethernet"

# Establecer DNS
Set-DnsClientServerAddress -InterfaceAlias "Ethernet" -ServerAddresses 8.8.8.8,1.1.1.1
```

## Credenciales SSH

```
Usuario: usuario
Contraseña: Root1234$
```

## Conexión desde Servidor SOC

```bash
ssh usuario@10.68.0.163
```

## Eventos Monitorizados

### Event Logs
- **Security**: Eventos de autenticación, acceso, cambios de permisos
- **System**: Eventos del sistema operativo, instalación de drivers
- **Application**: Eventos de aplicaciones
- **PowerShell**: Comandos y scripts ejecutados

### Integridad de Archivos (FIM)
- `C:\Windows\System32\config` - Configuración del sistema
- `C:\Windows\System32` - Archivos del sistema
- `C:\Program Files` - Programas instalados
- `C:\Users` - Directorios de usuarios

### Detección de Anomalías
- Rootkits de Windows
- Vulnerabilidades del sistema
- Cambios en archivos críticos
- Eventos sospechosos de PowerShell

## Verificación de Conexión

Para verificar que el agente está conectado:

```powershell
# Ver estado del servicio Wazuh
Get-Service -Name "Wazuh"

# Debe mostrar: Running

# Ver logs del agente
Get-Content -Path "C:\Program Files (x86)\ossec-agent\ossec.log" -Tail 20 -Wait
```

Desde el servidor SOC:
```bash
sudo /var/ossec/bin/agent_control -l

# Debe mostrar:
# ID: 002
# Name: windows-cliente
# IP: 10.68.0.163
# Status: Active
```

## Troubleshooting

### El agente no se conecta
```powershell
# Reiniciar servicio
Restart-Service -Name "Wazuh"

# Ver estado
Get-Service -Name "Wazuh"

# Ver logs detallados
Get-Content -Path "C:\Program Files (x86)\ossec-agent\ossec.log" -Tail 50
```

### Validar conectividad
```powershell
# Ping al servidor SOC
ping 10.68.0.158

# Verificar puerto 1514
Test-NetConnection -ComputerName 10.68.0.158 -Port 1514
```

### El firewall bloquea la comunicación
```powershell
# Permitir salida por puerto 1514
New-NetFirewallRule -DisplayName "Wazuh Agent" `
  -Direction Outbound -Action Allow `
  -LocalPort 1514 -Protocol TCP
```

## Notas Importantes

1. El cliente está en la misma subred que el servidor (10.68.0.0/8)
2. La conectividad se establece a través del puerto 1514 (TCP/UDP)
3. PowerShell debe estar habilitado para registrar eventos
4. El agente se reinicia automáticamente si se detiene
5. Todos los eventos se envían cifrados al servidor SOC
6. Los Event Logs deben estar habilitados en Windows
