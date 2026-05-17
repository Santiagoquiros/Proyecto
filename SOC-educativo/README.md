# Implantación de un SOC Educativo con Wazuh + ELK

## Proyecto de Fin de Grado (TFG)

**Ciclos Formativos de Grado Superior**: ASIR / DAW / DAM (Especialización en Seguridad Informática)

## Descripción del Proyecto

Este TFG corresponde al diseño e implementación de un **SOC (Security Operations Center) educativo** que replica el funcionamiento de un SOC profesional real. Es un sistema integral que centraliza la monitorización, detección de incidentes y generación de alertas automáticas en entornos de TI.

**Enfoque**: Entorno educativo para simular ataques controlados, recoger evidencias, analizarlas mediante Wazuh + Elastic Stack y formar al alumnado en detección, análisis y respuesta ante incidentes, con alineación a framework **MITRE ATT&CK**.

## Equipo del Proyecto

- **Antonio Marinero Jabalera**
- **Santiago Quirós Arenas**
- **Mario Palacios Moreno**

## Objetivo General

Montar un Centro de Operaciones de Seguridad (SOC) educativo que permita monitorizar de forma centralizada los eventos de seguridad de un laboratorio de servidores y equipos, detectar incidentes y generar alertas automáticas para la defensa de la red.

## Objetivos Específicos

1. Configurar un servidor central de logs y monitorización con ELK (Elasticsearch, Logstash, Kibana)
2. Integrar agentes Wazuh en sistemas Windows y Linux para recolectar eventos de seguridad
3. Configurar alertas de seguridad automáticas (intentos de acceso, cambios críticos, malware simulado)
4. Crear dashboards de visualización claros y didácticos para análisis de seguridad
5. Simular ataques controlados y documentar la detección y respuesta
6. Redactar documentación completa del proyecto: arquitectura, instalación, pruebas y conclusiones

## Arquitectura del Proyecto

### Componentes Principales

- **Wazuh Manager**: Gestiona agentes, reglas y alertas; consolida la información de seguridad
- **Agentes Wazuh**: Instalados en servidores; recolectan logs de sistema, autenticación, procesos y cambios críticos
- **ELK Stack**:
  - **Elasticsearch**: Almacena los logs de forma indexada
  - **Logstash**: Procesa los logs de Wazuh y los envía a Elasticsearch
  - **Kibana**: Visualiza la información mediante dashboards y paneles de control
- **Infraestructura**: Laboratorio virtual con 2-3 servidores Linux, 1 servidor Windows y firewall virtual

Ver [ARCHITECTURE.md](ARCHITECTURE.md) para diagrama detallado.

## Fases del Proyecto

| Fase | Duración | Descripción |
|------|----------|------------|
| 1. Planificación y diseño | 1 semana | Definir alcance, recursos, requisitos y arquitectura |
| 2. Montaje de infraestructura | 1-2 semanas | Instalar máquinas virtuales, configurar red, instalar Wazuh y ELK |
| 3. Configuración de agentes y logs | 1-2 semanas | Instalar agentes Wazuh, configurar envío de logs y pruebas |
| 4. Creación de reglas y alertas | 1 semana | Configurar alertas automáticas por eventos críticos |
| 5. Visualización y dashboards | 1 semana | Crear dashboards temáticos y personalizar gráficos |
| 6. Pruebas y simulación de incidentes | 1-2 semanas | Simular ataques controlados y documentar detección |
| 7. Documentación y presentación | 1 semana | Redactar manual completo e informe de pruebas |

## Entregables

1. Diagramas de arquitectura y flujo de datos
2. Capturas y configuraciones de Wazuh y ELK
3. Dashboards funcionales en Kibana
4. Documentación completa: instalación, configuración, pruebas y conclusiones
5. Demo funcional: detección y alertas ante ataques simulados

## Alcance del Proyecto

- **Infraestructura**: Laboratorio virtual o físico con múltiples sistemas operativos
- **Seguridad**: Solo pruebas en entorno controlado; ataques simulados con fines educativos
- **Automatización**: Alertas básicas ante eventos críticos (correo, Telegram, dashboard)
- **Opcional**: Integración con Suricata o Snort para monitorización de red

## Estructura del Proyecto

