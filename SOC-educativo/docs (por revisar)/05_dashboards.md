# 5. Creación de Dashboards en Kibana

## Introducción


Los dashboards son la interfaz visual del SOC para monitorizar seguridad en tiempo real. Esta guía cubre la creación de dashboards temáticos en Kibana.

## Estructura de Dashboards

Se crearán 5 dashboards temáticos:

1. **Resumen General** - Vista completa del SOC
2. **Seguridad del SO** - Eventos del sistema operativo
3. **Autenticación y Acceso** - Login y autorización
4. **Eventos Críticos** - Alertas de nivel alto
5. **Timeline de Incidentes** - Análisis temporal


## Acceder a Kibana

```
URL: http://<IP-SERVIDOR>:5601
Usuario: elastic
Contraseña: changeme
```

## Dashboard 1: Resumen General

### Crear Dashboard
1. En Kibana, ir a **Dashboard** → **Create new dashboard**
2. Nombre: `SOC - Resumen General`
3. Añadir visualizaciones:

### Visualizaciones a Añadir

#### 1. Total de Eventos (Number)
```
Index: wazuh-alerts-*
Metric: Count
```

#### 2. Alertas por Severidad (Pie Chart)
```
Index: wazuh-alerts-*
Aggregation: Terms
Field: data.severity
Size: 5
```

#### 3. Agentes Activos (Number)
```
Index: wazuh-alerts-*
Metric: Unique count de agent.name
```

#### 4. Eventos por Agente (Bar Chart)
```
Index: wazuh-alerts-*
X-axis: Terms (agent.name)
Y-axis: Count
```

#### 5. Eventos Recientes (Table)
```
Columnas:
- timestamp
- agent.name
- rule.id
- rule.description
- data.severity
Límite: 10 filas
```

## Dashboard 2: Seguridad del SO

### Nombre: `SOC - Seguridad del SO`

### Visualizaciones

#### 1. Cambios de Configuración (Timeline)
```
Index: wazuh-alerts-*
Filtro: rule.groups: "fim_"
Agregación temporal: 1h
```

#### 2. Modificación de Archivos (Table)
```
Datos:
- timestamp
- agent.name
- file.path
- action
Filtro: rule.id: "6001" OR "6002" OR "6003"
```

#### 3. Software Instalado/Desinstalado (Bar Chart)
```
Index: wazuh-alerts-*
Filtro: rule.groups: "software_change"
Términos: data.package
```

#### 4. Cambios de Permisos (Number)
```
Métrica: Count
Filtro: rule.id: "6002"
```

#### 5. Timeline de Cambios (Time Series)
```
Métrica: Count
Agregación temporal: 30 min
Desglose: rule.description
```

## Dashboard 3: Autenticación y Acceso

### Nombre: `SOC - Autenticación y Acceso`

### Visualizaciones

#### 1. Intentos Fallidos (Number)
```
Métrica: Count
Filtro: rule.groups: "authentication_failures"
```

#### 2. Intentos Fallidos por Usuario (Bar Chart)
```
Index: wazuh-alerts-*
Términos: data.user
Filtro: rule.groups: "authentication_failures"
Top 10
```

#### 3. Intentos Fallidos por IP (Geo-map)
```
Métrica: Count de geoip.location
Filtro: rule.groups: "authentication_failures"
```

#### 4. Login Exitosos (Number)
```
Métrica: Count
Filtro: rule.groups: "authentication_success"
```

#### 5. Análisis de Fuerza Bruta (Table)
```
Columnas:
- data.source_ip
- data.user
- Count
- Timestamp
Filtro: rule.id: "5711"
```

#### 6. Cambios de Usuario (Timeline)
```
Métrica: Count
Filtro: rule.groups: "account_changed"
Agregación: 1h
```

## Dashboard 4: Eventos Críticos

### Nombre: `SOC - Eventos Críticos`

### Visualizaciones

#### 1. Eventos Críticos (Number)
```
Métrica: Count
Filtro: severity: >= 12
```

#### 2. Alertas Críticas por Tipo (Pie Chart)
```
Términos: rule.groups
Filtro: severity: >= 12
```

#### 3. Detección de Malware (Table)
```
Columnas:
- timestamp
- agent.name
- data.virname
- file.path
Filtro: rule.id: "9001"
```

