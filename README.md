# OneDrive Business Monitor / Monitor de OneDrive Business

[English](#english) | [Español](#español)

---

<a name="english"></a>
# English

Headless monitoring system to detect and alert on OneDrive for Business synchronization issues on Windows.

## Features

- 📊 **Continuous monitoring** of OneDrive status
- 🚧 **Automatic detection** of issues (authentication, sync, process)
- 🔔 **Multi-channel notifications** (Email, Teams, Slack)
- ⚡ **Auto-remediation** of common problems
- 📈 **Real-time web dashboard** for monitoring
- 🕵️ **Multiple detection methods** (Process, Registry, Canary file, Log analysis)

## Installation

### Prerequisites

- Windows 10/11 with OneDrive for Business
- Python 3.11+
- Gmail App Password (for email notifications)

### Quick Installation

```bash
# Clone repository
git clone https://github.com/hbuddenberg/OneDrive_Business_Monitor.git
cd OneDrive_Business_Monitor

# Install dependencies (option 1: pip)
pip install -r requirements.txt

# Option 2: uv (recommended)
pip install uv
uv sync
```

## Configuration

### 1. Create Configuration File

```bash
cp config.yaml.template config.yaml
```

### 2. Configure Gmail App Password

1. Go to https://myaccount.google.com/apppasswords
2. Generate a new App Password
3. Copy the 16-character password

### 3. Edit config.yaml

```yaml
target:
  email: "your-email@company.com"
  folder: "C:\\Users\\YOUR_USER\\OneDrive - YOUR_COMPANY"
  title: "OneDrive - YOUR_COMPANY"

notifications:
  channels:
    email:
      sender_email: "your-email@gmail.com"
      sender_password: "xxxx xxxx xxxx xxxx"  # 16-character App Password
      to_email: "your-email@company.com"
```

**IMPORTANT**: Replace all `YOUR_...` values with your actual data.

## Usage

### Windows

```bash
# Run monitor (recommended)
run_monitor.bat

# Or using CLI with flags
onedrive-business --help
```

#### Command-Line Flags

```bash
# Show help
onedrive-business --help

# Run monitor only (headless)
onedrive-business monitor

# Run dashboard only
onedrive-business dashboard --port 2048 --host 0.0.0.0

# Dashboard with auto-reload (development)
onedrive-business dashboard --reload

# Clean monitoring data
onedrive-business clean
```

### Windows Service (Optional)

To run automatically at startup:

1. Open `taskschd.msc` (Task Scheduler)
2. Create Basic Task
3. Trigger: At log on
4. Action: Start a program
   - Program: `python`
   - Arguments: `src/main.py`
   - Start in: `path\to\project`

## Monitored States

| State | Description |
|-------|-------------|
| `OK` | OneDrive syncing correctly |
| `SYNCING` | Files syncing (normal) |
| `NOT_RUNNING` | OneDrive process not detected |
| `AUTH_REQUIRED` | Authentication required |
| `SYNC_TIMEOUT` | Prolonged sync (>10 min) |

## Notification Channels

### Email (Gmail)
- ✅ Enabled by default
- Requires App Password

### Microsoft Teams
```yaml
channels:
  teams:
    enabled: true
    webhook_url: "https://outlook.office.com/webhook/YOUR_WEBHOOK"
```

### Slack
```yaml
channels:
  slack:
    enabled: true
    webhook_url: "https://hooks.slack.com/services/YOUR_WEBHOOK"
```

## Web Dashboard

Access real-time dashboard at:

```
http://localhost:2048
```

Shows:
- Current OneDrive status
- Incident history
- Last verification
- Event counters

## Security

⚠️ **CRITICAL**: NEVER commit `config.yaml` - it contains sensitive credentials.

The `config.yaml` file is in `.gitignore` to prevent accidental commits.

## Troubleshooting

### Not receiving email notifications
- Verify App Password is correct
- Confirm 2FA is enabled on Gmail account
- Check SPAM folder

### AUTH_REQUIRED false positives
- Set `tray_auth_check: false` in `validations`
- Verify `title` in `target` matches tray icon exactly

### Dashboard not accessible
- Verify port 2048 is not in use
- Check Windows firewall

## Project Structure

```
OneDrive_Business_Monitor/
├── src/
│   ├── main.py              # Main entry point
│   ├── monitor/             # Monitoring logic
│   │   ├── checker.py       # Detects OneDrive status
│   │   ├── remediator.py    # Auto-remediation
│   │   ├── alerter.py       # Alert logic
│   │   └── main.py          # Monitor orchestrator
│   ├── shared/
│   │   ├── config.py        # Configuration loading
│   │   ├── notifier.py      # Notification sending
│   │   ├── templates.py     # Email/HTML templates
│   │   ├── database.py      # Incident history
│   │   └── schemas.py       # Data models
│   └── dashboard/
│       └── main.py          # Web dashboard (FastAPI)
├── config.yaml.template     # Configuration template
├── requirements.txt         # Dependencies
├── pyproject.toml          # Project configuration
├── run_monitor.bat         # Windows script
└── README.md               # This file
```

## Contributing

This project is maintained for personal use. Suggestions and improvements are welcome via Pull Requests.

## License

MIT License - See LICENSE file for details.

## Disclaimer

This tool is not affiliated with Microsoft or OneDrive. It is an independent monitoring project.

---

<a name="español"></a>
# Español

Monitor headless para detectar y alertar sobre problemas de sincronización de OneDrive for Business en Windows.

## Características

- 📊 **Monitoreo continuo** del estado de OneDrive
- 🚧 **Detección automática** de problemas (autenticación, sincronización, proceso)
- 🔔 **Notificaciones multi-canal** (Email, Teams, Slack)
- ⚡ **Auto-remediación** de problemas comunes
- 📈 **Dashboard web** para monitoreo en tiempo real
- 🕵️ **Múltiples métodos de detección** (Process, Registry, Canary file, Log analysis)

## Instalación

### Requisitos Previos

- Windows 10/11 con OneDrive for Business
- Python 3.11+
- Gmail App Password (para notificaciones por email)

### Instalación Rápida

```bash
# Clonar repositorio
git clone https://github.com/hbuddenberg/OneDrive_Business_Monitor.git
cd OneDrive_Business_Monitor

# Instalar dependencias (opción 1: pip)
pip install -r requirements.txt

# Opción 2: uv (recomendado)
pip install uv
uv sync
```

## Configuración

### 1. Crear Archivo de Configuración

```bash
cp config.yaml.template config.yaml
```

### 2. Configurar Gmail App Password

1. Ir a https://myaccount.google.com/apppasswords
2. Generar un nuevo App Password
3. Copiar el password de 16 caracteres

### 3. Editar config.yaml

```yaml
target:
  email: "tu-email@empresa.com"
  folder: "C:\\Users\\TU_USUARIO\\OneDrive - TU_EMPRESA"
  title: "OneDrive - TU_EMPRESA"

notifications:
  channels:
    email:
      sender_email: "tucorreo@gmail.com"
      sender_password: "xxxx xxxx xxxx xxxx"  # App Password de 16 caracteres
      to_email: "tu-email@empresa.com"
```

**IMPORTANTE**: Reemplaza todos los valores `TU_...` con tus datos reales.

## Uso

### Windows

```bash
# Ejecutar monitor (recomendado)
run_monitor.bat

# O usando el CLI con flags
onedrive-business --help
```

#### Flags de Línea de Comandos

```bash
# Mostrar ayuda
onedrive-business --help

# Ejecutar solo el monitor (sin dashboard)
onedrive-business monitor

# Ejecutar solo el dashboard web
onedrive-business dashboard --port 2048 --host 0.0.0.0

# Dashboard con auto-reload (desarrollo)
onedrive-business dashboard --reload

# Limpiar datos de monitoreo
onedrive-business clean
```

### Como Servicio de Windows (Opcional)

Para ejecutar automáticamente al inicio:

1. Abrir `taskschd.msc` (Programador de Tareas)
2. Crear tarea básica
3. Trigger: Al iniciar sesión
4. Acción: Iniciar programa
   - Programa: `python`
   - Argumentos: `src/main.py`
   - Iniciar en: `ruta\al\proyecto`

## Métricas Monitoreadas

| Estado | Descripción |
|--------|-------------|
| `OK` | OneDrive sincronizando correctamente |
| `SYNCING` | Archivos sincronizando (normal) |
| `NOT_RUNNING` | Proceso de OneDrive no detectado |
| `AUTH_REQUIRED` | Requiere autenticación |
| `SYNC_TIMEOUT` | Sincronización prolongada (>10 min) |

## Canales de Notificación

### Email (Gmail)
- ✅ Habilitado por defecto
- Requiere App Password

### Microsoft Teams
```yaml
channels:
  teams:
    enabled: true
    webhook_url: "https://outlook.office.com/webhook/TU_WEBHOOK"
```

### Slack
```yaml
channels:
  slack:
    enabled: true
    webhook_url: "https://hooks.slack.com/services/TU_WEBHOOK"
```

## Dashboard Web

Accede al dashboard en tiempo real:

```
http://localhost:2048
```

Muestra:
- Estado actual de OneDrive
- Historial de incidentes
- Última verificación
- Contadores de eventos

## Seguridad

⚠️ **CRÍTICO**: NUNCA hacer commit de `config.yaml` - contiene credenciales sensibles.

El archivo `config.yaml` está en `.gitignore` para prevenir commits accidentales.

## Solución de Problemas

### No recibo notificaciones de email
- Verificar que el App Password sea correcto
- Confirmar que 2FA está habilitado en la cuenta de Gmail
- Revisar carpeta de SPAM

### Falsos positivos de AUTH_REQUIRED
- Ajustar `tray_auth_check: false` en `validations`
- Verificar que el `title` en `target` coincida exactamente con el ícono de bandeja

### Dashboard no accesible
- Verificar que el puerto 2048 no esté en uso
- Revisar firewall de Windows

## Estructura del Proyecto

```
OneDrive_Business_Monitor/
├── src/
│   ├── main.py              # Entry point principal
│   ├── monitor/             # Lógica de monitoreo
│   │   ├── checker.py       # Detecta estado de OneDrive
│   │   ├── remediator.py    # Auto-remediación
│   │   ├── alerter.py       # Lógica de alertas
│   │   └── main.py          # Orquestador de monitoreo
│   ├── shared/
│   │   ├── config.py        # Carga de configuración
│   │   ├── notifier.py      # Envío de notificaciones
│   │   ├── templates.py     # Templates de email/HTML
│   │   ├── database.py      # Historial de incidentes
│   │   └── schemas.py       # Modelos de datos
│   └── dashboard/
│       └── main.py          # Dashboard web (FastAPI)
├── config.yaml.template     # Template de configuración
├── requirements.txt         # Dependencias
├── pyproject.toml          # Configuración de proyecto
├── run_monitor.bat         # Script Windows
└── README.md               # Este archivo
```

## Contribuciones

Este proyecto es mantenido para uso personal. Las sugerencias y mejoras son bienvenidas mediante Pull Requests.

## Licencia

MIT License - Ver archivo LICENSE para detalles.

## Disclaimer

Esta herramienta no está afiliada con Microsoft ni OneDrive. Es un proyecto independiente de monitoreo.
