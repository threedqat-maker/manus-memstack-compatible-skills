# Step 7: Image Optimization

**Optimization techniques applied:**

| Technique | Impact | Applied In |
|-----------|--------|-----------|
| **Multi-stage build** | 50-80% smaller image | Separate build/run stages |
| **Alpine base** | ~5MB vs ~100MB base | `node:20-alpine` vs `node:20` |
| **Non-root user** | Security hardening | `USER appuser` in final stage |
| **Layer caching** | Faster rebuilds | Copy package.json before source |
| **.dockerignore** | Smaller build context | Exclude node_modules, .git, docs |
| **Minimal final stage** | Only runtime files | No dev deps, no source in prod |
| **Combined RUN** | Fewer layers | Chain apt commands with `&&` |

**Image size comparison:**

| Approach | Typical Size | Notes |
|----------|-------------|-------|
| `node:20` (no optimization) | ~1 GB | Full Debian + dev tools |
| `node:20-slim` | ~200 MB | Slim Debian |
| `node:20-alpine` (multi-stage) | ~80-150 MB | Alpine + app only |
| Go with `scratch` | ~5-20 MB | Binary only, no OS |
| Python `slim` (multi-stage) | ~100-200 MB | Slim + deps only |

**Check image size:**
```bash
docker images | grep app
# Or detailed layer analysis:
docker history app:latest
```

### Step 8: Networking

**Development networking:**
```yaml
# docker-compose.yml — services on same default network
# Access between services by service name:
# app → db:5432, app → redis:6379
```

**Production networking:**
```yaml
# Explicit network for isolation
networks:
  frontend:
    driver: bridge
  backend:
    driver: bridge

services:
  app:
    networks: [frontend, backend]  # Talks to proxy AND database
  db:
    networks: [backend]            # Only accessible from app
  redis:
    networks: [backend]            # Only accessible from app
  nginx:
    networks: [frontend]           # Only talks to app
    ports: ['80:80', '443:443']    # Exposed to internet
```

**Port exposure rules:**
- Development: Expose DB/Redis ports for local tools (DBeaver, RedisInsight)
- Production: Only expose HTTP/HTTPS through reverse proxy
- Never expose database ports in production compose
- Use `127.0.0.1:5432:5432` instead of `5432:5432` to restrict to localhost

### Step 9: Volume Management

```
── VOLUMES ────────────────────────────────

Persistent data (named volumes):
  postgres_data    → /var/lib/postgresql/data   (DB files)
  redis_data       → /data                      (Redis AOF/RDB)
  uploads          → /app/uploads               (User uploads)

Development bind mounts:
  .:/app           → Live code reloading
  /app/node_modules → Prevent host overwriting container deps

Backup volumes:
  docker run --rm -v postgres_data:/data -v $(pwd):/backup \
    alpine tar czf /backup/postgres_backup.tar.gz /data
```

**Volume backup script:**
```bash
#!/bin/bash
# Backup all named volumes
DATE=$(date +%F)
BACKUP_DIR="/opt/backups/docker"
mkdir -p "$BACKUP_DIR"

for VOLUME in $(docker volume ls -q); do
  docker run --rm \
    -v "$VOLUME":/source:ro \
    -v "$BACKUP_DIR":/backup \
    alpine tar czf "/backup/${VOLUME}_${DATE}.tar.gz" -C /source .
done

# Cleanup backups older than 14 days
find "$BACKUP_DIR" -type f -mtime +14 -delete
```

### Step 10: Output

Present the complete Docker setup:

```
━━━ DOCKER SETUP ━━━━━━━━━━━━━━━━━━━━━━━━━
Project: [name]
Type: [language/framework]
Services: [app, db, redis, etc.]

── DOCKERFILE ─────────────────────────────
[complete multi-stage Dockerfile]
Final image size: ~[X] MB

── .DOCKERIGNORE ──────────────────────────
[complete .dockerignore]

── DOCKER-COMPOSE (Development) ───────────
[docker-compose.yml with dev services]
Start: docker compose up -d
Logs: docker compose logs -f app
Stop: docker compose down

── DOCKER-COMPOSE (Production) ────────────
[docker-compose.prod.yml]
Start: docker compose -f docker-compose.prod.yml up -d

── ENVIRONMENT ────────────────────────────
[env var strategy per environment]

── HEALTH CHECKS ──────────────────────────
[per-service health check config]

── NETWORKING ─────────────────────────────
[network topology for dev and prod]

── VOLUMES ────────────────────────────────
[persistent data + backup strategy]

── USEFUL COMMANDS ────────────────────────
Build:    docker compose build
Start:    docker compose up -d
Logs:     docker compose logs -f
Shell:    docker compose exec app sh
Rebuild:  docker compose up -d --build
Clean:    docker system prune -f
```

## Inputs
- Project type and language
- Required services (database, cache, queue)
- Development vs production needs
- Deployment target
- Existing Dockerfile (if any)

## Outputs
- Optimized multi-stage Dockerfile (Node.js, Python, Go, or Next.js)
- .dockerignore with comprehensive exclusions
- docker-compose.yml for development (app + services)
- docker-compose.prod.yml for production (with resource limits)
- Environment variable strategy per environment
- Health check configuration for all services
- Image optimization summary with size comparison
- Network topology (dev: open, prod: isolated)
- Volume management with backup script
- Quick-reference command cheat sheet
