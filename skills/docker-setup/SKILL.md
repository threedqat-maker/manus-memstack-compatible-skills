---
name: docker-setup
description: "Use when the user asks for 'Docker', 'Dockerfile', 'docker-compose', 'containerize', 'docker-setup', or needs to containerize an application with optimized Docker images and compose configurations. Do not use for serverless or static site deployments."
---

> **Conversion status: Manus-ready draft.** This skill was mechanically converted from the public MemStack MIT-licensed source and should be reviewed during first real use.

> **Original source:** `cwinvestments/memstack/skills/deployment/docker-setup/SKILL.md`.

#  Docker Setup — Container Configuration Generator
*Analyze a project and generate optimized Dockerfiles, docker-compose configs, and deployment-ready container setups with health checks and volume management.*

## Scope Guard

Use this skill when the task matches the skill description and the user needs this specific workflow or deliverable.

| Use this skill for | Do not use this skill for |
|---|---|
| Use when the user asks for 'Docker', 'Dockerfile', 'docker-compose', 'containerize', 'docker-setup', or needs to containerize an application with optimized Docker images and compose configurations. | serverless or static site deployments. |

## Protocol

### Step 1: Gather Inputs

Ask the user for:
- **Project type**: Node.js, Python, Go, Rust, static site?
- **Purpose**: Local development, production deployment, or both?
- **Services needed**: Database (PostgreSQL, MySQL), cache (Redis), queue (RabbitMQ)?
- **Build artifacts**: Does the project compile to a binary or run interpreted?
- **Environment**: Where will containers run? (local, VPS, cloud container service)

### Step 2: Analyze Project & Generate Dockerfile

Detect project type from files and generate an optimized multi-stage Dockerfile:

**Node.js (multi-stage):**
```dockerfile
# Stage 1: Install dependencies
FROM node:20-alpine AS deps
WORKDIR /app
COPY package.json package-lock.json ./
RUN npm ci --ignore-scripts

# Stage 2: Build
FROM node:20-alpine AS builder
WORKDIR /app
COPY --from=deps /app/node_modules ./node_modules
COPY . .
RUN npm run build

# Stage 3: Production
FROM node:20-alpine AS runner
WORKDIR /app
ENV NODE_ENV=production

# Security: run as non-root
RUN addgroup --system --gid 1001 appgroup && \
    adduser --system --uid 1001 appuser

COPY --from=builder --chown=appuser:appgroup /app/dist ./dist
COPY --from=builder --chown=appuser:appgroup /app/node_modules ./node_modules
COPY --from=builder --chown=appuser:appgroup /app/package.json ./

USER appuser
EXPOSE 3000

HEALTHCHECK --interval=30s --timeout=3s --start-period=10s --retries=3 \
  CMD wget --no-verbose --tries=1 --spider http://localhost:3000/health || exit 1

CMD ["node", "dist/index.js"]
```

**Python (multi-stage):**
```dockerfile
# Stage 1: Build
FROM python:3.12-slim AS builder
WORKDIR /app
RUN pip install --no-cache-dir --upgrade pip
COPY requirements.txt .
RUN pip install --no-cache-dir --prefix=/install -r requirements.txt

# Stage 2: Production
FROM python:3.12-slim AS runner
WORKDIR /app

# Security: run as non-root
RUN groupadd --gid 1001 appgroup && \
    useradd --uid 1001 --gid appgroup --shell /bin/false appuser

COPY --from=builder /install /usr/local
COPY --chown=appuser:appgroup . .

USER appuser
EXPOSE 8000

HEALTHCHECK --interval=30s --timeout=3s --start-period=10s --retries=3 \
  CMD python -c "import urllib.request; urllib.request.urlopen('http://localhost:8000/health')" || exit 1

CMD ["python", "-m", "uvicorn", "main:app", "--host", "0.0.0.0", "--port", "8000"]
```

**Go (minimal final image):**
```dockerfile
# Stage 1: Build
FROM golang:1.22-alpine AS builder
WORKDIR /app
COPY go.mod go.sum ./
RUN go mod download
COPY . .
RUN CGO_ENABLED=0 GOOS=linux go build -ldflags="-s -w" -o /app/server .

# Stage 2: Production (scratch — smallest possible image)
FROM scratch AS runner
COPY --from=builder /app/server /server
COPY --from=builder /etc/ssl/certs/ca-certificates.crt /etc/ssl/certs/

EXPOSE 8080

HEALTHCHECK --interval=30s --timeout=3s --retries=3 \
  CMD ["/server", "healthcheck"]

ENTRYPOINT ["/server"]
```

**Next.js (standalone output):**
```dockerfile
FROM node:20-alpine AS deps
WORKDIR /app
COPY package.json package-lock.json ./
RUN npm ci

FROM node:20-alpine AS builder
WORKDIR /app
COPY --from=deps /app/node_modules ./node_modules
COPY . .
RUN npm run build

FROM node:20-alpine AS runner
WORKDIR /app
ENV NODE_ENV=production
ENV NEXT_TELEMETRY_DISABLED=1

RUN addgroup --system --gid 1001 appgroup && \
    adduser --system --uid 1001 appuser

COPY --from=builder --chown=appuser:appgroup /app/.next/standalone ./
COPY --from=builder --chown=appuser:appgroup /app/.next/static ./.next/static
COPY --from=builder --chown=appuser:appgroup /app/public ./public

USER appuser
EXPOSE 3000

HEALTHCHECK --interval=30s --timeout=3s --start-period=15s --retries=3 \
  CMD wget --no-verbose --tries=1 --spider http://localhost:3000/ || exit 1

CMD ["node", "server.js"]
```

