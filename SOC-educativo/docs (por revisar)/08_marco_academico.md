# Marco Académico - Objetivos de Aprendizaje y Competencias

## Contexto del Proyecto TFG

Este proyecto es un **Trabajo de Fin de Grado (TFG)** dirigido a estudiantes de ciclos formativos de Grado Superior:
- **ASIR** (Administración de Sistemas Informáticos en Red)
- **DAW** (Desarrollo de Aplicaciones Web)
- **DAM** (Desarrollo de Aplicaciones Multiplataforma)

Con especialización en **Seguridad Informática y Gestión de Incidentes**.

---

## Objetivos Generales

### Objetivo Principal
Diseñar e implementar un **Centro de Operaciones de Seguridad (SOC) educativo** funcional, alineado con estándares profesionales internacionales, que permita a estudiantes experimentar con sistemas de detección de incidentes reales en un entorno controlado.

### Objetivos Secundarios

1. **Infraestructura**: Montar una topología de red segura con máquinas virtuales especializadas (servidor central, clientes vulnerables, máquina de ataque)

2. **Monitorización Centralizada**: Implementar Wazuh como gestor centralizado de seguridad con capacidad de recepción, análisis y correlación de eventos

3. **Visualización de Datos**: Configurar ELK Stack (Elasticsearch, Logstash, Kibana) para almacenar, procesar y visualizar logs de seguridad

4. **Detección de Amenazas**: Crear reglas personalizadas alineadas con MITRE ATT&CK para detectar patrones de ataque reales

5. **Respuesta ante Incidentes**: Establecer procedimientos de detección, análisis y respuesta ante incidentes simulados

6. **Educación Defensiva**: Preparar a estudiantes en competencias prácticas de ciberseguridad defensiva

---

## Competencias Técnicas a Desarrollar

### Competencia 1: Administración de Infraestructura de Seguridad
**Descripción**: Capaz de instalar, configurar y mantener componentes de SIEM

**Habilidades**:
- [ ] Instalar y configurar Wazuh Manager en Ubuntu
- [ ] Instalar y configurar Elasticsearch/OpenSearch
- [ ] Desplegar Kibana y crear conexiones a índices
- [ ] Configurar Logstash para procesamiento de logs
- [ ] Gestionar agentes Wazuh en múltiples plataformas

**Criterios de evaluación**:
- Instalación completada sin errores
- Servicios activos y respondiendo a consultas
- Comunicación correcta entre componentes

---

### Competencia 2: Monitorización Multiplataforma
**Descripción**: Capaz de desplegar y configurar agentes en sistemas Linux y Windows

**Habilidades**:
- [ ] Instalar agentes Wazuh en Ubuntu/Debian
- [ ] Instalar agentes Wazuh en Windows 10/Server
- [ ] Configurar File Integrity Monitoring (FIM)
- [ ] Habilitar rootkit detection
- [ ] Configurar monitorización de aplicaciones

**Criterios de evaluación**:
- Agentes conectados y reportando eventos
- Logs siendo procesados correctamente
- Eventos apareciendo en Kibana

---

### Competencia 3: Análisis de Seguridad y MITRE ATT&CK
**Descripción**: Capaz de identificar técnicas de ataque y mapearlas a framework MITRE

**Habilidades**:
- [ ] Entender framework MITRE ATT&CK (tácticas vs. técnicas)
- [ ] Mapear escenarios de ataque a técnicas MITRE
- [ ] Identificar indicadores de compromiso (IoC)
- [ ] Correlacionar eventos para detectar patrones
- [ ] Crear rules de detección personalizadas

**Criterios de evaluación**:
- Mapeos correctos a técnicas MITRE
- IoCs identificados de forma precisa
- Rules de detección funcionales

---

### Competencia 4: Detección de Incidentes
**Descripción**: Capaz de identificar eventos de seguridad sospechosos

**Habilidades**:
- [ ] Detectar escaneo de puertos/red (T1046)
- [ ] Detectar intentos de fuerza bruta (T1110)
- [ ] Detectar accesos con credenciales válidas (T1078)
- [ ] Detectar escalada de privilegios (T1068)
- [ ] Detectar persistencia (T1053)

**Criterios de evaluación**:
- Identificar eventos correctos en Kibana
- Tiempo de detección razonable (<1 min)
- Tasa baja de falsos positivos

---

### Competencia 5: Respuesta ante Incidentes
**Descripción**: Capaz de investigar, analizar y responder ante incidentes

