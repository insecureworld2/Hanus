# Hanus Agent

Hanus Agent is a multimodal, extensible, and customizable artificial intelligence assistant designed for voice and text interaction, with integration of advanced AI models, custom tools, and a web administration panel.

---

## Table of Contents

- [Key Features](#key-features)
- [Advanced Self-Programming Capabilities](#advanced-self-programming-capabilities)
- [Sample Included Tools](#sample-included-tools)
- [Architecture](#architecture)
- [Folder Structure](#folder-structure)
- [Installation](#installation)
- [Usage](#usage)
- [Main Endpoints](#main-endpoints)
- [Customization & Extensibility](#customization--extensibility)
- [Requirements](#requirements)
- [Credits & License](#credits--license)

---

## Why Choose Hanus?

- **Enterprise-ready**: Robust, modular, and scalable architecture, ready for production and enterprise deployment.
- **Security & Control**: Code execution in restricted environments, parameter validation, and logging for auditability.
- **Advanced Automation**: Ability to self-improve, create new functions, and adapt to changing needs without manual intervention.
- **Total Integration**: RESTful API, intuitive web panel, and support for integration with external systems and AI pipelines.
- **Multimodal Support**: Natural interaction via voice and text, adaptable to physical assistants, bots, or web applications.
- **Documentation & Maintainability**: Well-documented code, clear structure, and usage examples for easy adoption and maintenance.

## Roadmap & Vision

- **Integration with more AI models**: Support for Llama, Qwen, Claude, and local models.
- **Plugins & Skills**: Plugin system to extend functionalities without touching the core.
- **Analytics Panel**: Usage statistics, interaction logs, and performance metrics.
- **Cloud & On-premise Deployment**: Scripts and containers ready for Kubernetes, Docker Compose, and dedicated servers.
- **Internationalization**: Multilanguage support and cultural adaptation.

## Recognition & Motivation

Hanus is born from a passion for applied AI, automation, and the creation of truly useful, secure, and customizable assistants. Inspired by the best copilots and assistants on the market, but with total control and privacy.

---

## Key Features

- **Voice and text interaction**: Spanish speech recognition and synthesis (STT/TTS) using Whisper and Edge TTS.
- **Multi-AI support**: Integration with models like Gemini, GPT, Grok, DeepSeek, and more.
- **Custom tools and functions**: Created from the web panel or by the AI, executable on demand or periodically.
- **Web admin panel**: Manage AIs, triggers, tools, and users.
- **Continuous voice chat**: Wake word activation ("Hanus"/"Janus"), automatic listening and response.
- **Scheduled tasks & heartbeat**: Periodic task execution and system monitoring.
- **Persistence & database**: Users, configs, functions, and triggers stored in SQLite.

## Advanced Self-Programming Capabilities

- **Code generation and self-integration**: Hanus can create new Python functions (tools) from the web panel or natural language instructions, integrate them automatically, and execute them on demand or periodically—similar to GitHub Copilot, but in your own environment.
- **Dynamic extensibility**: The agent itself can register new tools on the fly, without restarting the server, using its own `self_register_tool` utility.
- **AI that codes**: Hanus is able to write Python code, integrate and test it, allowing it to evolve and adapt to new needs autonomously.

## Sample Included Tools

- **moltbook**: Client to interact with the Moltbook API (create posts, query data, etc.).
- **self_register_tool**: Allows Hanus to register new functions/tools on the fly, saving code and metadata to the database and filesystem.
- **docker_run_server**: Creates and runs servers (Flask, FastAPI, Node.js, etc.) in Docker containers.
- **web_search**: Performs internet searches and returns summaries.
- **calculator**: Safely evaluates mathematical expressions.
- **check_ram_usage, check_cpu_usage, check_disk_usage**: System monitoring.
- **camera_snapshot, camera_move, camera_speak**: RTSP camera control.

You can view and test all available tools from the web panel or by checking the `tools_registry.json` file.

## Architecture

- **Backend**: Python + Flask
- **Frontend**: HTML5, Bootstrap, JS (Jinja2 templates)
- **Speech recognition**: [faster-whisper](https://github.com/SYSTRAN/faster-whisper)
- **Speech synthesis**: [Edge TTS](https://github.com/rany2/edge-tts)
- **Database**: SQLite (SQLAlchemy)
- **Tasks & scheduler**: APScheduler
- **AI integrations**: Gemini, OnlineApp.pro, etc.

## Folder Structure

```
classAI/
├── app.py                  # External API client (optional)
├── hanus/
│   ├── run.py              # Flask server entry point
│   ├── config.py           # Global config
│   ├── hanus_voice_local.py# Local voice client (microphone)
│   ├── app/
│   │   ├── __init__.py     # Flask & DB initialization
│   │   ├── api.py          # REST/JSON endpoints (voice, chat, tools)
│   │   ├── admin.py        # Web admin panel
│   │   ├── hanus_core.py   # Hanus main logic
│   │   ├── voice.py        # STT/TTS
│   │   ├── models.py       # SQLAlchemy models
│   │   ├── ia_integrations.py # AI provider integration
│   │   ├── tools/          # Tools and functions
│   │   ├── templates/      # Web templates (admin, chat, voice, tools)
│   │   └── ...
│   ├── voces/              # Training/user audios
│   └── instance/site.db    # SQLite database
└── ...
```

## Installation

1. **Clone the repository**
2. **Install dependencies**:
   ```zsh
   pip install -r requirements.txt
   # Or install manually:
   pip install flask flask_sqlalchemy flask_login flask_wtf apscheduler faster-whisper edge-tts pyaudio numpy scipy requests
   ```
3. **Configure variables** (optional):
   - Edit `hanus/config.py` for custom keys and paths.
4. **Initialize the database**:
   ```zsh
   cd hanus
   flask shell
   >>> from app import db, create_app
   >>> app = create_app()
   >>> with app.app_context(): db.create_all()
   >>> exit()
   ```
5. **Run the server**:
   ```zsh
   python hanus/run.py
   ```
6. **(Optional) Run the local voice client**:
   ```zsh
   python hanus/hanus_voice_local.py
   ```

## Usage

- Access the web panel: [http://localhost:5000/admin](http://localhost:5000/admin)
- Try the continuous voice chat or text chat.
- Create and manage AIs, triggers, and tools from the panel.
- Use the `/api/hanus` endpoint for programmatic integration.

## Main Endpoints

- `POST /api/hanus` — AI chat (JSON: `{message, uid}`)
- `POST /api/tts` — Text to speech (JSON: `{text, voice}`)
- `POST /api/stt` — Audio to text (form-data: `audio`)
- `POST /api/hanus/clear_history` — Clear user history

## Customization & Extensibility

- **Tools**: Add Python functions in the panel or in `app/tools/`.
- **AIs**: Configure new models and prompts in the panel.
- **Triggers**: Define rules to route messages to different AIs.
- **Scheduled tasks**: Program automatic tool executions.

## Requirements
- Python 3.10+
- Linux recommended (partial support for Windows/Mac)
- Dependencies listed above

## Credits & License
- Private project for educational and personal use.
- Based on open source technologies.

---

Questions or suggestions? Contact the lead developer.
