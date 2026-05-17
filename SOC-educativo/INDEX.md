# ÍNDICE COMPLETO - SOC Educativo

## Inicio Rápido

**Para empezar inmediatamente:**
1. Lee: `README.md` (5 minutos)
2. Lee: `ARCHITECTURE.md` (10 minutos)
3. Sigue: `docs/01_instalacion_wazuh.md` (30 minutos)

---

## GUÍAS POR TEMA

### Arquitectura y Diseño
- `ARCHITECTURE.md` - Diagrama y componentes del SOC
- `config.yml` - Configuración centralizada del proyecto
- `docs/README.md` - Guía técnica general

### Instalación
- `docs/01_instalacion_wazuh.md` - Wazuh Manager
- `docs/02_instalacion_elk.md` - Elasticsearch, Logstash, Kibana
- `docs/03_configuracion_agentes.md` - Agentes en Linux y Windows

### Configuración
- `docs/04_configuracion_alertas.md` - Sistema de alertas
- `docs/05_dashboards.md` - Dashboards en Kibana
- `agents/README.md` - Configuración de agentes

### Pruebas
- `docs/06_pruebas_simulacion.md` - Ataques simulados
- `tests/README.md` - Scripts de prueba

### Desarrollo
- `CONTRIBUTING.md` - Cómo contribuir
- `ansible/README.md` - Automatización con Ansible
- `scripts/README.md` - Scripts de instalación

### Entrega
- `deliverables/README.md` - Entregables finales
- `dashboards/README.md` - Dashboards de Kibana

---

## ARCHIVOS PRINCIPALES

| Archivo | Descripción | Audiencia |
|---------|-------------|-----------|
| **README.md** | Visión general del proyecto + contexto TFG | Todos |
| **ARCHITECTURE.md** | Diagrama, componentes y especificaciones de VMs | Técnicos |
| **CONTRIBUTING.md** | Guía de contribuciones | Desarrolladores |
| **LICENSE** | Licencia MIT | Todos |
| **config.yml** | Configuración centralizada | Administradores |
| **RESUMEN.md** | Resumen de documentación | Todos |
| **INDEX.md** | Este archivo (índice) | Todos |
| **docs/07_escenarios_mitre_attack.md** | **NEW**: Escenarios de ataque + MITRE ATT&CK | Analistas |
| **docs/08_marco_academico.md** | **NEW**: Competencias, objetivos, evaluación | Profesores |
| **docs/09_escalabilidad_futuro.md** | **NEW**: Roadmap de mejoras y expansión | Arquitectos |

---

## DOCUMENTACIÓN TÉCNICA

### Guía 1: Instalación de Wazuh Manager
**Archivo**: `docs/01_instalacion_wazuh.md`
**Temas**:
- ✓ Requisitos previos
- ✓ Instalación paso a paso
- ✓ Configuración de red
- ✓ Configuración de alertas
- ✓ Reglas y decodificadores
- ✓ Troubleshooting

### Guía 2: Instalación de ELK Stack
**Archivo**: `docs/02_instalacion_elk.md`
**Temas**:
- ✓ Instalación de Elasticsearch
- ✓ Instalación de Logstash
- ✓ Instalación de Kibana
- ✓ Configuración del pipeline
- ✓ Gestión de índices
- ✓ Seguridad y firewall

### Guía 3: Configuración de Agentes
**Archivo**: `docs/03_configuracion_agentes.md`
**Temas**:
- ✓ Instalación en Linux
- ✓ Instalación en Windows
- ✓ File Integrity Monitoring (FIM)
- ✓ Detección de rootkits
- ✓ Integración con ClamAV
- ✓ Verificación de conexión

### Guía 4: Configuración de Alertas
**Archivo**: `docs/04_configuracion_alertas.md`
**Temas**:
- ✓ Alertas de autenticación
- ✓ Alertas de integridad
- ✓ Alertas de red
- ✓ Alertas de malware
- ✓ Notificaciones por email
- ✓ Integración con Telegram y Slack

### Guía 5: Creación de Dashboards
**Archivo**: `docs/05_dashboards.md`
**Temas**:
- ✓ Dashboard: Resumen General
- ✓ Dashboard: Seguridad del SO
- ✓ Dashboard: Autenticación y Acceso
- ✓ Dashboard: Eventos Críticos
- ✓ Dashboard: Timeline de Incidentes
- ✓ Visualizaciones avanzadas

