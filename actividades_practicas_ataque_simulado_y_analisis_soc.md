# Actividades prácticas: Ataque simulado y análisis en un SOC educativo

> **Aviso académico y ético**
>
> Todas las actividades descritas a continuación están pensadas **exclusivamente para un laboratorio educativo aislado**. Los ataques son **simulados** y se centran en **generar evidencias**, **analizar registros** y **aplicar medidas defensivas**. No se incluyen comandos, herramientas ofensivas ni instrucciones explotables fuera de este entorno.

---

## Actividad 1 – Reconocimiento y escaneo de servicios (simulado)

### Objetivo
Aprender a identificar patrones de reconocimiento inicial mediante el análisis de logs y alertas en Wazuh y Elastic.

### Sistemas implicados
- Cliente Linux vulnerable
- Cliente Windows vulnerable
- Servidor Wazuh

### Descripción del escenario
Uno de los sistemas recibe **múltiples intentos de conexión** a diferentes puertos en un corto periodo de tiempo, lo que simula una fase de reconocimiento.

### Pasos de la actividad (simulación controlada)
1. Activar el registro detallado de conexiones en los sistemas cliente.
2. Generar tráfico de prueba que acceda a **varios servicios** del sistema en poco tiempo (por ejemplo, intentos de conexión repetidos a servicios conocidos).
3. Verificar que el **agente Wazuh** envía los eventos al servidor.
4. Acceder a Kibana para comenzar el análisis.

### Dónde buscar la información
- Logs de red y firewall del sistema operativo
- Alertas de Wazuh relacionadas con conexiones múltiples
- Dashboard general del SOC

### Qué analizar
- IP origen de las conexiones
- Número de puertos distintos accedidos
- Intervalo de tiempo entre eventos

### Qué se puede observar
- Patrón repetitivo de conexiones
- Incremento de alertas de bajo/medio nivel
- Diferencias de comportamiento entre Linux y Windows

### Medidas de mitigación
- Configurar firewall para limitar conexiones
- Implementar sistemas de detección de escaneos
- Aplicar segmentación de red

---

## Actividad 2 – Ataque de fuerza bruta sobre autenticación (simulado)

### Objetivo
Detectar intentos repetidos de autenticación fallida y comprender su impacto.

### Sistemas implicados
- Linux (servicio SSH)
- Windows (servicio RDP)

### Descripción del escenario
Se producen **múltiples intentos fallidos de inicio de sesión** contra un mismo usuario.

### Pasos de la actividad
1. Crear un usuario de prueba con credenciales simples.
2. Provocar varios intentos de autenticación incorrecta desde un mismo origen.
3. Confirmar la generación de eventos de autenticación fallida.
4. Acceder a Kibana para su análisis.

### Dónde buscar la información
- Linux: registros de autenticación
- Windows: eventos de seguridad (inicio de sesión fallido)
- Alertas automáticas de Wazuh

### Qué analizar
- Número de intentos por usuario
- IP origen
- Tiempo entre intentos

### Qué se puede observar
- Alertas de fuerza bruta
- Posible bloqueo de cuentas
- Diferencias de registro entre sistemas

### Medidas de mitigación
- Políticas de contraseñas robustas
- Bloqueo automático de cuentas
- Autenticación multifactor

---

## Actividad 3 – Acceso inicial con credenciales válidas

### Objetivo
Detectar accesos legítimos pero sospechosos.

### Sistemas implicados
- Linux
- Windows

### Descripción del escenario
Un usuario accede correctamente al sistema, pero desde un **origen o franja horaria no habitual**.

### Pasos de la actividad
1. Registrar accesos normales de un usuario durante varios días.
2. Simular un acceso correcto desde una IP distinta o en horario inusual.
3. Verificar que los eventos se registran en Wazuh.

### Dónde buscar la información
- Logs de autenticación
- Dashboards de accesos

### Qué analizar
- Historial previo del usuario
- Cambios en IP y horario

### Qué se puede observar
- Anomalías de comportamiento
- Correlación entre eventos previos y acceso exitoso

### Medidas de mitigación
- Alertas por accesos anómalos
- Restricción por IP/horario

---

## Actividad 4 – Escalada de privilegios (simulada)

### Objetivo
Identificar cambios de privilegios no habituales.

### Sistemas implicados
- Linux

### Descripción del escenario
Un usuario estándar obtiene privilegios elevados debido a una mala configuración.

### Pasos de la actividad
1. Configurar permisos excesivos en el sistema.
2. Simular una acción que genere un cambio de privilegio.
3. Analizar los eventos generados.

### Dónde buscar la información
- Logs de sistema
- Alertas de Wazuh sobre cambios de UID

### Qué analizar
- Usuario afectado
- Proceso asociado

### Medidas de mitigación
- Principio de mínimo privilegio
- Auditoría de configuraciones

---

## Actividad 5 – Persistencia en el sistema

### Objetivo
Detectar mecanismos que permiten mantener acceso.

### Sistemas implicados
- Linux y Windows

### Descripción del escenario
Se crean tareas programadas no autorizadas.

### Pasos de la actividad
1. Registrar el estado inicial del sistema.
2. Añadir una tarea programada de prueba.
3. Analizar la detección en Wazuh.

### Dónde buscar la información
- Linux: cron
- Windows: tareas programadas

### Qué analizar
- Fecha de creación
- Usuario creador

### Medidas de mitigación
- Monitorización de tareas
- Revisión periódica del sistema

---

## Actividad 6 – Ejecución de scripts sospechosos

### Objetivo
Detectar comportamientos similares a malware sin usar software malicioso.

### Sistemas implicados
- Linux
- Windows

### Descripción del escenario
Se ejecutan scripts de prueba con comportamiento anómalo.

### Pasos de la actividad
1. Crear un script simple de prueba.
2. Ejecutarlo fuera del comportamiento normal del sistema.
3. Analizar los eventos generados.

### Qué se puede observar
- Procesos inusuales
- Alertas de ejecución

### Medidas de mitigación
- Control de ejecución
- Monitorización de procesos

---

## Conclusión

Estas actividades permiten al alumnado **replicar escenarios reales de seguridad**, analizar evidencias en un SOC y aplicar **medidas defensivas**, desarrollando competencias prácticas en **detección, análisis y respuesta ante incidentes**.

