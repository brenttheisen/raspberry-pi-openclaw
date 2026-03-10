# Raspberry Pi OpenClaw Docker Setup

## Purpose
Runs [OpenClaw](https://openclaw.dev) on a Raspberry Pi 3 (armv7) via Docker. OpenClaw is a self-hosted Claude AI gateway/proxy.

## Architecture
- **Base image**: `alpine/openclaw:2026.3.8` (official image, armv7-compatible)
- **Platform**: `linux/arm/v7` (Raspberry Pi 3)
- **Config**: Bind-mounted from `./config/` → `/home/node/.openclaw/` inside the container, so config persists across container rebuilds and is never baked into the image.

## Ports
| Port  | Purpose         |
|-------|-----------------|
| 18789 | Web UI / Gateway |
| 18790 | Bridge          |

## Build & Deploy

### Build the image
```bash
docker compose build
```

### Start the service
```bash
docker compose up -d
```

### View logs
```bash
docker compose logs -f
```

### Stop the service
```bash
docker compose down
```

## Configuration

Place your `openclaw.json` (and any other config files) in the `./config/` directory. On container start, these will be available at `/home/node/.openclaw/` inside the container.

After updating config, restart the container:
```bash
docker compose restart
```

### Environment Variables
Set sensitive values in the shell environment or a `.env` file (not committed to git):

| Variable | Description |
|----------|-------------|
| `OPENCLAW_GATEWAY_TOKEN` | Gateway authentication token |
| `CLAUDE_AI_SESSION_KEY` | Claude AI session key |
| `CLAUDE_WEB_SESSION_KEY` | Claude web session key |
| `CLAUDE_WEB_COOKIE` | Claude web cookie |

Example `.env` file:
```
OPENCLAW_GATEWAY_TOKEN=your-token-here
CLAUDE_AI_SESSION_KEY=your-key-here
```

## Cross-platform Build (from non-ARM host)

To build for armv7 from a Mac/Linux x86 machine:
```bash
docker buildx create --use
docker compose build
```

## Web UI
Once running, access the web UI at: `http://<pi-ip>:18789`
