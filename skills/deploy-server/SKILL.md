---
name: deploy-server
description: Service deployment guide for gpu5090 server (172.23.22.100). Use when user needs to deploy Docker services, configure domain names, setup Caddy reverse proxy, or troubleshoot deployment issues.
---

# Deploy on Server Service

## Connection Info

| Item | Value |
|------|-------|
| **Server** | `172.23.22.100` (gpu5090) |
| **SSH** | `ssh service@172.23.22.100` |
| **Caddy Config** | `/home/public/.caddy/Caddyfile` |
| **Domain Format** | `*.gpu5090.whaleforce.dev` |
| **Port Range** | 8000 ~ 9499 |

## Quick Start - Fastest Deploy

```bash
# 1. Connect to server
ssh service@172.23.22.100

# 2. Create project directory
mkdir -p ~/myapp && cd ~/myapp

# 3. Create docker-compose.yml
cat > docker-compose.yml << 'EOF'
version: "3.9"
services:
  backend:
    build: ./backend
    container_name: myapp-backend
    network_mode: host
    restart: unless-stopped
    environment:
      PORT: "9001"

  frontend:
    build: ./frontend
    container_name: myapp-frontend
    network_mode: host
    restart: unless-stopped
    environment:
      PORT: "9000"
    depends_on:
      - backend
EOF

# 4. Start services
docker compose up -d --build

# 5. Verify
curl http://127.0.0.1:9000/
curl http://127.0.0.1:9001/api/health
```

## Common Operations

| Action | Command |
|--------|---------|
| Start service | `docker compose up -d` |
| Rebuild | `docker compose up -d --build` |
| View logs | `docker compose logs -f` |
| Restart | `docker compose restart` |
| Stop | `docker compose down` |
| Check port | `ss -lntp \| grep :9000` |

## Pre-Deploy Checklist

```bash
# 1. Verify SSH connection
ssh -o ConnectTimeout=5 service@172.23.22.100 "echo 'SSH OK'"

# 2. Verify Docker permission
ssh service@172.23.22.100 "docker ps --format '{{.Names}}' | head -3"

# 3. Check port availability
ssh service@172.23.22.100 "ss -lntp | grep -E ':(9000|9001)\b' || echo 'Ports available'"
```

## Upload Project Files

```bash
# Using rsync (recommended)
rsync -avz --exclude 'node_modules' --exclude '.git' \
  ./project/ service@172.23.22.100:~/myapp/

# Or using scp
scp -r ./project/* service@172.23.22.100:~/myapp/
```

## Setup Domain Name (HTTPS)

```bash
# Enter Caddy config directory
cd /home/public/.caddy

# Add domain config
cat >> Caddyfile << 'EOF'

myapp.gpu5090.whaleforce.dev:443 {
  import common_tls
  reverse_proxy 0.0.0.0:9000
}
EOF

# Reload Caddy
./reload.sh

# Verify
curl -fsS https://myapp.gpu5090.whaleforce.dev/
```

## Domain Config Examples

```caddyfile
# Frontend
myapp.gpu5090.whaleforce.dev:443 {
  import common_tls
  reverse_proxy 0.0.0.0:9000
}

# Backend API
myapp-api.gpu5090.whaleforce.dev:443 {
  import common_tls
  reverse_proxy 0.0.0.0:9001
}
```

## Port Rules

| Item | Rule |
|------|------|
| **Available Range** | 8000 ~ 9499 |
| **Network Mode** | Must use `host` |
| **Pre-deploy** | Check port not in use |

## Common Errors

| Error | Cause | Solution |
|-------|-------|----------|
| `permission denied...docker.sock` | User not in docker group | `sudo usermod -aG docker $USER` |
| `address already in use` | Port occupied | Change port or stop occupying service |
| `COPY failed: file not found` | Dockerfile path wrong | Check build context |
| `Connection refused` | SSH service down | Contact admin |
| `Connection timed out` | Network issue | Check VPN/firewall |
