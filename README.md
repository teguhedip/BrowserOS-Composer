# 🌐 BrowserOS

BrowserOS is an AI-native Chromium browser packaged as a self-contained Docker stack. It allows you to run a full-featured browser in a container and interact with it via a web-based VNC interface or through its built-in MCP (Model Context Protocol) server.

## 🚀 Features
- **AI-Native Chromium**: Pre-configured with BrowserOS capabilities.
- **Web-Based Access**: Integrated **noVNC** allows you to access the browser GUI directly from any modern web browser.
- **MCP Integration**: Includes a sidecar server for Model Context Protocol (MCP) interactions.
- **Self-Healing**: Managed by **Supervisord** to ensure Xvfb, x11vnc, and Chromium stay running.
- **Ready for AI**: Configured to point to Ollama for local LLM workflows.

## 🛠️ Quick Start

### 1. Prerequisites
- Docker and Docker Compose installed.
- (Recommended) **Dockge** or Portainer for stack management.

### 2. Deployment
Save the `browseros-compose.yaml` file to your server and run:

```bash
docker compose up -d
```

### 3. Accessing the Stack
Once the container is healthy, you can access BrowserOS at the following endpoints:


| Service | URL | Description |
| :--- | :--- | :--- |
| **Browser GUI** | `http://<HOST_IP>:6080` | Interactive noVNC session |
| **MCP Server** | `http://<HOST_IP>:9200/mcp` | HTTP endpoint for AI tool calls |

## ⚙️ Configuration

You can customize the environment variables in the `browseros-compose.yaml` file:

- **RESOLUTION**: Set the virtual display size (default: `1920x1080x24`).
- **shm_size**: Set to `2gb` to prevent Chromium crashes on heavy pages.

## 🏗️ Architecture
The stack uses a multi-stage Docker build:
1. **Fetcher**: Downloads and extracts the official BrowserOS AppImage.
2. **Runtime**: An Ubuntu 24.04 base equipped with X11, VNC, and all necessary Chromium dependencies.
3. **Supervisord**: Manages the lifecycle of:
   - `Xvfb` (Virtual frame buffer)
   - `x11vnc` (VNC server)
   - `novnc` (Web-to-VNC proxy)
   - `BrowserOS` (The Chromium engine)

## 🔒 Security Note
This container is configured with `seccomp:unconfined` to allow Chromium's sandbox to function correctly within Docker. The default VNC access is provided without a password for ease of use; it is recommended to run this behind a VPN or an authenticating reverse proxy (like Traefik or Nginx Proxy Manager) for public deployments.