### Guía 6: Pruebas y Simulación
**Archivo**: `docs/06_pruebas_simulacion.md`
**Temas**:
- ✓ Test 1: Fuerza bruta
- ✓ Test 2: Cambios de archivo
- ✓ Test 3: Cambios de permisos
- ✓ Test 4: Software sospechoso
- ✓ Test 5: Detección de rootkit
- ✓ Test 6: Actividad de red
- ✓ Test 7: APT simulado

### Guía 7: Escenarios de Ataque y MITRE ATT&CK
**Archivo**: `docs/07_escenarios_mitre_attack.md`
**Temas**:
- ✓ Framework MITRE ATT&CK
- ✓ 8 escenarios de ataque educativos (Reconocimiento, Fuerza Bruta, Acceso Inicial, Escalada, Persistencia, Movimiento Lateral, Ejecución, Exfiltración)
- ✓ Tabla de mapeo: Ataque → Técnica MITRE → Evento
- ✓ 3 prácticas educativas para alumnado
- ✓ Indicadores de Compromiso (IoC)
- ✓ Escalabilidad futura

### Guía 8: Marco Académico - Objetivos y Competencias
**Archivo**: `docs/08_marco_academico.md`
**Temas**:
- ✓ Contexto del proyecto TFG
- ✓ Objetivos generales y secundarios
- ✓ 6 competencias técnicas principales
- ✓ 4 competencias transversales
- ✓ 24 horas de contenidos de aprendizaje
- ✓ Matriz de evaluación por competencia
- ✓ Rúbrica de calificación detallada
- ✓ Entregables requeridos

### Guía 9: Escalabilidad y Mejoras Futuras
**Archivo**: `docs/09_escalabilidad_futuro.md`
**Temas**:
- ✓ Roadmap de escalabilidad (Fases 1-4)
- ✓ Integraciones: Suricata, Telegram, Dashboards personalizados
- ✓ Análisis de comportamiento (UBA/UEBA)
- ✓ Automatización de respuesta (SOAR)
- ✓ Threat Intelligence
- ✓ Cumplimiento y auditoría
- ✓ Arquitectura escalada para 500+ agentes
- ✓ Escenarios de ataque avanzados

### Guía 10: Guía de Usuario del Entorno SOC (NUEVA)
**Archivo**: `docs/10_guia_usuario_entorno_soc.md`
**Temas**:
- ✓ Introducción y orientación
- ✓ Arquitectura general del laboratorio
- ✓ Acceso al Dashboard de Wazuh
- ✓ Acceso remoto SSH al servidor
- ✓ Acceso a máquinas internas (Ubuntu, Windows, Kali)
- ✓ Configuración de red detallada
- ✓ Comprobaciones básicas de conectividad
- ✓ Verificación de agentes Wazuh
- ✓ Flujo operativo del laboratorio
- ✓ Resolución de problemas
- ✓ Resumen de credenciales y direcciones IP
- ✓ Tareas comunes operativas

---

## ESTRUCTURA DE CARPETAS

```
SOC-educativo/
├── docs/                    ← Guías técnicas (10 GUÍAS)
│   ├── 01_instalacion_wazuh.md
│   ├── 02_instalacion_elk.md
│   ├── 03_configuracion_agentes.md
│   ├── 04_configuracion_alertas.md
│   ├── 05_dashboards.md
│   ├── 06_pruebas_simulacion.md
│   ├── 07_escenarios_mitre_attack.md          ← Ataque + MITRE ATT&CK
│   ├── 08_marco_academico.md                  ← Competencias TFG
│   ├── 09_escalabilidad_futuro.md             ← Roadmap
│   ├── 10_guia_usuario_entorno_soc.md         ← NUEVA: Operativa
│   └── README.md
│
├── agents/                  ← Configuración de agentes
├── ansible/                 ← Playbooks de Ansible
├── dashboards/              ← Dashboards JSON
├── scripts/                 ← Scripts de instalación
├── tests/                   ← Pruebas y simulación
├── deliverables/            ← Entregables
│
└── Archivos raíz:
    ├── README.md            ← Visión general + contexto TFG
    ├── ARCHITECTURE.md      ← Arquitectura + Especificaciones VMs
    ├── CONTRIBUTING.md      ← Contribuciones
    ├── LICENSE              ← Licencia MIT
    ├── config.yml           ← Configuración
    ├── RESUMEN.md           ← Resumen
    ├── CHANGELOG.md         ← Historial cambios
    ├── RESUMEN_EJECUTIVO_ACADEMICO.md ← Resumen académico
    └── INDEX.md             ← Este archivo
```

