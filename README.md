# Birdseye Agent Releases

Official releases of the Birdseye Windows monitoring agent.

## Download

**Latest release:** [Releases page](https://github.com/rwarettg/birdseye-releases/releases/latest)

Direct download URL pattern:
```
https://github.com/rwarettg/birdseye-releases/releases/download/agent-vX.X.X/birdseye-agent.exe
```

## Installation

1. Download `birdseye-agent.exe` from the latest release
2. Place in `C:\Birdseye\`
3. Create `config.json` in the same directory:
```json
{
  "server_url": "wss://your-server.com/ws/agent",
  "api_key": "YOUR_API_KEY"
}
```
4. Install as Windows service:
```powershell
sc create BirdseyeAgent binPath="C:\Birdseye\birdseye-agent.exe" start=auto
sc start BirdseyeAgent
```

## Verification

Each release includes `SHA256SUMS.txt` for integrity verification:

```powershell
# PowerShell
(Get-FileHash .\birdseye-agent.exe -Algorithm SHA256).Hash
```

Compare the output with the hash in `SHA256SUMS.txt`.

## What is Birdseye?

Birdseye is a fleet monitoring platform for Windows machines. The agent runs as a Windows service and:

- Reports system metrics (CPU, memory, disk, processes)
- Maintains WebSocket connection to central server
- Enables remote terminal access
- Supports self-updating

## Release History

| Version | Date | Notes |
|---------|------|-------|
| agent-v1.0.0 | 2025-03-03 | Initial release |

---

Built automatically via GitHub Actions from the private source repository.
