# 2. Instalación de ELK Stack

## Introducción

ELK Stack (Elasticsearch, Logstash, Kibana) es la plataforma de procesamiento y visualización de logs del SOC. Esta guía describe la instalación en Ubuntu Server 24.04 LTS.

### Instalación de Java
```bash
sudo apt-get update
sudo apt-get install -y openjdk-11-jdk

# Verificar instalación
java -version
```

## Instalación de Elasticsearch

### 1. Agregar Repositorio
```bash
# Añadir clave GPG
wget -qO - https://artifacts.elastic.co/GPG-KEY-elasticsearch | sudo apt-key add -

# Instalar paquete de repositorio
sudo apt-get install -y apt-transport-https

# Agregar repositorio
echo "deb https://artifacts.elastic.co/packages/7.x/apt stable main" | \
  sudo tee /etc/apt/sources.list.d/elastic-7.x.list

# Actualizar repositorios
sudo apt-get update
```

### 2. Instalar Elasticsearch
```bash
# Instalar Elasticsearch
sudo apt-get install -y elasticsearch

# Crear directorio de datos si no existe
sudo mkdir -p /var/lib/elasticsearch
sudo chown -R elasticsearch:elasticsearch /var/lib/elasticsearch
```

### 3. Configurar Elasticsearch

Archivo: `/etc/elasticsearch/elasticsearch.yml`

```yaml
# Nombre del cluster
cluster.name: wazuh-cluster

# Nombre del nodo
node.name: elasticsearch-node-1

# Rutas de datos
path.data: /var/lib/elasticsearch
path.logs: /var/log/elasticsearch

# Red
network.host: 0.0.0.0
http.port: 9200
transport.port: 9300

# Configuración de nodo
node.master: true
node.data: true

# Configuración de recuperación
gateway.recover_after_nodes: 1
gateway.expected_nodes: 1
gateway.recover_after_time: 5m

# Ajustes de performance
action.auto_create_index: "+wazuh-*,+.watches,-*"
discovery.type: single-node
```

### 4. Iniciar Elasticsearch
```bash
# Habilitar e iniciar servicio
sudo systemctl daemon-reload
sudo systemctl enable elasticsearch
sudo systemctl start elasticsearch

# Esperar a que inicie (30 segundos)
sleep 30

# Verificar estado
sudo systemctl status elasticsearch

# Verificar conectividad
curl -X GET "localhost:9200/"
```

### 5. Verificar Instalación
```bash
# Ver nodos
curl -X GET "localhost:9200/_nodes?pretty"

# Ver índices
curl -X GET "localhost:9200/_cat/indices?v"

# Ver estado del cluster
curl -X GET "localhost:9200/_cluster/health?pretty"
```

## Instalación de Logstash

### 1. Instalar Logstash
```bash
sudo apt-get install -y logstash
```

### 2. Crear Pipeline de Wazuh

Archivo: `/etc/logstash/conf.d/wazuh.conf`

```ruby
# Input: Recibir logs de Wazuh Manager
input {
  tcp {
    host => "0.0.0.0"
    port => 5000
    codec => json
  }
  udp {
    host => "0.0.0.0"
    port => 5000
    codec => json
  }
}

# Filter: Procesar y enriquecer logs
filter {
  # Parsear logs JSON de Wazuh
  if [data][srcip] {
    geoip {
      source => "[data][srcip]"
      target => "geoip"
    }
  }
  
  # Agregar campos personalizados
  mutate {
    add_field => {
      "[@metadata][index_name]" => "wazuh-events-%{+YYYY.MM.dd}"
    }
  }

  # Extraer información de alertas críticas
  if [data][severity] >= 10 {
    mutate {
      add_field => {
        "alert_level" => "high"
      }
    }
  } else if [data][severity] >= 5 {
    mutate {
      add_field => {
        "alert_level" => "medium"
      }
    }
  } else {
    mutate {
      add_field => {
        "alert_level" => "low"
      }
    }
  }
}

# Output: Enviar a Elasticsearch
output {
  elasticsearch {
    hosts => ["localhost:9200"]
    index => "%{[@metadata][index_name]}"
    document_type => "_doc"
  }
  
  # Opcional: Enviar también a stdout para debugging
  # stdout {
  #   codec => rubydebug
  # }
}
```

### 3. Verificar Configuración
```bash
# Probar sintaxis de Logstash
/usr/share/logstash/bin/logstash -f /etc/logstash/conf.d/wazuh.conf --config.test_and_exit

# Resultado esperado: "Configuration OK"
```

### 4. Iniciar Logstash
```bash
# Habilitar e iniciar servicio
sudo systemctl daemon-reload
sudo systemctl enable logstash
sudo systemctl start logstash

# Esperar a que inicie (1 minuto)
sleep 60

# Verificar estado
sudo systemctl status logstash

# Ver logs
tail -f /var/log/logstash/logstash-plain.log
```