### Step 3: Generate .dockerignore

```dockerignore
# Dependencies
node_modules/
.pnp
.pnp.js

# Build output (built inside container)
dist/
build/
.next/
out/

# Environment files (secrets)
.env
.env.local
.env.*.local

# Version control
.git
.gitignore

# IDE
.vscode/
.idea/
*.swp
*.swo

# OS files
.DS_Store
Thumbs.db

# Docker (prevent recursive copy)
Dockerfile
docker-compose*.yml
.dockerignore

# Testing & development
coverage/
*.test.js
*.spec.js
__tests__/
.nyc_output/

# Documentation
README.md
CHANGELOG.md
docs/

# CI/CD
.github/
.gitlab-ci.yml
```

### Step 4: Generate docker-compose.yml

**Development compose (app + database + cache):**
```yaml
# docker-compose.yml
version: '3.8'

services:
  app:
    build:
      context: .
      dockerfile: Dockerfile
      target: builder  # Use builder stage for dev (with dev dependencies)
    ports:
      - '3000:3000'
    environment:
      - NODE_ENV=development
      - DATABASE_URL=postgresql://postgres:postgres@db:5432/app_dev
      - REDIS_URL=redis://redis:6379
    volumes:
      - .:/app
      - /app/node_modules  # Prevent overwriting container node_modules
    depends_on:
      db:
        condition: service_healthy
      redis:
        condition: service_healthy
    restart: unless-stopped

  db:
    image: postgres:16-alpine
    ports:
      - '5432:5432'
    environment:
      POSTGRES_USER: postgres
      POSTGRES_PASSWORD: postgres
      POSTGRES_DB: app_dev
    volumes:
      - postgres_data:/var/lib/postgresql/data
    healthcheck:
      test: ['CMD-SHELL', 'pg_isready -U postgres']
      interval: 10s
      timeout: 5s
      retries: 5

  redis:
    image: redis:7-alpine
    ports:
      - '6379:6379'
    volumes:
      - redis_data:/data
    healthcheck:
      test: ['CMD', 'redis-cli', 'ping']
      interval: 10s
      timeout: 5s
      retries: 5
    command: redis-server --appendonly yes

volumes:
  postgres_data:
  redis_data:
```

**Production compose:**
```yaml
# docker-compose.prod.yml
version: '3.8'

services:
  app:
    build:
      context: .
      dockerfile: Dockerfile
      target: runner  # Production stage
    ports:
      - '3000:3000'
    env_file:
      - .env.production
    depends_on:
      db:
        condition: service_healthy
    restart: always
    deploy:
      resources:
        limits:
          memory: 512M
          cpus: '1.0'
        reservations:
          memory: 256M
          cpus: '0.5'

  db:
    image: postgres:16-alpine
    env_file:
      - .env.production
    volumes:
      - postgres_data:/var/lib/postgresql/data
    healthcheck:
      test: ['CMD-SHELL', 'pg_isready -U $${POSTGRES_USER}']
      interval: 10s
      timeout: 5s
      retries: 5
    restart: always
    deploy:
      resources:
        limits:
          memory: 1G

volumes:
  postgres_data:
    driver: local
```

### Step 5: Environment Variable Handling

```
── ENVIRONMENT STRATEGY ───────────────────

Development (.env — local only, gitignored):
  DATABASE_URL=postgresql://postgres:postgres@db:5432/app_dev
  REDIS_URL=redis://redis:6379
  API_KEY=dev_key_not_real
  NODE_ENV=development

Production (.env.production — on server, never in repo):
  DATABASE_URL=postgresql://user:STRONG_PASS@db:5432/app_prod
  REDIS_URL=redis://:STRONG_PASS@redis:6379
  API_KEY=prod_real_key
  NODE_ENV=production
```

**Docker env var priority (highest to lowest):**
1. `docker run -e VAR=value` (command line)
2. `environment:` in docker-compose.yml
3. `env_file:` in docker-compose.yml
4. `ENV` in Dockerfile (build-time defaults only)

**Secrets handling:**
- Never put secrets in Dockerfile `ENV` (visible in image layers)
- Use `env_file` for production (file on server, not in repo)
- For CI/CD: inject via `docker run -e` from pipeline secrets
- Consider Docker Secrets for Swarm/production deployments

### Step 6: Health Check Configuration

Design health checks for each service:

```
── HEALTH CHECKS ──────────────────────────

App:
  Endpoint: GET /health
  Response: { "status": "ok", "db": "connected", "redis": "connected" }
  Interval: 30s
  Timeout: 3s
  Start period: 10s (allow app to boot)
  Retries: 3

PostgreSQL:
  Command: pg_isready -U postgres
  Interval: 10s
  Timeout: 5s
  Retries: 5

Redis:
  Command: redis-cli ping
  Interval: 10s
  Timeout: 5s
  Retries: 5
```

**Application health endpoint implementation:**
```javascript
// GET /health
app.get('/health', async (req, res) => {
  const checks = {
    status: 'ok',
    uptime: process.uptime(),
    timestamp: new Date().toISOString(),
  };

  try {
    await db.query('SELECT 1');
    checks.db = 'connected';
  } catch {
    checks.db = 'disconnected';
    checks.status = 'degraded';
  }

  try {
    await redis.ping();
    checks.redis = 'connected';
  } catch {
    checks.redis = 'disconnected';
    checks.status = 'degraded';
  }

  const statusCode = checks.status === 'ok' ? 200 : 503;
  res.status(statusCode).json(checks);
});
```

### Additional Detailed Guidance

Read `references/advanced-docker-operations.md` when the task requires image optimization, networking, and volume-management details. Keep the main workflow concise unless those details are needed.
