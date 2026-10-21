# AsusWRT MCP

Exposes an [asuswrt-mcp](https://github.com/Teeflo/asuswrt-mcp) Model Context Protocol server so Home Assistant conversation agents (including **local Ollama**) can inspect and optionally repair AsusWRT / Merlin routers over SSH.

## Configuration

| Option | Description |
|--------|-------------|
| **host** | Router IP or hostname |
| **ssh_username** | SSH login (often `admin`) |
| **ssh_password** | SSH password (leave empty if using a key) |
| **ssh_key_file** | Optional path to a private key **inside the container** |
| **ssh_port** | SSH port (default 22) |
| **allow_mutations** | Enable write tools (default **off**) |
| **timeout_seconds** | SSH operation timeout |
| **log_level** | `debug` / `info` / `warning` / `error` |

The add-on uses **host networking** so it can reach the router on your LAN.

## MCP endpoint

After start, the SSE endpoint is:

```text
http://HOME_ASSISTANT_HOST:8765/sse
```

Example: `http://homeassistant.local:8765/sse` or `http://192.168.1.10:8765/sse`.

Check the add-on **Log** tab for startup messages and connection errors.

## Wire into Home Assistant

1. Install and start this add-on; confirm the log shows the proxy listening.
2. **Settings → Devices & services → Add Integration → Model Context Protocol**
3. Enter the SSE URL above.
4. Open your Assist / conversation pipeline that uses Ollama (or another tool-capable model) and enable the MCP tools / LLM API from this server.

### Suggested agent instructions

```text
You can use AsusWRT MCP tools to diagnose the home router.
Always start with read-only tools (health, network overview, DHCP, clients).
Explain findings before any change.
For mutations: dry_run=true first, then only confirm=true after the user agrees.
Never change firmware or run arbitrary commands.
```

## Mutations

Leave **allow_mutations** disabled until diagnosis works reliably.

When enabled, the upstream server still requires `confirm=true` on write tools and supports `dry_run=true` previews. Prefer dry-run for DHCP, firewall, wireless, and port-forward changes.

## Troubleshooting

| Symptom | What to check |
|---------|----------------|
| MCP integration cannot connect | Port 8765, host IP, add-on running, firewall |
| SSH failures in log | Host, user, password/key, router SSH enabled |
| Tools missing in agent | MCP client integration added; pipeline has tools enabled |
| Model ignores tools | Use a tool-capable local model; reduce tool count if needed |

## Upstream & license

- Server logic: [Teeflo/asuswrt-mcp](https://github.com/Teeflo/asuswrt-mcp) (MIT)
- This add-on packaging: MIT
