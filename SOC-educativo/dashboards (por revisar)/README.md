# Dashboards de Kibana

Este directorio contiene exportaciones de dashboards Kibana para visualizar datos del SOC.

## Dashboards Incluidos

### security_dashboard.json
**Nombre**: SOC - Seguridad del SO
**Descripción**: Monitoriza cambios en configuración del sistema, archivos críticos y software.
**Visualizaciones**: 5
**Temas**: FIM, Software changes, Permisos

### authentication_dashboard.json
**Nombre**: SOC - Autenticación y Acceso
**Descripción**: Monitoriza intentos de acceso, fallos de autenticación y cambios de usuarios.
**Visualizaciones**: 6
**Temas**: Login failures, Fuerza bruta, Cambios de usuario

### incidents_dashboard.json
**Nombre**: SOC - Eventos Críticos
**Descripción**: Alertas críticas, detección de malware y violaciones de seguridad.
**Visualizaciones**: 6
**Temas**: Malware, Cambios críticos, Violaciones

### general_dashboard.json
**Nombre**: SOC - Resumen General
**Descripción**: Vista general de todo el SOC con métricas principales.
**Visualizaciones**: 5
**Temas**: Total eventos, Agentes activos, Alertas por severidad

## Cómo Importar

### Método 1: Interfaz Kibana
1. Ir a **Stack Management** → **Saved Objects**
2. Click en **Import**
3. Seleccionar archivo JSON
4. Click en **Import**

### Método 2: API
```bash
curl -X POST "localhost:5601/api/saved_objects/dashboard" \
  -H 'Content-Type: application/json' \
  -H 'kbn-xsrf: true' \
  -d @security_dashboard.json
```

## Personalización

Cada dashboard puede personalizarse:
- Cambiar rangos de tiempo
- Ajustar filtros
- Agregar/remover visualizaciones
- Cambiar colores y temas

Ver documentación `/docs/05_dashboards.md` para detalles.
