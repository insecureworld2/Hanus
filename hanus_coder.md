# Hanus AI Coder 🛠️🤖

[![Python](https://img.shields.io/badge/Python-3.8%2B-blue.svg)](https://www.python.org/downloads/)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)
[![Gemini](https://img.shields.io/badge/Gemini-Compatible-orange.svg)](https://ai.google.dev/)
[![Grok](https://img.shields.io/badge/Grok-Compatible-purple.svg)](https://grok.x.ai/)

**Hanus AI Coder** es un agente programador interactivo y autónomo especializado en Python. Utiliza modelos de IA avanzados (como Gemini, Grok o GPT) para entender, refactorizar, documentar y extender proyectos de software de manera profesional y segura. El agente responde exclusivamente en formato XML con tags específicos para operaciones seguras como leer archivos, crear nuevos, reemplazar bloques de código, etc., evitando ediciones directas no confirmadas.

Ideal para desarrolladores que buscan un asistente IA que **modifique código automáticamente** mientras respeta reglas estrictas de seguridad y PEP 8.

## 🚀 Características Principales

- **Autonomía total**: El agente analiza el proyecto, planea cambios y aplica modificaciones vía tags XML (leer, escribir, crear archivos).
- **Integraciones IA flexibles**: Soporte nativo para Google Gemini y OnlineApp.pro (Grok-4, GPT-4, DeepSeek, etc.).
- **Herramientas de contexto**: Listado de archivos, árbol de proyecto, extracción de entidades Python (funciones/clases), previews de código.
- **Seguridad prioritaria**: Rutas relativas al root del proyecto, validación estricta, no sale del directorio de trabajo.
- **Personalizable**: Prompt del sistema editable en `prompt.md`, configuración de modelos y tokens.
- **Modo interactivo**: Chat con historial, streaming opcional para respuestas largas.
- **Documentación-first**: Prioriza claridad, mantenibilidad sobre rendimiento; respeta estilo existente.

## 📋 Requisitos

- Python 3.8+
- Comando `tree` (opcional, para visualización del árbol de proyecto; instala con `apt install tree` o similar).
- Dependencias: `requests`, `google-generativeai` (para Gemini), `pathlib`, `ast`, `subprocess` (estándar).

## 🛠️ Instalación

1. Clona o descarga el repositorio:
   ```
   git clone https://github.com/insecureworld2/hanus-coder.git
   cd hanus-coder
   ```

2. Crea un entorno virtual (recomendado):
   ```
   python -m venv .venv
   source .venv/bin/activate  # Linux/Mac
   # .venv\Scripts\activate  # Windows
   ```

3. Instala dependencias:
   ```
   pip install google-generativeai requests
   ```

4. Configura claves API (ver sección Configuración).

## ⚙️ Configuración

Copia `.env.example` a `.env` y edita:
```
GEMINI_API_KEY=tu_clave_gemini
ONLINEAPP_API_KEY=tu_clave_onlineapp  # Para Grok/etc.
```

O exporta directamente:
```bash
export GEMINI_API_KEY="AIza..."
export ONLINEAPP_API_KEY="sk-..."
```

Edita `prompt.md` para personalizar el comportamiento del agente (reglas absolutas ya incluidas).

## 📖 Uso Rápido

Ejecuta el agente:
```
python agent_runner.py
```

- Interactúa en modo chat: Describe cambios (e.g., "Agrega logging a todas las funciones").
- El agente **piensa** (`&lt;think&gt;`), **lee archivos** si necesita (`&lt;read_file&gt;`), **modifica** (`&lt;write_file&gt;`, etc.) y resume en `&lt;final_answer&gt;`.
- Salida: Solo tags XML válidos, cambios aplicados automáticamente.

**Ejemplo de interacción**:
```
Usuario: Crea un README.md profesional.
Agente: &lt;think&gt;Plan...&lt;/think&gt;
       &lt;create_file path="README.md"&gt;Contenido completo&lt;/create_file&gt;
       &lt;final_answer&gt;Hecho.&lt;/final_answer&gt;
```

Para modelos específicos, edita `tu_llm` en `agent_runner.py`.

## 📁 Estructura del Proyecto

```
.
├── agent_runner.py      # Flujo principal del agente (chat loop, LLM wrapper)
├── ia_integrations.py   # Clientes IA: GeminiClient, OnlineAppClient
├── project_tools.py     # list_files(), read_file_content(), get_project_tree(), extract_entities()
├── prompt.md            # Prompt del sistema (reglas del agente)
├── README.md            # Esta documentación
└── .env.example         # Plantilla de config (crear tú)
```

## 🔌 Integraciones IA

| Proveedor       | Clase                  | Modelos Soportados                  | Config |
|----------------|------------------------|-------------------------------------|--------|
| Google Gemini | `GeminiClient`        | `gemini-1.5-flash`, `gemini-pro`   | `GEMINI_API_KEY` |
| Internal | `Internal`     | `grok4t` (default), `gpt4`, `deepseek`, `GPT5` | `API_KEY` + model_name |

Extiende fácilmente en `ia_integrations.py`.

## 🧰 Herramientas del Proyecto (project_tools.py)

- `list_files()`: Lista archivos ignorando venv, .git, etc.
- `read_file_content(rel_path)`: Lee contenido seguro (UTF-8).
- `get_project_tree()`: Genera árbol con `tree` (fallback texto).
- `extract_python_entities(code)`: Extrae funciones, clases, asignaciones top-level con líneas y docstrings.

Usadas automáticamente por el agente para construir contexto.

## 🤝 Contribución

1. Forkea el repo.
2. Crea branch: `git checkout -b feature/nueva-feature`.
3. Commit: `git commit -m "Agrega X"`.
4. Push y PR.

**Guías de estilo**: PEP 8, type hints, black formatter (`pip install black; black .`), mypy.

Reporta issues en GitHub.

## 📄 Licencia

Este proyecto está bajo la [Licencia MIT](LICENSE). Ver `LICENSE` para detalles.

## 📈 Changelog

### v0.1.0 (Inicial)
- Agente runner con XML tags.
- Integraciones Gemini + OnlineApp.
- Project tools básicas.
- Prompt con reglas estrictas.

### Próximas versiones
- Tests unitarios.
- Soporte Docker.
- Más IAs (OpenAI oficial).
- UI web (Streamlit/Gradio).

¡Gracias por usar Hanus AI Coder! 🚀

---

*Desarrollado con ❤️ por CiTriX. Contribuciones bienvenidas.*
