# 4. Configuración de Alertas

## Configuración de Alertas

Las alertas son eventos que generan reacciones en el SOC. Esta guía cubre la creación y configuración de alertas automáticas de seguridad.

## Tipos de Alertas

### 1. Alertas de Autenticación
Detectan intentos de acceso no autorizados.

**Descripción**: Reglas para detectar intentos fallidos de login, múltiples fallos de autenticación, cambios de contraseña y escalada de privilegios.

### 2. Alertas de Integridad de Archivos (FIM)

**Descripción**: Detecta modificaciones, cambios de permisos y nuevos archivos en directorios críticos del sistema.

### 3. Alertas de Red y Conexiones

**Descripción**: Monitoriza puertos abiertos, conexiones rechazadas por firewall y detecta escaneos de red.

### 4. Alertas de Cambios de Sistema

**Descripción**: Detecta instalación/eliminación de software, cambios en configuración del sistema y mensajes del kernel.

### 5. Alertas de Detección de Malware

**Descripción**: Detección de malware mediante ClamAV, procesos ocultos y rootkits.

## Configuración de Notificaciones por Email

En `/var/ossec/etc/ossec.conf`:

**Descripción**: Configuración de alertas por correo electrónico para eventos críticos de seguridad.

## Integración con Telegram

### 1. Crear Bot en Telegram
1. Hablar con @BotFather en Telegram
2. Crear nuevo bot
3. Obtener API token
4. Crear grupo/canal para alertas
5. Obtener Chat ID

### 2. Script de Integración
Archivo: `/var/ossec/active-response/bin/telegram-alert.sh`

**Descripción**: Script para enviar alertas automáticas a Telegram cuando se detectan eventos críticos.

## Integración con Slack (Opcional)

Archivo: `/var/ossec/active-response/bin/slack-alert.sh`

**Descripción**: Script para enviar alertas a Slack en tiempo real.

## Configuración de Alertas por Severidad

| Nivel | Rango | Descripción | Acción |
|-------|-------|-------------|--------|
| Low | 1-4 | Información general | Log |
| Medium | 5-7 | Eventos anómalos | Log + Dashboard |
| High | 8-11 | Eventos sospechosos | Log + Email |
| Critical | 12-15 | Alertas críticas | Log + Email + Telegram + Active Response |

## Prueba de Alertas

### Simular Intento de Acceso Fallido
```bash
# En servidor con agente
ssh -u wronguser@localhost
```

### Simular Cambio en Archivo
```bash
# Modificar archivo monitoreado
echo "test" >> /etc/hosts
```

### Simular Puerto Abierto
```bash
# Crear nuevo servicio
nc -l -p 8888
```

## Troubleshooting

**Descripción**: Procedimientos para verificar que los emails se envían correctamente, las alertas se generan y el sistema funciona sin problemas.

## Siguiente Paso

Continúa con [Creación de Dashboards](05_dashboards.md)

---

**Última actualización**: Mayo 2026
