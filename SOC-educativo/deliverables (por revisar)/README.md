# Entregables del Proyecto

Este directorio contiene los entregables finales del SOC educativo.

## Documentación

### 1. Manual de Instalación
**Archivo**: `installation_manual.pdf`
**Contenido**:
- Requisitos de hardware
- Paso a paso de instalación
- Configuración inicial
- Troubleshooting

**Ubicación**: Generado desde `/docs/01_*.md`

### 2. Informe de Arquitectura
**Archivo**: `architecture_diagram.pdf`
**Contenido**:
- Diagrama de componentes
- Flujo de datos
- Tabla de puertos
- Topología de red

**Ubicación**: Basado en `ARCHITECTURE.md`

### 3. Reporte de Pruebas
**Archivo**: `test_report.pdf`
**Contenido**:
- Resultados de simulaciones
- Matriz de detección
- Análisis de rendimiento
- Recomendaciones

**Ubicación**: Generado desde `/tests`

### 4. Guía de Operaciones
**Archivo**: `operations_guide.pdf`
**Contenido**:
- Uso diario del SOC
- Dashboards principales
- Interpretación de alertas
- Respuesta a incidentes

**Ubicación**: Compilado desde documentación

### 5. Guía de Mantenimiento
**Archivo**: `maintenance_guide.pdf`
**Contenido**:
- Backups y recuperación
- Actualización de componentes
- Optimización de performance
- Troubleshooting avanzado

## Configuraciones

### Exportaciones de Wazuh
- `wazuh_manager_config.xml` - Configuración Manager
- `wazuh_agent_config.xml` - Configuración Agentes
- `wazuh_rules.xml` - Reglas personalizadas

### Exportaciones de Kibana
- `kibana_dashboards.json` - Todos los dashboards
- `kibana_indices.json` - Configuración de índices

### Ansible Playbooks
- `playbook_complete_installation.yml` - Instalación completa

## Media

### Videos
- `demo_login_attack.mp4` - Demostración de detección de fuerza bruta
- `demo_malware_detection.mp4` - Detección de malware
- `demo_dashboard_overview.mp4` - Tour por dashboards

### Imágenes
- `screenshot_wazuh_manager.png`
- `screenshot_kibana_dashboard.png`
- `screenshot_alert_example.png`

## Presentación

### slides.pptx
Presentación ejecutiva del proyecto con:
- Contexto y justificación
- Arquitectura y componentes
- Resultados de pruebas
- Conclusiones y recomendaciones
- Demo funcional

## Checklist de Entrega

- [ ] Documentación completa en `/docs`
- [ ] Dashboards funcionales en Kibana
- [ ] Alertas configuradas y testadas
- [ ] Agentes conectados y monitorizando
- [ ] Logs procesados en Elasticsearch
- [ ] Simulaciones de incidentes completadas
- [ ] Reportes de test generados
- [ ] Presentación preparada

## Cómo Generar Entregables

### PDF desde Markdown
```bash
# Instalar pandoc
sudo apt-get install -y pandoc

# Generar PDF
pandoc docs/*.md -o deliverables/complete_documentation.pdf
```

### Presentación
```bash
# Editar slides.pptx con:
# LibreOffice Impress (Linux)
# Microsoft PowerPoint (Windows)
# Google Slides (Online)
```

### Videos de Demo
```bash
# Grabar terminal con asciinema
asciinema rec demo_login_attack.cast
asciinema upload demo_login_attack.cast

# O usar OBS Studio para grabación de pantalla
```

## Distribución

### Para Evaluadores
1. `README.md` del proyecto
2. `installation_manual.pdf`
3. `architecture_diagram.pdf`
4. Acceso a demostración en vivo

### Para Estudiantes
1. Documentación completa en `/docs`
2. Scripts en `/scripts`
3. Configuraciones en `/agents` y `/ansible`
4. Guías de pruebas en `/tests`

### Para Producción
1. Playbooks Ansible completos
2. Configuraciones optimizadas
3. Scripts de backup/recuperación
4. Documentación de mantenimiento

## Versionado

Todos los entregables incluyen:
- Fecha de creación
- Versión del SOC (v4.x de Wazuh, v7.x de ELK)
- Autores
- Changelog

---

**Proyecto**: Implantación SOC Educativo con Wazuh + ELK
**Equipo**: Antonio Marinero, Santiago Quirós, Mario Palacios
**Fecha**: Enero 2025