**Habilidades**:
- [ ] Investigar incidentes en dashboards de Kibana
- [ ] Correlacionar eventos relacionados
- [ ] Realizar análisis forense básico
- [ ] Documentar hallazgos y recomendaciones
- [ ] Implementar remediación

**Criterios de evaluación**:
- Investigación documentada paso a paso
- Conclusiones soportadas por evidencia
- Recomendaciones de mitigación apropiadas

---

### Competencia 6: Visualización y Reporting
**Descripción**: Capaz de crear dashboards y reportes ejecutivos

**Habilidades**:
- [ ] Crear dashboards temáticos en Kibana
- [ ] Diseñar visualizaciones claras y efectivas
- [ ] Crear reportes para stakeholders
- [ ] Presentar hallazgos de forma comprensible
- [ ] Documentar KPIs de seguridad

**Criterios de evaluación**:
- Dashboards informativos y navegables
- Visualizaciones apropiadas para datos
- Reportes ejecutivos claros

---

## Competencias Transversales

### Competencia 7: Pensamiento Crítico y Análisis
- Capacidad para analizar logs complejos
- Identificar patrones anómalos
- Tomar decisiones basadas en evidencia

### Competencia 8: Comunicación Técnica
- Documentar procedimientos y hallazgos
- Explicar conceptos de seguridad a diferentes audiencias
- Presentar resultados de forma profesional

### Competencia 9: Trabajo en Equipo
- Colaboración en análisis de incidentes
- Compartir conocimiento de técnicas de ataque
- Contribuir a mejora de procesos

### Competencia 10: Aprendizaje Continuo
- Familiaridad con frameworks internacionales (MITRE ATT&CK)
- Seguimiento de nuevas técnicas de ataque
- Actualización de reglas de detección

---

## Matriz de Evaluación por Competencia

| Competencia | Evidencia | Criterio | Ponderación |
|-------------|-----------|----------|-------------|
| 1. Infraestructura | Instalación exitosa | Todos servicios activos | 15% |
| 2. Monitorización | Agentes reportando | Events en Kibana | 15% |
| 3. MITRE ATT&CK | Mapeos documentados | Precisión 90%+ | 15% |
| 4. Detección | Rules funcionando | Detecta escenarios | 20% |
| 5. Respuesta | Análisis de incidente | Investigación completa | 20% |
| 6. Visualización | Dashboards creados | Útiles y profesionales | 15% |
| **Total** | | | **100%** |

---

## Contenidos de Aprendizaje

### Módulo 1: Fundamentos de SIEM (2 horas)
- Concepto de SIEM vs. Log Management
- Componentes de un SIEM típico
- Casos de uso de SIEM
- Ventajas y limitaciones

### Módulo 2: Arquitectura de Wazuh (2 horas)
- Componentes principales: Manager, Agent, API
- Flujo de datos: Agente → Manager → Elasticsearch
- Comunicación segura y autenticación
- Escalabilidad

### Módulo 3: ELK Stack en Profundidad (3 horas)
- Elasticsearch: Indices, shards, replicación
- Logstash: Pipelines, filtros, outputs
- Kibana: Visualizaciones, dashboards, alertas
- Monitorización del stack

### Módulo 4: Framework MITRE ATT&CK (3 horas)
- Estructura: Tácticas vs. Técnicas
- Matriz de ATT&CK: Fases del ataque
- Navegador MITRE ATT&CK
- Mapeo de eventos a técnicas

### Módulo 5: Detección de Amenazas (4 horas)
- Indicadores de Compromiso (IoC)
- Reglas de correlación
- Detección basada en comportamiento
- Tuning de alertas

### Módulo 6: Respuesta ante Incidentes (4 horas)
- Ciclo de respuesta: Detección, Análisis, Contención, Erradicación
- Análisis forense básico
- Cadena de custodia de evidencias
- Documentación de incidentes

### Módulo 7: Casos Prácticos Simulados (6 horas)
- Laboratorio 1: Reconocimiento y escaneo
- Laboratorio 2: Fuerza bruta
- Laboratorio 3: Acceso inicial
- Laboratorio 4: Escalada y persistencia
- Laboratorio 5: Movimiento lateral
- Laboratorio 6: Incidente completo

**Total de horas lectivas**: 24 horas (aproximadamente)

---

## Entregables Requeridos

### Entregable 1: Documentación Técnica
- [ ] Manual de instalación paso a paso
- [ ] Guía de configuración de agentes
- [ ] Documentación de reglas personalizadas
- [ ] Guía de troubleshooting