#### 4. Cambios Críticos de Archivo (Alert Box)
```
Métrica: Count
Filtro: rule.id: ("6001" OR "6002")
```

#### 5. Violaciones de Seguridad (Timeline)
```
Métrica: Count
Filtro: rule.groups: "security_policy"
Agregación: 30 min
```

#### 6. Top 10 Alertas Críticas (Horizontal Bar)
```
Términos: rule.description
Filtro: severity: >= 12
Límite: 10
```

## Dashboard 5: Timeline de Incidentes

### Nombre: `SOC - Timeline de Incidentes`

### Visualizaciones

#### 1. Eventos por Hora (Line Chart)
```
Métrica: Count
Agregación temporal: 1h
Desglose: severity
```

#### 2. Evolución de Alertas (Area Chart)
```
Métrica: Count
Agregación temporal: 30 min
Desglose por términos: rule.groups
```

#### 3. Top 5 Agentes con Eventos (Table)
```
Columnas:
- agent.name
- Count
Ordenar: Count DESC
Límite: 5
```

#### 4. Mapa de Red de Eventos (Geo Map)
```
Métrica: Count de geoip.location
Filtro: data.srcip: *
```

#### 5. Detalles de Evento Seleccionado (Table)
```
Columnas:
- timestamp
- agent.name
- rule.id
- rule.description
- rule.level
- data.full_log
```

## Exportar Dashboards

### Exportar como JSON
```bash
# En Kibana:
1. Dashboard → Seleccionar dashboard
2. Botón "..." → Share → Copy iFrame code
3. O ir a Management → Stack Management → Saved Objects → Export
```

### Guardar en Archivo
Los dashboards se pueden guardar en:
```
dashboards/security_dashboard.json
dashboards/authentication_dashboard.json
dashboards/incidents_dashboard.json
```

## Personalización

### Cambiar Tema
```
Kibana → Stack Management → Advanced Settings → Dark mode
```

### Agregar Filtros Globales
1. En dashboard, click en "Add filter"
2. Field: `agent.name` or `severity`
3. Operator: `is`
4. Value: Seleccionar valores

### Crear Alertas desde Kibana
```
1. Discover → Buscar datos
2. Click "Alerts" → Create alert
3. Condición: Count >= 5
4. Notificación: Email, Slack, etc.
```

## Consultas Útiles (KQL)

```
# Solo eventos críticos
severity >= 12

# Eventos de un agente específico
agent.name: "linux-01"

# Intentos de acceso fallidos
rule.groups: "authentication_failures"

# Cambios en archivos críticos
rule.id: "6001" OR rule.id: "6002"

# Eventos en las últimas 24 horas
@timestamp: [now-24h TO now]

# Múltiples condiciones
rule.groups: "malware" AND severity >= 12
```

## Visualizaciones Avanzadas

### Heatmap de Actividad
```
Métrica X: agent.name
Métrica Y: data.severity
Valor celda: Count
```

### Sankey Diagram
```
Source: agent.name
Target: rule.groups
Valor: Count
```

### Gauge Chart
```
Métrica: Count de Critical Alerts
Max: 100
Colores: 0-20 Green, 20-50 Yellow, 50+ Red
```

## Compartir Dashboards

### Generar Link
```
Dashboard → Share → Copy link
```

### Exportar para Presentaciones
```
Dashboard → Share → PDF
Dashboard → Share → PNG
```

## Troubleshooting

### No aparecen datos en visualizaciones
```
1. Verificar índices creados:
   Management → Saved Objects → Indices

2. Comprobar filtro temporal

3. Verificar que hay eventos:
   Kibana → Discover
```

### Visualización lenta
```
1. Reducir rango temporal
2. Usar filtros más específicos
3. Agregación con menos términos
```

### Error de conexión
```
1. Verificar Elasticsearch está corriendo:
   curl http://localhost:9200

2. Verificar credenciales en kibana.yml

3. Reiniciar Kibana:
   sudo systemctl restart kibana
```

## Siguiente Paso

Continúa con [Pruebas y Simulación de Incidentes](06_pruebas_simulacion.md)

---

**Última actualización**: Mayo 2026