---

## CAMINOS DE APRENDIZAJE

### Ruta 1: Estudiante Principiante (Objetivo: Competencia Técnica)
**Duración**: 3-4 semanas
1. README.md (contexto TFG y proyecto)
2. ARCHITECTURE.md (especificaciones de VMs y requisitos)
3. docs/08_marco_academico.md (objetivos y competencias)
4. docs/01_instalacion_wazuh.md (instalación)
5. docs/02_instalacion_elk.md (stack ELK)
6. docs/03_configuracion_agentes.md (agentes multiplataforma)
7. docs/04_configuracion_alertas.md (sistema de alertas)
8. docs/05_dashboards.md (visualización)
9. docs/07_escenarios_mitre_attack.md (ataques educativos)
10. docs/06_pruebas_simulacion.md (simulación)

### Ruta 2: Administrador Experimentado (Objetivo: Implementación Rápida)
**Duración**: 1 semana
1. ARCHITECTURE.md
2. ansible/README.md (automatización)
3. scripts/README.md (scripts de instalación)
4. Implementar con automatización

### Ruta 3: Analista de Seguridad (Objetivo: Detección y Respuesta)
**Duración**: 2 semanas
1. docs/07_escenarios_mitre_attack.md (framework MITRE ATT&CK)
2. docs/08_marco_academico.md (competencias de análisis)
3. docs/04_configuracion_alertas.md (creación de reglas)
4. docs/06_pruebas_simulacion.md (simulación)
5. docs/05_dashboards.md (visualización de eventos)

### Ruta 4: Especialista en Seguridad (Objetivo: Auditoría y Cumplimiento)
**Duración**: 2-3 semanas
1. ARCHITECTURE.md
2. docs/08_marco_academico.md (marco de evaluación)
3. docs/07_escenarios_mitre_attack.md (cobertura MITRE)
4. docs/09_escalabilidad_futuro.md (cumplimiento y auditoría)
5. docs/05_dashboards.md (reportes ejecutivos)

### Ruta 5: Desarrollador/Contribuidor (Objetivo: Mejora del Proyecto)
**Duración**: 3-4 semanas
1. README.md
2. CONTRIBUTING.md
3. docs/07_escenarios_mitre_attack.md (nuevos escenarios)
4. docs/09_escalabilidad_futuro.md (mejoras)
5. Crear nuevas reglas, dashboards, playbooks

### Ruta 6: Profesor/Educador (Objetivo: Material Docente)
**Duración**: 1-2 semanas
1. docs/08_marco_academico.md (competencias y evaluación)
2. README.md (contexto de proyecto)
3. docs/07_escenarios_mitre_attack.md (prácticas)
4. docs/09_escalabilidad_futuro.md (mejoras futuras)

---

## BÚSQUEDA POR TEMA

### ¿Cómo instalar Wazuh Manager?
→ `docs/01_instalacion_wazuh.md`

### ¿Cómo instalar ELK Stack?
→ `docs/02_instalacion_elk.md`

### ¿Cómo instalar un agente?
→ `docs/03_configuracion_agentes.md`

### ¿Cómo crear alertas?
→ `docs/04_configuracion_alertas.md`

### ¿Cómo crear dashboards?
→ `docs/05_dashboards.md`

### ¿Cómo simular ataques?
→ `docs/06_pruebas_simulacion.md`

### ¿Cómo contribuir?
→ `CONTRIBUTING.md`

### ¿Cómo automatizar con Ansible?
→ `ansible/README.md`

### ¿Qué comandos útiles existen?
→ `docs/README.md` (Sección: Comandos Útiles)

### ¿Cómo resolver problemas?
→ `docs/README.md` (Sección: Solución de Problemas)