### Entregable 2: Dashboards Funcionales
- [ ] Dashboard "Visión General del SOC"
- [ ] Dashboard "Autenticación y Accesos"
- [ ] Dashboard "Actividad Sospechosa en Endpoints"
- [ ] Dashboard "Persistencia y Cambios en Archivos"
- [ ] Dashboard "MITRE ATT&CK Coverage"

### Entregable 3: Análisis de Escenarios
- [ ] Informe por cada escenario de ataque
- [ ] Logs y eventos capturados
- [ ] Análisis de detección y falsos positivos
- [ ] Recomendaciones de mejora

### Entregable 4: Presentación y Demo
- [ ] Presentación PowerPoint del proyecto
- [ ] Demo en vivo de detección
- [ ] Q&A sobre decisiones técnicas
- [ ] Conclusiones y trabajo futuro

---

## Rúbrica de Evaluación

### Escala de Puntuación
- **5 (Excelente)**: Cumple todos los criterios, implementación profesional
- **4 (Bueno)**: Cumple los criterios principales, minor issues
- **3 (Satisfactorio)**: Funciona correctamente, documentación suficiente
- **2 (Insuficiente)**: Funciona con limitaciones, documentación incompleta
- **1 (Deficiente)**: No funciona o no entregado

### Criterios por Aspecto

#### Funcionalidad Técnica (Peso: 40%)
| Criterio | 5 | 4 | 3 | 2 | 1 |
|----------|---|---|---|---|---|
| Instalación/Configuración | Todos componentes funcionales | 1-2 issues menores | Problemas configuración | Componentes no funcionan | No instalado |
| Agentes/Monitorización | Todos agentes en línea, datos fluyen | 90% agentes activos | 70%+ agentes activos | <70% agentes | No hay datos |
| Detección de Eventos | Detecta todos escenarios | Detecta 80%+ escenarios | Detecta 60%+ escenarios | Detecta <60% | Sin detección |
| Escalabilidad | Diseño preparado para escala | Preparado para 10 VMs | Preparado para 5 VMs | Diseño limitado | No escalable |

#### Análisis y Documentación (Peso: 30%)
| Criterio | 5 | 4 | 3 | 2 | 1 |
|----------|---|---|---|---|---|
| MITRE ATT&CK | Mapeos precisos y completos | 95%+ mapeos correctos | 80%+ mapeos correctos | <80% correcto | Mapeos ausentes |
| IoCs/Indicadores | Identificación exhaustiva | Identifica principales IoCs | IoCs básicos identificados | IoCs parciales | Sin IoCs |
| Análisis Forense | Conclusiones bien fundamentadas | Análisis sólido con minor gaps | Análisis aceptable | Análisis superficial | Sin análisis |
| Documentación | Completa, clara, profesional | Buena calidad, minor omisiones | Suficiente, pero mejorable | Incompleta | Mínima/ausente |

#### Presentación y Comunicación (Peso: 20%)
| Criterio | 5 | 4 | 3 | 2 | 1 |
|----------|---|---|---|---|---|
| Claridad de Explicación | Muy claro, profesional | Claro y bien estructurado | Aceptable | Confuso en algunos puntos | No se entiende |
| Visualizaciones | Dashboards excelentes | Dashboards buenos | Dashboards funcionales | Dashboards básicos | Visualizaciones pobres |
| Presentación | Profesional, bien ensayada | Preparada, fluida | Adecuada | Mejorable | Desorganizada |
| Q&A | Responde todas preguntas | Responde mayoría bien | Responde parcialmente | Dificultad responder | Sin respuestas |

---

## Valor Añadido del Proyecto

Este SOC educativo permite a estudiantes:

**Aprender haciendo**: Experiencia práctica con herramientas reales
**Entender defensa**: Mentalidad defensiva vs. ofensiva
**Análisis forense**: Investigación de incidentes paso a paso
**Estándares internacionales**: Alineación con MITRE ATT&CK
**Habilidades demandadas**: Competencias solicitadas por industria
**Contexto profesional**: Replicar un SOC empresarial
**Ética y legalidad**: Principios claros y controlados

---

## Conclusión

Este Marco Académico proporciona estructura clara para:

- **Definición de competencias**: Qué debe saber/hacer el estudiante
- **Evaluación objetiva**: Rúbricas y criterios medibles
- **Aprendizaje progresivo**: De conceptos básicos a análisis complejos
- **Documentación**: Entregables y evidencias evaluables
- **Alineación profesional**: Preparación para mundo laboral real

**Resultado**: Graduados competentes en seguridad ofensiva/defensiva, capaces de trabajar en SOCs profesionales.
**Última actualización**: Mayo 2026
