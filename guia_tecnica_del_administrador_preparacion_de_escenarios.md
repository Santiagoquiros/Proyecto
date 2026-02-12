# Guía técnica del administrador – Instalación y configuración completa del laboratorio SOC

## 0. Objetivo del documento
Esta guía permite **crear desde cero** el laboratorio del TFG: infraestructura, Wazuh + Elastic, agentes, **servicios deliberadamente vulnerables** y validación de que los eventos se registran correctamente. Está pensada para **administradores sin experiencia previa en SOC**.

> ⚠️ Uso exclusivo en laboratorio aislado. No exponer a Internet.

---

## 1. Infraestructura base

### 1.1 Máquinas virtuales
| Máquina | SO | CPU | RAM | Disco |
|------|----|----|----|------|
| VM-SOC | Ubuntu Server 22.04 | 6–8 | 16 GB | 250 GB |
| VM-LINUX | Ubuntu Server 20.04/22.04 | 2–4 | 4 GB | 60 GB |
| VM-WINDOWS | Windows 10 / Server 2019 | 4 | 8 GB | 100 GB |

### 1.2 Red
- Tipo: **Host-only** o red interna
- Sin acceso directo a Internet
- IPs estáticas:
  - SOC: 192.168.56.10
  - Linux: 192.168.56.20
  - Windows: 192.168.56.30

---

## 2. Instalación del servidor SOC (Wazuh + Elastic)

### 2.1 Preparación del sistema
```bash
sudo apt update && sudo apt upgrade -y
sudo apt install curl unzip gnupg apt-transport-https ca-certificates -y
sudo hostnamectl set-hostname wazuh-soc
sudo reboot
```

### 2.2 Instalación oficial (all-in-one)
Guía oficial: https://documentation.wazuh.com/current/installation-guide/

```bash
curl -sO https://packages.wazuh.com/4.7/wazuh-install.sh
sudo bash wazuh-install.sh -a
```

### 2.3 Verificación
```bash
sudo systemctl status wazuh-manager
sudo systemctl status wazuh-indexer
sudo systemctl status wazuh-dashboard
```

Acceso web:
- https://soc.informatica.iesgrancapitan.org

---

## 3. Configuración de firewall

```bash
sudo ufw allow 1514/tcp
sudo ufw allow 1514/udp
sudo ufw allow 1515/tcp
sudo ufw allow 55000/tcp
sudo ufw allow 5601/tcp
sudo ufw enable
```

---

## 4. Instalación de agentes Wazuh

### 4.1 Agente Linux
Guía: https://documentation.wazuh.com/current/installation-guide/wazuh-agent/

```bash
curl -s https://packages.wazuh.com/key/GPG-KEY-WAZUH | sudo apt-key add -
echo "deb https://packages.wazuh.com/4.x/apt/ stable main" | sudo tee /etc/apt/sources.list.d/wazuh.list
sudo apt update && sudo apt install wazuh-agent -y
```

Editar `/var/ossec/etc/ossec.conf`:
```xml
<server>
  <address>192.168.56.10</address>
</server>
```

```bash
sudo systemctl enable wazuh-agent
sudo systemctl start wazuh-agent
```

### 4.2 Agente Windows
Guía: https://documentation.wazuh.com/current/installation-guide/wazuh-agent/wazuh-agent-windows.html

1. Descargar MSI
2. Durante la instalación indicar IP: `192.168.56.10`
3. Verificar servicio **Wazuh Agent**

---

## 5. Configuración de servicios vulnerables – Linux

### 5.1 SSH vulnerable (fuerza bruta)
```bash
sudo apt install openssh-server -y
sudo nano /etc/ssh/sshd_config
```
Asegurar:
```
PasswordAuthentication yes
PermitRootLogin no
```
```bash
sudo systemctl restart ssh
```
Crear usuario débil (laboratorio):
```bash
sudo useradd alumno
sudo passwd alumno
```

**MITRE:** T1110 https://attack.mitre.org/techniques/T1110/

---

### 5.2 Apache mal configurado
```bash
sudo apt install apache2 -y
sudo chmod -R 777 /var/www/html
```
- Logs: `/var/log/apache2/access.log`

**MITRE:** T1190 https://attack.mitre.org/techniques/T1190/

---

### 5.3 FTP anónimo
```bash
sudo apt install vsftpd -y
sudo nano /etc/vsftpd.conf
```
Activar:
```
anonymous_enable=YES
write_enable=YES
```
```bash
sudo systemctl restart vsftpd
```

**MITRE:** T1078 https://attack.mitre.org/techniques/T1078/

---

## 6. Configuración de servicios vulnerables – Windows

### 6.1 RDP
- Activar Escritorio remoto
- Crear usuario `alumno`
- Contraseña simple (laboratorio)

**MITRE:** T1021.001 https://attack.mitre.org/techniques/T1021/001/

---

### 6.2 SMB inseguro
- Crear carpeta compartida
- Permisos: lectura/escritura para Everyone

**MITRE:** T1021.002 https://attack.mitre.org/techniques/T1021/002/

---

## 7. Verificación de eventos en Wazuh

Dashboards clave:
- Security Events
- Authentication
- Integrity Monitoring

Filtros útiles:
- `agent.name`
- `rule.level > 7`
- `data.srcip`

---

## 8. Checklist final
- [ ] SOC operativo
- [ ] Agentes conectados
- [ ] SSH / FTP / Apache activos
- [ ] RDP / SMB activos
- [ ] Eventos visibles en dashboards

---

## 9. Endurecimiento tras la práctica
- Deshabilitar FTP anónimo
- Cambiar contraseñas
- Activar MFA
- Aplicar firewall restrictivo

