# AsusWRT MCP — Home Assistant Add-on

Home Assistant add-on that runs [asuswrt-mcp](https://github.com/Teeflo/asuswrt-mcp) and exposes it over **SSE** so your **local LLM** (Ollama, etc.) can diagnose and safely repair AsusWRT / AsusWRT-Merlin router configuration.

This is **not** a replacement for [ha-mcp](https://github.com/homeassistant-ai/ha-mcp).  
ha-mcp controls Home Assistant. This add-on talks to the **router over SSH**.

## Features

- SSH-only, allowlisted router operations (no arbitrary shell)
- 40+ read-only diagnostic tools (health, WAN/LAN, DHCP, clients, Wi-Fi, port forwards, …)
- Optional guarded mutations (dry-run + `confirm` required)
- SSE endpoint for Home Assistant’s **Model Context Protocol** client integration
- Mutations **disabled by default**

## Install (Home Assistant OS / Supervised)

1. **Settings → Add-ons → Add-on store → ⋮ → Repositories**
2. Add:
   ```text
   https://github.com/cjprsnin/asuswrt-mcp-addon
   ```
3. Refresh the store, install **AsusWRT MCP**
4. Configure router host, SSH username, password (or key path)
5. Start the add-on
6. Note the MCP URL from the add-on logs / docs (default port **8765**)

## Connect to your local LLM

1. **Settings → Devices & services → Add Integration → Model Context Protocol**
2. SSE URL example:
   ```text
   http://homeassistant.local:8765/sse
   ```
   (Use your HA host IP if needed. With `host_network`, the port is on the HA host.)
3. Assign the discovered tools to your Ollama (or other) conversation agent / Assist pipeline

## Configuration options

| Option | Default | Description |
|--------|---------|-------------|
| `host` | `192.168.1.1` | Router IP |
| `ssh_username` | `admin` | SSH user |
| `ssh_password` | *(empty)* | SSH password |
| `ssh_key_file` | *(empty)* | Optional private key path inside the container |
| `ssh_port` | `22` | SSH port |
| `allow_mutations` | `false` | Enable write tools |
| `timeout_seconds` | `10` | SSH timeout |
| `log_level` | `info` | Logging |

## Safety

- Leave **allow_mutations** off until the model reliably uses read-only tools
- Even with mutations on, tools require `confirm=true` (and support `dry_run=true`)
- Do not expose port 8765 to the public internet

## Upstream

Python package: [Teeflo/asuswrt-mcp](https://github.com/Teeflo/asuswrt-mcp) (MIT)

## License

MIT — see [LICENSE](LICENSE)
