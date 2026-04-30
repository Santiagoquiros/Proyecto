# Información completa del servidor SOC y configuración de red

1. Acceso web del servidor

        Dashboard del Wazuh:

        https://wazuh.soc.informatica.iesgrancapitan.org/

        Credenciales para acceso a Wazuh:

        - Usuario: admin
        - Password: bgCt6c9pHkPg+Z2OHL*Z.MCiEWo9rTWy



2. Acceso remoto desde el exterior

Para conectarse al servidor desde fuera del centro, desde una terminal ejecutamos lo siguiente:

    ssh -p 9313 administrador@cpd.informatica.iesgrancapitan.org

    - Usuario: administrador
    - Password: Root1234$

Desde el propio servidor nos conectaremos via ssh a cada una de las maquinas:

    Ubuntu-cliente:  

    ssh usuario@10.68.0.161 -----> Root1234$

    Windows-cliente:

    ssh usuario@10.68.0.163 -----> Root1234$

    kali-atacante:

    ssh kali@10.68.0.165 -----> Root1234$

3. Configuración de red del servidor (cloud-init)

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

Configuración de red del cliente ubuntu:

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

Configuración de red del cliente windows:
    ip: 10.68.0.163
      mascara: 255.0.0.0
        puerta de enlace: 10.0.0.8
          dns: 8.8.8.8 / 1.1.1.1

Configuración de red del atacante kali:
    ip: 10.68.0.165
      mascara: 255.0.0.0
        puerta de enlace: 10.0.0.8
          dns: 8.8.8.8 / 1.1.1.1