### ¿Cómo usar el entorno SOC operativamente?
→ **`docs/10_guia_usuario_entorno_soc.md`** (NUEVA)

### ¿Cuáles son las direcciones IP y credenciales?
→ **`docs/10_guia_usuario_entorno_soc.md`** (Tablas de resumen)

### ¿Cómo conectarse al servidor SOC remotamente?
→ **`docs/10_guia_usuario_entorno_soc.md`** (Sección: Acceso Remoto SSH)

### ¿Cómo acceder al Dashboard de Wazuh?
→ **`docs/10_guia_usuario_entorno_soc.md`** (Sección: Dashboard)

---

## COBERTURA DE TEMAS

| Tema | Guía | Sección |
|------|------|---------|
| Arquitectura | ARCHITECTURE.md | Diagrama completo |
| Wazuh Manager | docs/01 | Instalación y configuración |
| Elasticsearch | docs/02 | Instalación y setup |
| Logstash | docs/02 | Instalación y pipeline |
| Kibana | docs/02 | Instalación y acceso |
| Agentes Linux | docs/03 | Instalación en Ubuntu/CentOS |
| Agentes Windows | docs/03 | Instalación en Windows |
| FIM | docs/03 | Monitorización de integridad |
| Rootkit | docs/03 | Detección de rootkits |
| Alertas | docs/04 | 5 tipos de alertas |
| Email | docs/04 | Notificaciones por correo |
| Telegram | docs/04 | Integración con Telegram |
| Slack | docs/04 | Integración con Slack |
| Dashboards | docs/05 | 5 dashboards temáticos |
| Pruebas | docs/06 | 7 escenarios de ataque |
| Automatización | ansible/ | Playbooks Ansible |
| Scripts | scripts/ | Instalación automatizada |

---

## REFERENCIAS EXTERNAS

### Documentación Oficial
- [Wazuh Documentation](https://documentation.wazuh.com/)
- [Elasticsearch Guide](https://www.elastic.co/guide/en/elasticsearch/reference/7.15/index.html)
- [Kibana User Guide](https://www.elastic.co/guide/en/kibana/7.15/index.html)
- [Logstash Manual](https://www.elastic.co/guide/en/logstash/7.15/index.html)

### Recursos de Seguridad
- [CIS Benchmarks](https://www.cisecurity.org/cis-benchmarks/)
- [NIST Cybersecurity Framework](https://www.nist.gov/cyberframework)
- [OWASP Top 10](https://owasp.org/www-project-top-ten/)

### Comunidades
- [Wazuh Community](https://wazuh.com/community/)
- [Elastic Community](https://www.elastic.co/community/)
- [Stack Overflow - Wazuh](https://stackoverflow.com/questions/tagged/wazuh)

---

## CHECKLIST DE REFERENCIA

- [ ] He leído README.md
- [ ] He entendido ARCHITECTURE.md
- [ ] He revisado los requisitos del sistema
- [ ] He instalado Wazuh Manager
- [ ] He instalado ELK Stack
- [ ] He configurado agentes
- [ ] He creado alertas
- [ ] He creado dashboards
- [ ] He ejecutado pruebas
- [ ] He documentado resultados

---

## SOPORTE Y CONTACTO

### Autores del Proyecto
- **Antonio Marinero Jabalera** - Wazuh
- **Santiago Quirós Arenas** - ELK Stack
- **Mario Palacios Moreno** - Integración y Documentación

### Canales de Soporte
- Documentación: `/docs`
- Issues: GitHub Issues
- Discussions: GitHub Discussions
- Email: Ver README.md

---

## VERSIÓN E HISTORIAL

**Versión Actual**: 1.0
**Fecha**: Enero 2025
**Estado**: Completo

### Cambios Principales (v1.0)
- Documentación completa del anteproyecto
- 6 guías técnicas de instalación
- Sistema de alertas configurado
- 5 dashboards diseñados
- 7 pruebas de seguridad
- Guías de troubleshooting

---

**Navegación**: Usa Ctrl+F para buscar términos específicos en este índice.

**Próximo paso**: Comienza por `README.md` si es tu primera vez.

---

**Última actualización**: Mayo 2026
*Proyecto: Implantación de un SOC Educativo con Wazuh + ELK*