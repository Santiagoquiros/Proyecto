# Guía de Usuario del Entorno SOC con Wazuh

## Introducción

Este documento describe la infraestructura del entorno SOC (Security Operations Center) desplegado para prácticas de monitorización, administración y análisis de seguridad utilizando la plataforma Wazuh.

La guía está orientada a administradores, docentes y estudiantes que necesiten:

- Acceder al panel de monitorización.
- Conectarse remotamente al servidor.
- Administrar los distintos equipos del laboratorio.
- Comprender la configuración de red utilizada.
- Verificar conectividad entre sistemas.
- Realizar tareas básicas de operación y mantenimiento.



# Arquitectura general del laboratorio

| Equipo | Función | Dirección IP |
|---|---|---|
| Servidor SOC/Wazuh | Monitorización y gestión centralizada | 10.68.0.158 |
| Ubuntu-cliente | Equipo Linux monitorizado | 10.68.0.161 |
| Windows-cliente | Equipo Windows monitorizado | 10.68.0.163 |
| Kali-atacante | Máquina de pruebas ofensivas | 10.68.0.165 |



# Acceso al Dashboard de Wazuh

## URL de acceso

```text
https://wazuh.soc.informatica.iesgrancapitan.org/
```

## Credenciales

| Campo | Valor |
|---|---|
| Usuario | admin |
| Contraseña | bgCt6c9pHkPg+Z2OHL*Z.MCiEWo9rTWy |

## Funciones principales del dashboard

- Visualización de alertas.
- Gestión de agentes.
- Revisión de logs.
- Detección de amenazas.
- Monitorización en tiempo real.
- Correlación de eventos.



# Acceso remoto al servidor SOC

## Conexión SSH

```bash
ssh -p 9313 administrador@cpd.informatica.iesgrancapitan.org
```

## Credenciales

| Campo | Valor |
|---|---|
| Usuario | administrador |
| Contraseña | Root1234$ |

## Explicación del comando

| Elemento | Descripción |
|---|---|
| ssh | Cliente SSH |
| -p 9313 | Puerto personalizado |
| administrador | Usuario remoto |
| cpd.informatica.iesgrancapitan.org | Servidor remoto |



# Acceso desde el servidor a las máquinas internas

## Ubuntu-cliente

### Conexión

```bash
ssh usuario@10.68.0.161
```

### Credenciales

| Campo | Valor |
|---|---|
| Usuario | usuario |
| Contraseña | Root1234$ |

### Función

Sistema Linux monitorizado por Wazuh para pruebas de seguridad y generación de logs.



## Windows-cliente

### Conexión

```bash
ssh usuario@10.68.0.163
```

### Credenciales

| Campo | Valor |
|---|---|
| Usuario | usuario |
| Contraseña | Root1234$ |

### Función

Equipo Windows destinado a la monitorización de eventos y pruebas de seguridad.



## Kali-atacante

### Conexión

```bash
ssh kali@10.68.0.165
```

### Credenciales

| Campo | Valor |
|---|---|
| Usuario | kali |
| Contraseña | Root1234$ |

### Función

Máquina utilizada para simulación de ataques y pruebas ofensivas.



# Configuración de red del servidor SOC

```yaml
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
```

## Explicación

- IP estática para asegurar conectividad estable.
- DNS públicos de Google y Cloudflare.
- Puerta de enlace centralizada.
- Configuración mediante Netplan y cloud-init.



# Configuración de red Ubuntu-cliente

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



# Configuración de red Windows-cliente

| Parámetro | Valor |
|---|---|
| IP | 10.68.0.163 |
| Máscara | 255.0.0.0 |
| Gateway | 10.0.0.8 |
| DNS | 8.8.8.8 / 1.1.1.1 |



# Configuración de red Kali Linux

| Parámetro | Valor |
|---|---|
| IP | 10.68.0.165 |
| Máscara | 255.0.0.0 |
| Gateway | 10.0.0.8 |
| DNS | 8.8.8.8 / 1.1.1.1 |



# Comprobaciones básicas

## Verificar conectividad

```bash
ping 10.68.0.161
ping 10.68.0.163
ping 10.68.0.165
```

## Verificar Internet

```bash
ping 8.8.8.8
```

## Verificar DNS

```bash
ping google.com
```



# Verificación de agentes Wazuh

Los agentes deben aparecer en estado:

```text
Active
```

## Aspectos a revisar

- Última conexión.
- Estado del agente.
- Logs recibidos.
- Alertas generadas.
- Inventario del sistema.


# Flujo de funcionamiento del laboratorio

1. Kali Linux genera actividad ofensiva.
2. Los clientes registran eventos.
3. Los agentes Wazuh envían logs al servidor.
4. Wazuh analiza los eventos.
5. El dashboard muestra alertas.
6. El administrador revisa los incidentes.



# Resolución de problemas

## SSH no funciona

```bash
systemctl status ssh
```

Revisar también:

- Firewall.
- Puerto SSH.
- Credenciales.
- Conectividad de red.

## El agente no aparece

Comprobar:

- Estado del servicio.
- Configuración IP.
- Conectividad con el servidor.
- Configuración del agente.



# Resumen de credenciales

| Servicio | Usuario | Contraseña |
|---|---|---|
| Dashboard Wazuh | admin | bgCt6c9pHkPg+Z2OHL*Z.MCiEWo9rTWy |
| SSH servidor | administrador | Root1234$ |
| Ubuntu cliente | usuario | Root1234$ |
| Windows cliente | usuario | Root1234$ |
| Kali atacante | kali | Root1234$ |

Credenciales del sistema Wazuh:

PI Wazuh (puerto 55000)wazuh        Root1234$
API Wazuh (interno)wazuh-wui            WazuhWui2026.
Dashboard → Indexerkibanaserver         WazuhDashboard2026Secure

# Resumen de direcciones IP

| Equipo | IP |
|---|---|
| Servidor Wazuh | 10.68.0.158 |
| Ubuntu-cliente | 10.68.0.161 |
| Windows-cliente | 10.68.0.163 |
| Kali-atacante | 10.68.0.165 |



# Conclusión

Este entorno SOC permite realizar prácticas reales de:

- Monitorización.
- Detección de amenazas.
- Análisis de logs.
- Administración remota.
- Simulación de ataques.
- Gestión de incidentes de seguridad.

Todo ello utilizando Wazuh como plataforma centralizada de seguridad.
