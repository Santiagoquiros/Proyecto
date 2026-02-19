# Información completa del servidor SOC y configuración de red

1. Credenciales de Proxmox

Usuario SOC: soc

Contraseña: soc2026

2. Acceso web del servidor

Dashboard del Wazuh:  

https://soc.informatica.iesgrancapitan.org/

3. Acceso remoto desde el exterior

Para conectarse al servidor desde fuera del centro, desde una terminal ejecutamos lo siguiente:

ssh -p 9313 administrador@cpd.informatica.iesgrancapitan.org

Contraseña del usuario administrador:  Root1234$

4. Configuración de red del servidor (cloud-init)

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

5. Credenciales para acceso a Wazuh:



