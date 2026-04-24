# 🔐 Laboratorio de Ciberseguridad con Wazuh

Este repositorio contiene una serie de actividades escalonadas para aprender ciberseguridad defensiva utilizando Wazuh (SIEM/XDR). Cada actividad depende de la anterior, formando un laboratorio práctico completo.


## 🎯 Objetivo

Construir un entorno de seguridad funcional capaz de:

- Detectar amenazas
- Analizar eventos de seguridad
- Responder automáticamente a ataques
- Aplicar medidas de hardening


## 🧩 Estructura de Actividades

### ✅ Actividad 1: Instalación de Wazuh

**Objetivo:** Desplegar Wazuh en un servidor.

**Tareas:**
- Instalar Wazuh (modo all-in-one recomendado)
- Verificar servicios activos
- Acceder al dashboard web

**Resultado esperado:**
- Dashboard accesible
- Sistema listo para recibir logs


### ⚙️ Actividad 2: Configuración y agentes

**Requisito:** Haber completado la Actividad 1

**Objetivo:** Monitorizar sistemas mediante agentes.

**Tareas:**
- Instalar agente en máquina cliente
- Registrar agente en Wazuh
- Configurar:
  - Monitoreo de logs
  - Integridad de archivos (FIM)
- Generar eventos (ej: login fallido)

**Resultado esperado:**
- Agente visible en el dashboard
- Alertas generadas correctamente


### 🖥️ Actividad 3: Creación de laboratorio virtual

**Requisito:** Actividad 2

**Objetivo:** Crear entorno controlado de pruebas.

**Tareas:**
- Instalar VirtualBox o VMware
- Crear máquinas:
  - Atacante (Kali Linux)
  - Víctima (Ubuntu/Windows con agente)
- Configurar red interna

**Resultado esperado:**
- Comunicación entre máquinas
- Wazuh monitorizando la víctima


### ⚔️ Actividad 4: Simulación de ataques

**Requisito:** Actividad 3

**Objetivo:** Generar eventos de seguridad reales.

**Tareas:**
- Escaneo de red (Nmap)
- Ataque de fuerza bruta (Hydra sobre SSH)
- Revisar detección en Wazuh

**Resultado esperado:**
- Alertas de seguridad generadas
- Identificación de IP atacante


### 🧠 Actividad 5: Análisis y reglas

**Requisito:** Actividad 4

**Objetivo:** Entender y mejorar la detección.

**Tareas:**
- Analizar alertas
- Identificar:
  - Severidad
  - Reglas activadas
- Crear regla personalizada

**Resultado esperado:**
- Nueva regla funcionando
- Mejor comprensión del sistema


### 🛡️ Actividad 6: Respuesta activa

**Requisito:** Actividad 5

**Objetivo:** Automatizar la defensa.

**Tareas:**
- Configurar Active Response
- Bloquear IP atacante automáticamente
- Probar con nuevos ataques

**Resultado esperado:**
- IP bloqueada tras ataque


### 📊 Actividad 7: Hardening

**Requisito:** Actividad 6

**Objetivo:** Prevenir vulnerabilidades.

**Tareas:**
- Configurar seguridad SSH:
  - Cambiar puerto
  - Deshabilitar root
- Aplicar estándares (CIS, PCI-DSS)
- Revisar alertas tras cambios

**Resultado esperado:**
- Sistema más seguro
- Reducción de alertas críticas


## 🧱 Arquitectura del laboratorio
