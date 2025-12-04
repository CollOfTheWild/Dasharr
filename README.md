# Dasharr

**The "Script-First" Dashboard for Homelabbers.**

> *Because sometimes you don't just want to launch an app—you want to know if your ZFS pool is degrading or which Minecraft server is hogging the RAM.*

![License](https://img.shields.io/badge/license-%20%20GNU%20GPLv3%20-green?style=flat-square)
![Status](https://img.shields.io/badge/status-pre--alpha-red?style=flat-square)

## 🏴‍☠️ What is Dasharr?

Dasharr is a self-hosted dashboard builder designed to fill the gap between simple "Launchers" (like Homarr/Heimdall) and complex "Data Visualization" tools (like Grafana).

While other dashboards are great at giving you a button to open Sonarr, **Dasharr** is built to show you what is happening *inside* Sonarr—or inside your server itself.

### The Core Philosophy: "Everything is a Command"
If you can run a bash command for it, you can have a widget for it. Dasharr doesn't wait for plugins; it lets you pipe the output of `docker ps`, `zpool status`, or `cat /sys/class/thermal/thermal_zone0/temp` directly into a visual widget.

## ✨ Features (Planned)

### 🖥️ The "Script-to-Widget" Engine
* **Native Shell Support:** Create a widget that runs a shell command on an interval.
* **Regex Parsing:** Map the output of your command (e.g., "ONLINE") to visual states (Green/Red).
* **Docker Socket Integration:** Monitor container health without complex exporters.

### ⚓ Deep Integration with the *Arrs
* **Library Growth:** Graphs showing movies/shows added over time.
* **Quality Breakdown:** Pie charts showing 4K vs 1080p, or x265 vs h264.
* **Queue Velocity:** See real-time download speeds across all clients.

### 🎮 Game Server Monitoring
* **Unified Minecraft View:** Aggregate stats from multiple Minecraft containers (`mc-1710`, `mc-bedwars`, etc.) into a single, sortable table.
* **Player Counts & TPS:** Live tracking of server performance.

### 🏠 IoT & Network
* **MQTT Support:** Subscribe to topics from Mosquitto/Frigate to flash alerts on your dashboard when a doorbell rings or a person is detected.
* **Ad-Blocking Stats:** Pi-hole/AdGuard Home query volume and block percentages.

## 🛠️ Tech Stack

* **Backend:** Python (FastAPI) - For easy OS interaction and shell execution.
* **Frontend:** React + Tailwind CSS - For a responsive, drag-and-drop grid layout.
* **Communication:** REST API + WebSockets (for real-time updates).

## 🚀 Getting Started

*Current Status: Initial Development. Contributors welcome!*

### Prerequisites
* Docker & Docker Compose
* A Linux-based host (Ubuntu/Debian recommended for shell features)

### Installation (Dev)

1.  Clone the repository:
    ```bash
    git clone [https://github.com/CollOfTheWild/Dasharr.git](https://github.com/CollOfTheWild/Dasharr.git)
    cd Dasharr
    ```
2.  (Instructions coming soon as we build the MVP)

## 🗺️ Roadmap

- [ ] **Phase 1: The Core**
    - [ ] Basic React Grid Layout.
    - [ ] Python Backend with "Command Runner" endpoint.
    - [ ] "Hello World" Widget (ZFS Health Check).
- [ ] **Phase 2: The *Arrs**
    - [ ] Sonarr/Radarr API Connectors.
    - [ ] Library Stats Widgets.
- [ ] **Phase 3: The Advanced**
    - [ ] Minecraft Multi-Server Aggregator.
    - [ ] MQTT/IoT Event Listeners (Ring/Frigate integration).

## 🤝 Contributing

We welcome pull requests! Please read `CONTRIBUTING.md` (coming soon) for details on our code of conduct, and the process for submitting pull requests to us.

## 📄 License

This project is licensed under the **GNU General Public License v3.0** - see the [LICENSE](LICENSE) file for details.
