# Configuración del Servidor SOC Central

## Información del Servidor

**URL de Acceso Web:**
```
https://wazuh.soc.informatica.iesgrancapitan.org/
```

**Credenciales Wazuh:**
- Usuario: `admin`
- Contraseña: `bgCt6c9pHkPg+Z2OHL*Z.MCiEWo9rTWy`

## Configuración de Red - Servidor SOC

```yaml
network:
  version: 2
  renderer: networkd
  ethernets:
    ens18:
      dhcp4: no
      addresses:
        - 10.68.0.158/8
      gateway4: 10.0.0.8
      nameservers:
        addresses: [8.8.8.8, 1.1.1.1]
```

## Especificaciones del Servidor

- **Dirección IP**: 10.68.0.158
- **Máscara**: 255.0.0.0
- **Puerta de enlace**: 10.0.0.8
- **DNS primario**: 8.8.8.8
- **DNS secundario**: 1.1.1.1

## Servicios Corriendo

- **Wazuh Manager**: Puerto 1514 (TCP/UDP)
- **Elasticsearch**: Puerto 9200
- **Kibana**: Puerto 5601
- **Logstash**: Puerto 5000
- **API Wazuh**: Puerto 55000

## Notas de Configuración

1. El servidor está configurado con IP estática en la subred 10.68.0.0/8
2. Se utiliza cloud-init para la configuración de red
3. Los DNS públicos (Google y Cloudflare) garantizan conectividad
4. La puerta de enlace 10.0.0.8 conecta con la red principal del centro educativo

## Acceso de Administración

Para tareas de administración del servidor, contactar a los responsables del proyecto:
- Antonio Marinero Jabalera
- Santiago Quirós Arenas
- Mario Palacios Moreno
