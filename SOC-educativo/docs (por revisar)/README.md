# SOC Educativo - Documentación Adicional

## Guía de Inicio Rápido

### Instalación Rápida (30 minutos)

#### Prerrequisitos
```bash
# Sistema: Ubuntu Server 20.04 LTS
# RAM: 16 GB mínimo
# Almacenamiento: 100 GB

# Ejecutar instalación completa
sudo bash scripts/install_wazuh_manager.sh
sudo bash scripts/install_elk_stack.sh
```

#### Acceso
```
Kibana: http://localhost:5601
Usuario: elastic
Contraseña: changeme
```

### Agregar Primer Agente (5 minutos)

#### En servidor a monitorizar
```bash
sudo bash scripts/install_wazuh_agent.sh \
  -m 192.168.1.100 \
  -n "mi-servidor-01"
```

#### Verificar en Wazuh Manager
```bash
sudo /var/ossec/bin/agent_control -l
```

## Comandos Útiles

### Wazuh Manager
```bash
# Ver estado
systemctl status wazuh-manager

# Ver agentes conectados
/var/ossec/bin/agent_control -l

# Ver alertas recientes
tail -f /var/ossec/logs/alerts/alerts.json

# Ver reglas compiladas
/var/ossec/bin/wazuh-control rule-test
```

### Elasticsearch
```bash
# Ver estado del cluster
curl http://localhost:9200/_cluster/health?pretty

# Ver índices
curl http://localhost:9200/_cat/indices?v

# Eliminar índices antiguos
curl -X DELETE http://localhost:9200/wazuh-alerts-2024.*
```

### Logstash
```bash
# Ver logs de Logstash
tail -f /var/log/logstash/logstash-plain.log

# Probar configuración
/usr/share/logstash/bin/logstash -f /etc/logstash/conf.d/wazuh.conf --config.test_and_exit
```

### Kibana
```bash
# Ver logs de Kibana
sudo journalctl -u kibana -n 50

# Reiniciar Kibana
sudo systemctl restart kibana
```

## Solución de Problemas Comunes

### 1. Agentes no se conectan
```bash
# Verificar firewall
sudo ufw allow 1514/tcp
sudo ufw allow 1515/tcp

# Verificar conectividad
nc -zv <wazuh-manager-ip> 1514

# Ver logs del agente
tail -f /var/ossec/logs/ossec.log
```

### 2. Kibana no muestra datos
```bash
# Verificar índices
curl http://localhost:9200/_cat/indices?v

# Verificar conexión Elasticsearch
curl http://localhost:9200/

# Reiniciar Kibana y Elasticsearch
sudo systemctl restart elasticsearch kibana
```

### 3. Alerts no se generan
```bash
# Validar reglas
sudo /var/ossec/bin/wazuh-control rule-test

# Ver logs del manager
tail -f /var/ossec/logs/ossec.log

# Verificar que hay eventos
grep -c "msg" /var/ossec/logs/alerts/alerts.log
```

### 4. Alto consumo de memoria
```bash
# Ver proceso con mayor consumo
top -b -o +%MEM | head -n 20

# Reducir tamaño de logs
sed -i 's/<keep>1<\/keep>/<keep>5<\/keep>/' /var/ossec/etc/ossec.conf

# Reducir frecuencia de FIM
sed -i 's/<frequency>43200<\/frequency>/<frequency>86400<\/frequency>/' /var/ossec/etc/ossec.conf
```

## Mejores Prácticas

### 1. Mantenimiento Regular
```bash
# Backup semanal de configuraciones
tar czf /backup/wazuh-config-$(date +%Y%m%d).tar.gz \
  /var/ossec/etc/ /etc/elasticsearch/ /etc/kibana/

# Limpieza de logs antiguos
find /var/ossec/logs -mtime +30 -delete
```

### 2. Seguridad
```bash
# Cambiar contraseña por defecto
curl -X POST "localhost:9200/_security/user/elastic/_password" \
  -H "Content-Type: application/json" \
  -d '{"password": "nueva_contraseña"}'

# Restringir acceso a puertos
sudo ufw default deny incoming
sudo ufw allow 5601  # Kibana
sudo ufw allow 1514  # Wazuh agents
```

### 3. Monitorización
```bash
# Crear alerta si un agente se desconecta
# En Kibana: Alerts & Actions → Create alert

# Monitorizar espacio en disco
df -h / | awk 'NR==2 {if ($5 > 80) print "Disco al 80%"}'
```

## Escalabilidad

Para producción con múltiples agentes:

### 1. Cluster Elasticsearch
```yaml
cluster.name: wazuh-cluster
node.name: elasticsearch-node-1
discovery.seed_hosts: ["192.168.1.101", "192.168.1.102"]
cluster.initial_master_nodes: ["elasticsearch-node-1", "elasticsearch-node-2"]
```

### 2. Múltiples Logstash
```bash
# Balancear carga entre múltiples instancias
# Usar upstream en nginx o HAProxy
```

### 3. Wazuh Cluster
```xml
<cluster>
  <name>wazuh-cluster</name>
  <node_name>manager-1</node_name>
  <bind_addr>0.0.0.0</bind_addr>
  <port>1516</port>
</cluster>
```

## Integración con Herramientas Externas

### Slack
```bash
# Webhook de Slack
https://hooks.slack.com/services/YOUR/WEBHOOK/URL

# Script en active-response
/var/ossec/active-response/bin/slack-alert.sh
```

### PagerDuty
```bash
# Integración para alertas críticas
# Webhook de PagerDuty en ossec.conf
```

### SIEM Corporativo
```bash
# Syslog a servidor central
<remote>
  <connection>syslog</connection>
  <address>siem-central.example.com</address>
  <port>514</port>
  <protocol>udp</protocol>
</remote>
```

## Reporting

### Generar Reportes
```bash
# Reporte de alertas del mes
curl -s "localhost:9200/wazuh-alerts-*/_search?q=@timestamp:[now-1M TO now]" \
  | jq '.hits.hits | length'

# Reporte por severidad
curl -s "localhost:9200/wazuh-alerts-*/_search" -H "Content-Type: application/json" \
  -d '{
    "size": 0,
    "aggs": {
      "por_severidad": {
        "terms": {"field": "data.severity"}
      }
    }
  }'
```

### Planificar Reportes en Kibana
1. Stack Management → Reporting
2. Create report
3. Seleccionar dashboard/visualización
4. Programar envío por email

## Continuous Learning

### Recursos Recomendados
- [Documentación oficial Wazuh](https://documentation.wazuh.com/)
- [Elasticsearch Guide](https://www.elastic.co/guide/en/elasticsearch/reference/current/)
- [Kibana User Guide](https://www.elastic.co/guide/en/kibana/current/)
- [CIS Benchmarks](https://www.cisecurity.org/cis-benchmarks/)

### Certificaciones
- Wazuh Certified Professional
- Elastic Certified Professional
- CompTIA Security+

## Contacto y Soporte

### Equipo del Proyecto
- **Wazuh**: Antonio Marinero Jabalera
- **ELK**: Santiago Quirós Arenas
- **Integración**: Mario Palacios Moreno

### Recursos
- Issue Tracker: GitHub Issues
- Documentación: `/docs`
- Scripts: `/scripts`
- Pruebas: `/tests`

---

**Versión**: 1.0
**Última actualización**: Mayo 2026
**Licencia**: MIT
