# GEMINI.md - Context & Architecture Guide

**System Prompt for AI Assistants:**
You are an expert Full Stack Developer assisting in the creation of **Dasharr**, a homelab dashboard application. This document outlines the architectural decisions, tech stack, and core philosophy of the project. **Always refer to this file before generating code.**

## 1. Project Overview
**Name:** Dasharr
**Goal:** Create a dashboard that visualizes server data (via shell scripts/Docker) and application data (via APIs) in a user-friendly grid.
**USP:** "If you can script it, you can widget it."

## 2. Technical Stack (Strict)

### Backend
* **Language:** Python 3.11+
* **Framework:** **FastAPI** (Chosen for async support and ease of creating REST endpoints).
* **Task Management:** `APScheduler` (AsyncIOScheduler) to handle periodic background execution of user scripts.
* **Shell Execution:** Python `subprocess` (asyncio-compatible) for running shell commands.
* **Database:** `SQLite` (via **SQLModel** / SQLAlchemy) for storing dashboard layout and widget config. Keeping it light.

### Frontend
* **Framework:** **React** (Vite).
* **Language:** TypeScript.
* **Styling:** **Tailwind CSS** (No other CSS frameworks; keep it utility-first).
* **Grid System:** `react-grid-layout` (RGL) for the drag-and-drop dashboard canvas.
* **State Management:** `Zustand` (Simpler than Redux, perfect for widget state).
* **Icons:** `Lucide-React`.

## 3. Architecture & Data Flow

### The "Widget" Concept
A Widget is an object stored in the DB with:
1.  `id`: UUID.
2.  `type`: (e.g., `SHELL_COMMAND`, `RADARR_GRAPH`, `IFRAME`).
3.  `config`: JSON blob specific to the type (e.g., `{ "command": "zpool status -x", "refresh_interval": 60 }`).
4.  `layout`: `{ x, y, w, h }` for the grid.

### The "Runner" Engine (Backend)
* The backend does not just serve the API; it runs a background loop.
* It iterates through active widgets.
* If a widget is type `SHELL_COMMAND`, it executes the command on the host OS.
* **Security Note:** We assume the user is the admin. However, we should blacklist dangerous commands (`rm -rf`, `:(){ :|:& };:`) in the validation layer.

### Communication
* **REST:** For saving layouts and fetching initial configuration.
* **Polling/WebSockets:** The frontend polls the backend `/api/widgets/{id}/data` endpoint. (Upgrade to WebSockets later for real-time logs).

## 4. Specific Implementation Guidelines

### A. Shell Command Widget Logic
When generating code for the shell widget:
1.  Input: User provides a command string (e.g., `docker ps | grep 'healthy' | wc -l`).
2.  Execution: Backend runs this via `subprocess.run` with a timeout (5s max).
3.  Parsing: Simple stdout capture.
4.  Formatting: The frontend receives the raw string. The frontend config allows a "Regex Match" to color-code the output (e.g., `^0$` = Red, `>0` = Green).

### B. The "Arr" Integration
* Do NOT use external wrappers if possible. Use direct `httpx` calls to the Sonarr/Radarr APIs.
* We want *stats*, not *control*. Focus on endpoints like `/api/v3/calendar` and `/api/v3/queue`.

### C. Docker Integration
* Use the `docker` python library (or direct socket requests if needed) to fetch container stats.
* **Requirement:** The app must be able to group containers by regex (e.g., "Show all containers matching `mc-*`").

## 5. Directory Structure Target
```text
/dasharr
├── /backend
│   ├── /app
│   │   ├── main.py
│   │   ├── /api          # Routes
│   │   ├── /core         # Config, Database
│   │   ├── /models       # SQLModel definitions
│   │   ├── /services     # The "Runner" logic lives here
│   │   └── /schemas      # Pydantic models
│   ├── requirements.txt
│   └── Dockerfile
├── /frontend
│   ├── /src
│   │   ├── /components
│   │   │   ├── /widgets  # Individual widget components
│   │   │   └── /grid     # Dashboard layout logic
│   │   ├── /store        # Zustand stores
│   │   └── App.tsx
│   └── Dockerfile
└── docker-compose.yml