```
SOC-educativo/
├── docs/                 # Documentación del proyecto
├── agents/               # Configuración de agentes Wazuh
├── ansible/              # Playbooks de automatización
├── dashboards/           # Exportaciones de dashboards Kibana
├── deliverables/         # Entregables finales
├── scripts/              # Scripts de instalación y configuración
├── tests/                # Pruebas y simulaciones de ataques
├── ARCHITECTURE.md       # Diagrama y descripción de arquitectura
├── CONTRIBUTING.md       # Guía de contribuciones
└── README.md            # Este archivo
```

## Acceso al Servidor SOC

### Dashboard Web de Wazuh

```
URL: https://wazuh.soc.informatica.iesgrancapitan.org/
Usuario: admin
Contraseña: bgCt6c9pHkPg+Z2OHL*Z.MCiEWo9rTWy
```

## Información del Servidor Central

| Componente | Dirección | Puerto |
|-----------|-----------|--------|
| **Servidor SOC** | 10.68.0.158 | - |
| **Wazuh Manager** | 10.68.0.158 | 1514 (TCP/UDP) |
| **Elasticsearch** | 10.68.0.158 | 9200 |
| **Kibana** | 10.68.0.158 | 5601 |
| **Logstash** | 10.68.0.158 | 5000 |
| **API Wazuh** | 10.68.0.158 | 55000 |

### Configuración de Red - Servidor SOC

```yaml
Dirección IP: 10.68.0.158
Máscara: 255.0.0.0
Puerta de enlace: 10.0.0.8
DNS primario: 8.8.8.8
DNS secundario: 1.1.1.1
```

Ver: [Configuración de Red Servidor](agents/server/config_red.md)

## Acceso Remoto desde el Exterior

Para conectarse al servidor desde fuera del centro educativo:

```bash
ssh -p 9313 administrador@cpd.informatica.iesgrancapitan.org

Usuario: administrador
Contraseña: Root1234$
```

## Máquinas Clientes Monitorizadas

### 1. Ubuntu-Cliente (Linux)

| Parámetro | Valor |
|-----------|-------|
| **IP** | 10.68.0.161 |
| **Usuario SSH** | usuario |
| **Contraseña SSH** | Root1234$ |
| **SO** | Ubuntu Server 24.04 LTS |
| **Agente Wazuh** | Instalado |

**Conexión SSH desde servidor SOC:**
```bash
ssh usuario@10.68.0.161
```

**Configuración de red:** [Ver detalles](agents/linux/config_red.md)

---

### 2. Windows-Cliente (Windows Server)

| Parámetro | Valor |
|-----------|-------|
| **IP** | 10.68.0.163 |
| **Usuario SSH** | usuario |
| **Contraseña SSH** | Root1234$ |
| **SO** | Windows 10 |
| **Agente Wazuh** | Instalado |

**Conexión SSH desde servidor SOC:**
```bash
ssh usuario@10.68.0.163
```

**Configuración de red:** [Ver detalles](agents/windows/config_red.md)

---

### 3. Kali-Atacante (Linux)

| Parámetro | Valor |
|-----------|-------|
| **IP** | 10.68.0.165 |
| **Usuario SSH** | kali |
| **Contraseña SSH** | Root1234$ |
| **SO** | Kali Linux |
| **Propósito** | Máquina de pruebas/ataques |
| **Agente Wazuh** | NO instalado |

**Conexión SSH desde servidor SOC:**
```bash
ssh kali@10.68.0.165
```

**Configuración de red y herramientas:** [Ver detalles](agents/kali_linux.md)

## Inicio Rápido

### Requisitos Previos

- Acceso a la red del centro educativo (10.68.0.0/8)
- Credenciales proporcionadas por los responsables del proyecto
- Navegador web para acceder a Wazuh Dashboard
- Cliente SSH para conexiones remotas

### Acceso a Dashboards

Para monitorizar eventos en tiempo real:

```
Wazuh Dashboard: https://wazuh.soc.informatica.iesgrancapitan.org/
Kibana: https://wazuh.soc.informatica.iesgrancapitan.org:5601
```

### Instalación Local (Para Pruebas en Laboratorio)

Consulta las guías de instalación específicas:

