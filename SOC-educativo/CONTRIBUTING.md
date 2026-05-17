# Guía de Contribuciones

## Cómo Contribuir

Este documento describe cómo contribuir al proyecto de implantación del SOC educativo con Wazuh + ELK.

## Equipo del Proyecto

- **Antonio Marinero Jabalera** - Responsable de Wazuh
- **Santiago Quirós Arenas** - Responsable de ELK Stack
- **Mario Palacios Moreno** - Responsable de Integración y Documentación

## Tipos de Contribuciones

### Documentación
- Mejorar manuales de instalación
- Crear guías de troubleshooting
- Documentar nuevas configuraciones
- Traducir documentación

### Configuración
- Crear nuevas reglas de alertas
- Diseñar dashboards mejorados
- Optimizar configuraciones de Logstash
- Crear playbooks Ansible

### Pruebas
- Simular nuevos tipos de ataques
- Documentar comportamiento del sistema
- Validar alertas
- Pruebas de rendimiento

### Scripts
- Automatizar instalación
- Crear scripts de deployment
- Herramientas de análisis
- Scripts de mantenimiento

## Proceso de Contribución

### 1. Crear Rama Feature
```bash
git checkout -b feature/descripcion-breve
# Ejemplo: feature/nuevas-reglas-auth
```

### 2. Realizar Cambios
- Seguir la estructura existente
- Mantener consistencia en nomenclatura
- Agregar comentarios donde sea necesario
- Documentar cambios importantes

### 3. Commit Descriptivos
```bash
git add .
git commit -m "type: descripción corta

Descripción más detallada si es necesario.
- Punto 1
- Punto 2
"
```

**Tipos de commits**:
- `feat:` Nuevas funcionalidades
- `fix:` Correcciones de bugs
- `docs:` Cambios en documentación
- `style:` Formato y estilo
- `refactor:` Refactorización de código
- `perf:` Mejoras de rendimiento
- `test:` Adición de pruebas
- `chore:` Tareas de mantenimiento

### 4. Push a tu Fork
```bash
git push origin feature/descripcion-breve
```

### 5. Crear Pull Request
- Describir cambios claramente
- Referenciar issues relacionados
- Explicar por qué es necesario el cambio
- Proporcionar ejemplos si es posible

## Estándares de Código

### Documentación
- Usar Markdown para documentos
- Mantener consistencia de formato
- Incluir ejemplos prácticos
- Actualizar tabla de contenidos

### Configuración YAML
- Indentar con 2 espacios
- Usar comillas simples para strings
- Comentar secciones complejas
- Validar sintaxis YAML

### Scripts Bash
```bash
#!/bin/bash
# Comentario descriptivo
set -euo pipefail  # Exit on error, undefined vars, pipe errors

# Funciones primero
function_name() {
    local var="value"
    # Code
}

# Main execution
main() {
    # Code
}

main "$@"
```

### Configuración JSON
- Indentar con 4 espacios
- Usar comillas dobles
- Validar con `jq` o similar
- Incluir comentarios en archivos relacionados

## Pruebas

### Antes de hacer Pull Request

1. **Validar Sintaxis**
   ```bash
   # YAML
   yamllint <archivo>
   
   # JSON
   jq . <archivo>
   ```

2. **Pruebas Funcionales**
   - Verificar que la configuración funciona
   - Probar con casos de uso reales
   - Validar en entorno similar al de producción

3. **Documentación**
   - Actualizar README si es necesario
   - Documentar nuevos parámetros
   - Incluir ejemplos

## Estructura de Archivos

```
SOC-educativo/
├── docs/
│   ├── 01_instalacion_wazuh.md
│   ├── 02_instalacion_elk.md
│   ├── 03_configuracion_agentes.md
│   ├── 04_configuracion_alertas.md
│   ├── 05_dashboards.md
│   └── 06_pruebas_simulacion.md
├── agents/
│   ├── wazuh_agent_config.conf
│   ├── agent_alerts_rules.xml
│   └── decoders/
├── ansible/
│   ├── playbook_install_wazuh.yml
│   ├── playbook_install_elk.yml
│   └── roles/
├── dashboards/
│   ├── security_dashboard.json
│   ├── authentication_dashboard.json
│   └── incidents_dashboard.json
├── scripts/
│   ├── install_wazuh_manager.sh
│   ├── install_elk_stack.sh
│   ├── install_wazuh_agent.sh
│   └── utils/
├── tests/
│   ├── test_login_attacks.sh
│   ├── test_file_integrity.sh
│   ├── test_malware_detection.sh
│   └── test_suspicious_traffic.sh
├── deliverables/
│   ├── architecture_diagram.pdf
│   ├── installation_manual.pdf
│   └── test_report.pdf
└── [archivos principales del proyecto]
```

### Convenciones de Nomenclatura

- **Documentos**: `NN_descripcion_corta.md` (01, 02, 03...)
- **Scripts**: `nombre_descriptivo.sh` (minúsculas, guiones)
- **Configuraciones**: `nombre_componente_config.conf`
- **Reglas**: `nombre_regla_tipo.xml`
- **Dashboards**: `nombre_dashboard.json`

## Checklist para Pull Requests

- [ ] He actualizado la documentación relevante
- [ ] He testeado los cambios localmente
- [ ] He validado la sintaxis (YAML, JSON, Bash)
- [ ] He agregado comentarios donde es necesario
- [ ] Mi código sigue los estándares del proyecto
- [ ] He actualizado el changelog si es aplicable
- [ ] He verificado que no hay conflictos de merge

## Comunicación

- **Issues**: Para reportar bugs o sugerir features
- **Discussions**: Para preguntas y discusiones generales
- **Pull Requests**: Para proponer cambios

## Recursos Útiles

- [Documentación de Wazuh](https://documentation.wazuh.com/)
- [Elasticsearch Documentation](https://www.elastic.co/guide/en/elasticsearch/reference/current/index.html)
- [Kibana User Guide](https://www.elastic.co/guide/en/kibana/current/index.html)
- [Git Workflow](https://guides.github.com/introduction/flow/)

## Preguntas Frecuentes

### ¿Cómo reporto un bug?
Abre un Issue describiendo:
- Pasos para reproducir
- Resultado esperado
- Resultado actual
- Entorno (SO, versiones)

### ¿Cómo sugiero una mejora?
Abre una Discussion o Issue con etiqueta `enhancement` describiendo:
- La mejora propuesta
- Por qué es útil
- Posibles implementaciones

### ¿Quién revisa los Pull Requests?
Los mantainers del proyecto:
- Antonio Marinero Jabalera
- Santiago Quirós Arenas
- Mario Palacios Moreno

## Aceptación de Contribuciones

Los cambios deben cumplir:
1. Ser relevantes al proyecto
2. Mantener la calidad del código
3. Estar bien documentados
4. Incluir pruebas cuando sea aplicable
5. No romper funcionalidad existente

## Código de Conducta

Nos comprometemos a mantener un entorno respetuoso y profesional. Por favor:
- Sé respetuoso en todas las interacciones
- Proporciona crítica constructiva
- Acepta crítica con profesionalismo
- Reporta comportamiento inapropiado

## Contacto

Para dudas sobre contribuciones, puedes:
- Abrir una Discussion en el repositorio
- Contactar a los creadores directamente

---

¡Gracias por tu interés en contribuir!
