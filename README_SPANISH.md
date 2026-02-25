# Agente Hanus

Agente Hanus es un asistente de inteligencia artificial multimodal, extensible y personalizable, diseñado para interacción por voz y texto, con integración de modelos de IA avanzados, herramientas personalizadas y administración web.

---

## Tabla de Contenidos

- [Características principales](#características-principales)
- [Capacidades avanzadas de autoprogramación](#capacidades-avanzadas-de-autoprogramación)
- [Ejemplos de tools incluidas](#ejemplos-de-tools-incluidas)
- [Arquitectura](#arquitectura)
- [Estructura de carpetas](#estructura-de-carpetas)
- [Instalación](#instalación)
- [Uso](#uso)
- [Endpoints principales](#endpoints-principales)
- [Personalización y extensibilidad](#personalización-y-extensibilidad)
- [Requisitos](#requisitos)
- [Créditos y licencias](#créditos-y-licencias)

---

## ¿Por qué elegir Hanus?

- **Enterprise-ready**: Arquitectura robusta, modular y escalable, lista para producción y despliegue en entornos empresariales.
- **Seguridad y control**: Ejecución de código en entornos restringidos, validación de parámetros y registro de logs para auditoría.
- **Automatización avanzada**: Capacidad de auto-mejorarse, crear nuevas funciones y adaptarse a necesidades cambiantes sin intervención manual.
- **Integración total**: API RESTful, panel web intuitivo, y soporte para integración con sistemas externos y pipelines de IA.
- **Soporte multimodal**: Interacción natural por voz y texto, adaptable a asistentes físicos, bots, o aplicaciones web.
- **Documentación y mantenibilidad**: Código documentado, estructura clara y ejemplos de uso para facilitar la adopción y el mantenimiento.

## Roadmap y visión

- **Integración con más modelos de IA**: Soporte para Llama, Qwen, Claude, y modelos locales.
- **Plugins y skills**: Sistema de plugins para ampliar funcionalidades sin tocar el core.
- **Panel de analítica**: Estadísticas de uso, logs de interacción y métricas de rendimiento.
- **Despliegue cloud y on-premise**: Scripts y contenedores listos para Kubernetes, Docker Compose y servidores dedicados.
- **Internacionalización**: Soporte multilenguaje y adaptación cultural.

## Reconocimientos y motivación

Hanus nace de la pasión por la IA aplicada, la automatización y la creación de asistentes realmente útiles, seguros y personalizables. Inspirado en los mejores copilots y asistentes del mercado, pero con control total y privacidad.

---

## Características principales

- **Interacción por voz y texto**: Reconocimiento y síntesis de voz en español (STT/TTS) usando Whisper y Edge TTS.
- **Soporte multiai**: Integración con modelos como Gemini, GPT, Grok, DeepSeek, y más.
- **Herramientas (Tools) y funciones personalizadas**: Creadas desde el panel web o por la IA, ejecutables bajo demanda o de forma periódica.
- **Panel de administración web**: Gestión de IAs, triggers, tools y usuarios.
- **Chat de voz continuo**: Activación por palabra clave ("Hanus"/"Janus"), escucha y respuesta automática.
- **Tareas periódicas y heartbeat**: Ejecución de tareas programadas y monitorización del sistema.
- **Persistencia y base de datos**: Usuarios, configuraciones, funciones y triggers almacenados en SQLite.

## Capacidades avanzadas de autoprogramación

- **Generación y auto-integración de código**: Hanus puede crear nuevas funciones Python (tools) desde el panel web o por instrucciones en lenguaje natural, integrarlas automáticamente y ejecutarlas bajo demanda o de forma periódica, similar a GitHub Copilot pero en tu propio entorno.
- **Extensibilidad dinámica**: El propio agente puede registrar nuevas tools en caliente, sin reiniciar el servidor, usando su propia herramienta `self_register_tool`.
- **IA que programa**: Hanus es capaz de escribir código Python, integrarlo y probarlo, permitiendo que evolucione y se adapte a nuevas necesidades de forma autónoma.

## Ejemplos de tools incluidas

- **moltbook**: Cliente para interactuar con la API de Moltbook (crear posts, consultar datos, etc.).
- **self_register_tool**: Permite a Hanus registrar nuevas funciones/tools en caliente, guardando el código y los metadatos en la base de datos y el sistema de archivos.
- **docker_run_server**: Crea y ejecuta servidores (Flask, FastAPI, Node.js, etc.) en contenedores Docker.
- **web_search**: Realiza búsquedas en internet y devuelve resúmenes.
- **calculator**: Evalúa expresiones matemáticas de forma segura.
- **check_ram_usage, check_cpu_usage, check_disk_usage**: Monitorización del sistema.
- **camera_snapshot, camera_move, camera_speak**: Control de cámaras RTSP.

Puedes ver y probar todas las tools disponibles desde el panel web o consultando el archivo `tools_registry.json`.

## Arquitectura

- **Backend**: Python + Flask
- **Frontend**: HTML5, Bootstrap, JS (plantillas Jinja2)
- **Reconocimiento de voz**: [faster-whisper](https://github.com/SYSTRAN/faster-whisper)
- **Síntesis de voz**: [Edge TTS](https://github.com/rany2/edge-tts)
- **Base de datos**: SQLite (SQLAlchemy)
- **Tareas y scheduler**: APScheduler
- **Integraciones IA**: Gemini, etc.

## Estructura de carpetas

```
classAI/
├── app.py                  # Cliente API externo (opcional)
├── hanus/
│   ├── run.py              # Entry point del servidor Flask
│   ├── config.py           # Configuración global
│   ├── hanus_voice_local.py# Cliente local de voz (micrófono)
│   ├── app/
│   │   ├── __init__.py     # Inicialización Flask y DB
│   │   ├── api.py          # Endpoints REST/JSON (voz, chat, tools)
│   │   ├── admin.py        # Panel web de administración
│   │   ├── hanus_core.py   # Lógica principal de Hanus
│   │   ├── voice.py        # STT/TTS
│   │   ├── models.py       # Modelos SQLAlchemy
│   │   ├── ia_integrations.py # Integración con proveedores IA
│   │   ├── tools/          # Herramientas y funciones
│   │   ├── templates/      # Plantillas web (admin, chat, voz, tools)
│   │   └── ...
│   ├── voces/              # Audios de entrenamiento/usuarios
│   └── instance/site.db    # Base de datos SQLite
└── ...
```

## Instalación

1. **Clona el repositorio**
2. **Instala dependencias**:
   ```zsh
   pip install -r requirements.txt
   # O instala manualmente:
   pip install flask flask_sqlalchemy flask_login flask_wtf apscheduler faster-whisper edge-tts pyaudio numpy scipy requests
   ```
3. **Configura variables** (opcional):
   - Edita `hanus/config.py` para claves y rutas personalizadas.
4. **Inicializa la base de datos**:
   ```zsh
   cd hanus
   flask shell
   >>> from app import db, create_app
   >>> app = create_app()
   >>> with app.app_context(): db.create_all()
   >>> exit()
   ```
5. **Ejecuta el servidor**:
   ```zsh
   python hanus/run.py
   ```
6. **(Opcional) Ejecuta el cliente de voz local**:
   ```zsh
   python hanus/hanus_voice_local.py
   ```

## Uso

- Accede al panel web: [http://localhost:5000/admin](http://localhost:5000/admin)
- Prueba el chat de voz continuo o el chat de texto.
- Crea y administra IAs, triggers y tools desde el panel.
- Usa el endpoint `/api/hanus` para integración programática.

## Endpoints principales

- `POST /api/hanus` — Chat IA (JSON: `{message, uid}`)
- `POST /api/tts` — Texto a voz (JSON: `{text, voice}`)
- `POST /api/stt` — Audio a texto (form-data: `audio`)
- `POST /api/hanus/clear_history` — Borra historial de usuario

## Personalización y extensibilidad

- **Tools**: Añade funciones Python en el panel o en `app/tools/`.
- **IAs**: Configura nuevos modelos y prompts en el panel.
- **Triggers**: Define reglas para enrutar mensajes a diferentes IAs.
- **Tareas periódicas**: Programa ejecuciones automáticas de tools.

## Requisitos
- Python 3.10+
- Linux recomendado (soporte parcial en Windows/Mac)
- Dependencias listadas arriba

## Créditos y licencias
- Proyecto privado para uso educativo y personal.
- Basado en tecnologías open source.

---

¿Dudas o sugerencias? Contacta al desarrollador principal.
