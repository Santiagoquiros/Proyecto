# Configuración del Atacante - Kali Linux

## Información de Kali

**IP**: 10.68.0.165
**SO**: Kali Linux
**Propósito**: Máquina atacante para pruebas de seguridad

## Configuración de Red

```
Dirección IP: 10.68.0.165
Máscara de subred: 255.0.0.0
Puerta de enlace: 10.0.0.8
DNS primario: 8.8.8.8
DNS secundario: 1.1.1.1
```

### Configuración en Terminal
```bash
# Configurar interfaz de red permanentemente
sudo nano /etc/network/interfaces

# Agregar:
auto ens18
iface ens18 inet static
    address 10.68.0.165
    netmask 255.0.0.0
    gateway 10.0.0.8
    dns-nameservers 8.8.8.8 1.1.1.1

# Reiniciar red
sudo systemctl restart networking
```

## Credenciales SSH

```
Usuario: kali
Contraseña: Root1234$
```

## Conexión desde Servidor SOC

```bash
ssh kali@10.68.0.165
```

## Herramientas Disponibles en Kali

### Herramientas de Prueba Instaladas
- **nmap** - Escaneo de puertos y servicios
- **metasploit** - Framework de explotación
- **hydra** - Ataque de fuerza bruta
- **tcpdump** - Captura de paquetes
- **wireshark** - Análisis de tráfico
- **hashcat** - Cracking de contraseñas
- **john** - Cracking de contraseñas
- **sqlmap** - Inyección SQL
- **nikto** - Scanner web
- **aircrack-ng** - Auditoría WiFi

## Escenarios de Prueba Desde Kali

### Escenario 1: Escaneo de Red
```bash
# Escaneo de puertos del servidor SOC
nmap -p- 10.68.0.158

# Escaneo de servicios
nmap -sV 10.68.0.158

# Escaneo de red completa
nmap -A 10.68.0.0/8
```

### Escenario 2: Ataque de Fuerza Bruta SSH
```bash
# Contra Ubuntu-cliente
hydra -l usuario -P /usr/share/wordlists/rockyou.txt ssh://10.68.0.161

# Contra Windows-cliente (si tiene SSH habilitado)
hydra -l usuario -P /usr/share/wordlists/rockyou.txt ssh://10.68.0.163
```

### Escenario 3: Captura de Tráfico
```bash
# Capturar tráfico de red
sudo tcpdump -i ens18 -n -X

# Capturar tráfico específico
sudo tcpdump -i ens18 host 10.68.0.161
```

### Escenario 4: Análisis con Wireshark
```bash
# Iniciar Wireshark
sudo wireshark &

# Seleccionar interfaz ens18
# Aplicar filtros y analizar tráfico
```

## Monitoreo Desde Kali

### Ver Tráfico de Red
```bash
# Mostrar conexiones activas
netstat -tuln

# Ver conexiones de red
ss -tuln
```

### Información del Sistema
```bash
# Ver configuración de red
ip addr show
ip route show

# Ver tabla de ARP
arp -a

# Prueba de conectividad
ping 10.68.0.158
ping 10.68.0.161
ping 10.68.0.163
```

## Consideraciones Importantes

### Uso Ético
- **SOLO** usar esta máquina para pruebas en el laboratorio educativo
- **NO** usar contra sistemas externos sin autorización explícita
- **DOCUMENTAR** todas las pruebas realizadas
- **REPORTAR** vulnerabilidades encontradas

### Alcance Autorizado
- Pruebas contra máquinas en la subred 10.68.0.0/8
- Simulaciones de ataques controladas
- Pruebas de detección del SOC
- Ataques a sistemas externos
- Uso para obtener acceso no autorizado
- Daño intencional a sistemas

## Tipos de Pruebas Recomendadas

### Test 1: Fuerza Bruta SSH
```bash
# Simular intentos fallidos
for i in {1..10}; do
  ssh -u wronguser@10.68.0.161
  sleep 1
done
```

### Test 2: Escaneo de Puertos
```bash
# Escaneo agresivo
nmap -A -p- 10.68.0.161
```

### Test 3: Captura de Tráfico
```bash
# Monitorear tráfico hacia el cliente
sudo tcpdump -i ens18 -w traffic.pcap host 10.68.0.161
```

### Test 4: Validar Detección
```bash
# Modificar archivo en el cliente Linux
ssh usuario@10.68.0.161
echo "test" >> /etc/hosts

# Ver si el SOC detecta el cambio en Kibana
```

## Notas Importantes

1. **Kali NO es cliente** del SOC, no ejecuta agente Wazuh
2. Es la máquina desde la que se lanzan ataques para probar la detección
3. Sus acciones serán monitorizadas por los agentes de otros clientes
4. Se usa solo en escenarios controlados de prueba
5. Todos los eventos generados deben ser documentados
6. Es fundamental mantener esta máquina separada de la red de producción

## Recordatorio de Seguridad

> Esta máquina tiene herramientas de hacking poderosas.
> **Solo úsala para fines educativos autorizados**
> **En el laboratorio del centro educativo**

Cualquier uso malicioso es:
- Ilegal
- Contrario a la ética profesional
- Motivo de expulsión del programa
- Pasible de acciones legales

---

**Responsables**: Asegurar uso ético y controlado de esta máquina
