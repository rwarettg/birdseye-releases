# Birdseye Agent

The Birdseye agent is a lightweight Windows service that connects your machines to the Birdseye fleet monitoring platform. It maintains a persistent WebSocket connection to the Birdseye backend, enabling real-time monitoring, remote management, and AI-driven automation of your Windows fleet.

## What is Birdseye?

Birdseye is a platform for **Windows fleet monitoring** and **agentic device management**. It pairs a backend API server with a React dashboard and this Windows agent to give you full visibility and control over your fleet.

### Key Capabilities

**Real-Time Monitoring**
- CPU, memory, disk usage, and process metrics
- Windows Update status (pending updates, reboot readiness)
- Installed software inventory
- Device health tracking with automatic degradation detection

**Remote Management**
- Interactive remote PowerShell terminal via the dashboard
- Structured command execution via MCP (Model Context Protocol)
- Remote device restart

**Agentic Automation via MCP**

Birdseye exposes an MCP server that AI agents (like Claude) can use as tools to manage your fleet programmatically:

| MCP Tool | Description |
|----------|-------------|
| `list_devices` | List all devices with online status |
| `get_device_info` | Get full system report for a device |
| `update_agent` | Update the agent on all or specific devices |
| `trigger_windows_updates` | Set registry keys and trigger Windows Update scan/download/install |
| `restart_device` | Restart a device remotely |
| `start_session` / `run_command` / `end_session` | Open a PowerShell session and execute commands |

This enables workflows like:
- An AI agent checking for pending updates across your fleet and triggering installs
- Automated device maintenance: update agents, install Windows updates, restart when ready
- On-demand diagnostics: query device info, check processes, inspect disk usage
- Scripted fleet-wide operations via PowerShell sessions

**Self-Updating**
- The agent can update itself when triggered via the dashboard or MCP
- Downloads new binary, verifies SHA256 hash, replaces itself, restarts the service
- Automatic rollback if the update fails

## Download

**Latest release:** [Releases page](https://github.com/rwarettg/birdseye-releases/releases/latest)

Direct download URL pattern:
```
https://github.com/rwarettg/birdseye-releases/releases/download/agent-vX.X.X/birdseye-agent.exe
```

## Installation

1. Download `birdseye-agent.exe` from the latest release
2. Download the installer script from the source repository
3. Run as Administrator:
```powershell
.\Install-BirdseyeRustAgent.ps1
```

The installer:
- Places the agent in `C:\Program Files\BirdseyeAgent\`
- Registers it as a Windows service with automatic startup
- Configures failure recovery (auto-restart on crash)
- Starts the service

To uninstall:
```powershell
.\Install-BirdseyeRustAgent.ps1 -Uninstall
```

## Verification

Each release includes `SHA256SUMS.txt` for integrity verification:

```powershell
(Get-FileHash .\birdseye-agent.exe -Algorithm SHA256).Hash
```

Compare the output with the hash in `SHA256SUMS.txt`.

## Architecture

```
Windows Machine                    Birdseye Backend
+-------------------+             +------------------+
|  Birdseye Agent   |  WebSocket  |  Express API     |
|  (Windows Service)|<----------->|  + MCP Server    |
|                   |             |  + WebSocket Hub  |
|  - System metrics |             +------------------+
|  - WinUpdate mgmt |                    |
|  - Remote terminal|             +------------------+
|  - Self-update    |             |  React Dashboard |
+-------------------+             +------------------+
```

---

Built automatically via GitHub Actions. Agent source code is maintained in a private repository.