## Instalación de Kibana

### 1. Instalar Kibana
```bash
sudo apt-get install -y kibana
```

### 2. Configurar Kibana

Archivo: `/etc/kibana/kibana.yml`

```yaml
# Puerto de acceso
server.port: 5601

# Host de Kibana
server.host: "0.0.0.0"

# URL base
server.basePath: ""

# Host de Elasticsearch
elasticsearch.hosts: ["http://localhost:9200"]

# Nombre de usuario Elasticsearch (si aplica)
# elasticsearch.username: "elastic"
# elasticsearch.password: "changeme"

# Timeout de índice
elasticsearch.requestTimeout: 40000

# Índice por defecto en Kibana
kibana.defaultAppId: "discover"

# Habilitar logging
logging.verbose: false
```

### 3. Iniciar Kibana
```bash
# Habilitar e iniciar servicio
sudo systemctl daemon-reload
sudo systemctl enable kibana
sudo systemctl start kibana

# Esperar a que inicie (30 segundos)
sleep 30

# Verificar estado
sudo systemctl status kibana
```

### 4. Acceder a Kibana
```
URL: http://<IP-SERVIDOR>:5601
Usuario: elastic (por defecto)
Contraseña: changeme (por defecto)
```

## Configuración de Seguridad

### Habilitar X-Pack Security (Opcional)
```bash
# Generar contraseñas de bootstrap
/usr/share/elasticsearch/bin/elasticsearch-setup-passwords auto

# O con contraseñas interactivas
/usr/share/elasticsearch/bin/elasticsearch-setup-passwords interactive
```

### Restringir Acceso a Puertos
```bash
# Firewall: Solo IP autorizadas pueden acceder
sudo ufw allow from 192.168.1.0/24 to any port 9200  # Elasticsearch
sudo ufw allow from 192.168.1.0/24 to any port 5601  # Kibana
sudo ufw allow from 192.168.1.0/24 to any port 5000  # Logstash
```

## Configuración de Índices

### Crear Índice de Wazuh
```bash
# Crear índice manual (normalmente Logstash lo hace automáticamente)
curl -X PUT "localhost:9200/wazuh-events-2025.01.01" -H 'Content-Type: application/json' -d'{
  "settings": {
    "number_of_shards": 1,
    "number_of_replicas": 0,
    "index.refresh_interval": "30s"
  },
  "mappings": {
    "properties": {
      "timestamp": { "type": "date" },
      "severity": { "type": "integer" },
      "agent_name": { "type": "keyword" },
      "data": { "type": "object", "enabled": false }
    }
  }
}'
```

### Lifecycle Management (Opcional)
```bash
# Crear política de retención de 90 días
curl -X PUT "localhost:9200/_ilm/policy/wazuh-policy" -H 'Content-Type: application/json' -d'{
  "policy": "wazuh-policy",
  "phases": {
    "hot": {
      "min_age": "0ms",
      "actions": {
        "rollover": {
          "max_primary_shard_size": "50gb",
          "max_age": "30d"
        }
      }
    },
    "delete": {
      "min_age": "90d",
      "actions": {
        "delete": {}
      }
    }
  }
}'
```

## Verificación Final

### Test de Conectividad
```bash
# Verificar que todos los servicios están corriendo
sudo systemctl status elasticsearch logstash kibana

# Verificar puertos
netstat -tlnp | grep -E "9200|5601|5000"

# Test de Elasticsearch
curl -X GET "localhost:9200/_cluster/health?pretty"

# Test de Kibana (esperar respuesta)
curl -I http://localhost:5601

# Test de Logstash
ps aux | grep logstash
```

## Troubleshooting

### Elasticsearch no inicia
```bash
# Ver logs
sudo journalctl -u elasticsearch -n 50

# Liberar memoria virtual
sudo sysctl -w vm.max_map_count=262144

# Hacer permanente
echo "vm.max_map_count=262144" | sudo tee -a /etc/sysctl.conf
```

### Logstash no procesa logs
```bash
# Ver logs detallados
tail -f /var/log/logstash/logstash-plain.log

# Validar configuración nuevamente
/usr/share/logstash/bin/logstash -f /etc/logstash/conf.d/wazuh.conf --config.test_and_exit

# Reiniciar con verbose
sudo systemctl stop logstash
sudo -u logstash /usr/share/logstash/bin/logstash -f /etc/logstash/conf.d/wazuh.conf
```

### Kibana no conecta a Elasticsearch
```bash
# Verificar conectividad
curl -u elastic:changeme http://localhost:9200

# Revisar logs de Kibana
sudo journalctl -u kibana -n 50

# Reiniciar servicios
sudo systemctl restart elasticsearch
sudo systemctl restart kibana
```

## Siguiente Paso

Continúa con [Configuración de Agentes Wazuh](03_configuracion_agentes.md)

---

**Última actualización**: Mayo 2026