1. [Instalación de Wazuh Manager](docs/01_instalacion_wazuh.md)
2. [Instalación de ELK Stack](docs/02_instalacion_elk.md)
3. [Configuración de Agentes Wazuh](docs/03_configuracion_agentes.md)
4. [Configuración de Alertas](docs/04_configuracion_alertas.md)

## Proyecto de Fin de Grado (TFG)

**Ciclos Formativos de Grado Superior**: ASIR / DAW / DAM (Especialización en Seguridad Informática)

**Enfoque Académico**: Este TFG proporciona un entorno educativo realista para:
- Aprender gestión de seguridad informática con herramientas profesionales
- Comprender detección de incidentes y respuesta (Incident Response)
- Experiencia práctica con MITRE ATT&CK Framework
- Desarrollo de competencias demandadas por industria
- Análisis forense y correlación de eventos
- Mentalidad defensiva en ciberseguridad

## Competencias Desarrolladas

Administración de Infraestructura: Instalación/configuración de SIEM
Monitorización Multiplataforma: Linux, Windows, cloud
Análisis de Seguridad: MITRE ATT&CK y técnicas de ataque
Detección de Incidentes: Reglas y correlación de eventos
Respuesta ante Incidentes: Investigación y remediación
Visualización de Datos: Dashboards y reportes ejecutivos

## Justificación

La creciente complejidad de los entornos de TI y la frecuencia de ataques cibernéticos hacen imprescindible contar con sistemas de monitorización y detección de incidentes. Los Centros de Operaciones de Seguridad (SOC) permiten centralizar la vigilancia, generar alertas automáticas y fortalecer la defensa de redes y servidores. Este proyecto permite aprender estos conceptos en un entorno seguro y controlado.

## Marco Académico Integrado

### Documentación Académica del TFG

Este proyecto incluye documentación completa para el marco académico:

**[docs/08_marco_academico.md](docs/08_marco_academico.md)** - Objetivos y Competencias TFG
- Contexto del proyecto para ciclos ASIR/DAW/DAM
- 6 competencias técnicas principales con criterios de evaluación
- 4 competencias transversales (pensamiento crítico, comunicación, trabajo en equipo)
- 24 horas de contenidos de aprendizaje estructurados
- Rúbrica de evaluación detallada (escala 1-5)
- Matriz de ponderación por competencia
- Entregables requeridos

**[docs/07_escenarios_mitre_attack.md](docs/07_escenarios_mitre_attack.md)** - Escenarios de Ataque y MITRE ATT&CK
- 8 escenarios de ataque educativos alineados con MITRE ATT&CK
- Tabla de mapeo: Ataque → Técnica MITRE → Evento detectable
- 3 prácticas educativas con procedimientos paso a paso:
  - Práctica 1: Reconocimiento de red
  - Práctica 2: Detección de fuerza bruta
  - Práctica 3: Análisis de accesos sospechosos
- Indicadores de Compromiso (IoC)
- Escenarios avanzados futuros

**[docs/09_escalabilidad_futuro.md](docs/09_escalabilidad_futuro.md)** - Roadmap de Escalabilidad
- Fase 1: Mejoras inmediatas (Suricata, Telegram, Dashboards)
- Fase 2: Capacidades avanzadas (UBA, AD, Threat Intel)
- Fase 3: Automatización (SOAR, Playbooks)
- Fase 4: Cumplimiento (ISO 27001, PCI-DSS, HIPAA, GDPR)
- Arquitectura escalada (de 2 a 500+ agentes)
- Escenarios de ataque avanzados

### Caminos de Aprendizaje Personalizados

Consulta [INDEX.md](INDEX.md) para 6 caminos de aprendizaje:
1. **Estudiante Principiante** (3-4 semanas) - Competencia técnica
2. **Administrador Experimentado** (1 semana) - Implementación rápida
3. **Analista de Seguridad** (2 semanas) - Detección y respuesta
4. **Especialista en Seguridad** (2-3 semanas) - Auditoría y cumplimiento
5. **Desarrollador/Contribuidor** (3-4 semanas) - Mejora del proyecto
6. **Profesor/Educador** (1-2 semanas) - Material docente

## Licencia

Ver archivo [LICENSE](LICENSE)

## Contribuciones

Ver [CONTRIBUTING.md](CONTRIBUTING.md) para más información.

---