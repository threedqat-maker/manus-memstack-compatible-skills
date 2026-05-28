---
name: hetzner-setup
description: "Use when the user asks for 'Hetzner', 'VPS setup', 'server provisioning', 'deploy to VPS', 'hetzner-setup', 'cloud server', or needs to provision, harden, and deploy applications to a Hetzner Cloud server with Docker, Nginx/Caddy, SSL, and monitoring. Do not use for managed platform deployments like Railway or Netlify."
---

> **Conversion status: Manus-ready draft.** This skill was mechanically converted from the public MemStack MIT-licensed source and should be reviewed during first real use.

> **Original source:** `cwinvestments/memstack/skills/deployment/hetzner-setup/SKILL.md`.

# ️ Hetzner Setup — VPS Provisioning & Deployment
*Provision a Hetzner Cloud server with security hardening, reverse proxy, SSL, database setup, monitoring, and automated backups.*

## Protocol

### Step 1: Gather Inputs

Ask the user for:
- **Workload type**: Web app, API, database, scraper, background worker?
- **Expected traffic**: Low (< 1k/day), medium (1k-50k/day), high (50k+/day)?
- **Stack**: Node.js, Python, Go, Rust? Docker or direct?
- **Database needs**: PostgreSQL, MySQL, Redis, none?
- **Domain**: Do you have a domain to point at this server?
- **Budget preference**: Minimum viable or room to grow?

### Step 2: Recommend Instance Type

Match workload to Hetzner Cloud server:

| Instance | vCPU | RAM | SSD | Traffic | Price | Best For |
|----------|------|-----|-----|---------|-------|----------|
| **CX22** | 2 | 4 GB | 40 GB | 20 TB | ~$4.5/mo | Small API, static site, hobby project |
| **CX32** | 4 | 8 GB | 80 GB | 20 TB | ~$8/mo | Medium web app, small DB |
| **CPX31** | 4 | 8 GB | 160 GB | 20 TB | ~$13/mo | High-perf apps (AMD EPYC), scrapers |
| **CPX41** | 8 | 16 GB | 240 GB | 20 TB | ~$24/mo | Large apps, multiple services |
| **CPX51** | 16 | 32 GB | 360 GB | 20 TB | ~$47/mo | Heavy workloads, large databases |
| **CCX13** | 2 | 8 GB | 80 GB | 20 TB | ~$13/mo | Dedicated vCPU — consistent performance |

**Recommendation logic:**
- Scraper/background worker → **CPX31** (AMD EPYC, burst-friendly)
- Web app with DB → **CX32** or **CPX31** (depends on traffic)
- API-heavy service → **CPX31** (fast single-thread)
- Multiple services → **CPX41** (headroom for containers)
- Budget-constrained → **CX22** (surprisingly capable)

Include: location recommendation (Nuremberg for EU, Ashburn for US).

### Step 3: Initial Server Setup Script

Generate a complete setup script:

```bash
#!/bin/bash
# Hetzner Server Initial Setup
# Run as root on fresh Ubuntu 22.04/24.04 LTS

set -euo pipefail

echo "=== SYSTEM UPDATE ==="
apt update && apt upgrade -y
apt install -y curl wget git htop ufw fail2ban unattended-upgrades \
  apt-listchanges software-properties-common

echo "=== CREATE DEPLOY USER ==="
useradd -m -s /bin/bash deploy
mkdir -p /home/deploy/.ssh
cp /root/.ssh/authorized_keys /home/deploy/.ssh/
chown -R deploy:deploy /home/deploy/.ssh
chmod 700 /home/deploy/.ssh
chmod 600 /home/deploy/.ssh/authorized_keys
usermod -aG sudo deploy

# Set password for sudo (optional, SSH key preferred)
echo "deploy ALL=(ALL) NOPASSWD:ALL" > /etc/sudoers.d/deploy

echo "=== SSH HARDENING ==="
cat > /etc/ssh/sshd_config.d/hardened.conf << 'SSHEOF'
Port 2222
PermitRootLogin no
PasswordAuthentication no
PubkeyAuthentication yes
AuthorizedKeysFile .ssh/authorized_keys
X11Forwarding no
MaxAuthTries 3
ClientAliveInterval 300
ClientAliveCountMax 2
AllowUsers deploy
SSHEOF
systemctl restart sshd

echo "=== FIREWALL (UFW) ==="
ufw default deny incoming
ufw default allow outgoing
ufw allow 2222/tcp comment 'SSH'
ufw allow 80/tcp comment 'HTTP'
ufw allow 443/tcp comment 'HTTPS'
ufw --force enable

echo "=== FAIL2BAN ==="
cat > /etc/fail2ban/jail.local << 'F2BEOF'
[DEFAULT]
bantime = 3600
findtime = 600
maxretry = 3
backend = systemd

[sshd]
enabled = true
port = 2222
filter = sshd
logpath = /var/log/auth.log
maxretry = 3
bantime = 86400
F2BEOF
systemctl enable fail2ban
systemctl restart fail2ban

echo "=== UNATTENDED UPGRADES ==="
cat > /etc/apt/apt.conf.d/20auto-upgrades << 'UUEOF'
APT::Periodic::Update-Package-Lists "1";
APT::Periodic::Unattended-Upgrade "1";
APT::Periodic::AutocleanInterval "7";
UUEOF

echo "=== SWAP (for low-RAM instances) ==="
if [ $(free -m | awk '/^Mem:/{print $2}') -lt 8192 ]; then
  fallocate -l 2G /swapfile
  chmod 600 /swapfile
  mkswap /swapfile
  swapon /swapfile
  echo '/swapfile none swap sw 0 0' >> /etc/fstab
  sysctl vm.swappiness=10
  echo 'vm.swappiness=10' >> /etc/sysctl.conf
fi

echo "=== KERNEL TUNING ==="
cat >> /etc/sysctl.conf << 'KERNEOF'
# Network performance
net.core.somaxconn = 65535
net.ipv4.tcp_max_syn_backlog = 65535
net.ipv4.tcp_tw_reuse = 1
net.ipv4.ip_local_port_range = 1024 65535

# Security
net.ipv4.conf.all.rp_filter = 1
net.ipv4.conf.default.rp_filter = 1
net.ipv4.icmp_echo_ignore_broadcasts = 1
net.ipv4.conf.all.accept_redirects = 0
net.ipv4.conf.default.accept_redirects = 0
net.ipv4.conf.all.send_redirects = 0
KERNEOF
sysctl -p

echo "=== SETUP COMPLETE ==="
echo "SSH port changed to 2222"
echo "Root login disabled"
echo "Deploy user created with SSH key"
echo "Firewall enabled (ports: 2222, 80, 443)"
echo "Fail2ban active"
echo "Unattended upgrades enabled"
```

**IMPORTANT:** After running this script, reconnect using:
```bash
ssh -p 2222 deploy@[server-ip]
```

### Step 4: Application Deployment

**Option A: Docker deployment (recommended):**
```bash
# Install Docker
curl -fsSL https://get.docker.com | sh
usermod -aG docker deploy

# Install Docker Compose
apt install -y docker-compose-plugin

# Create app directory
mkdir -p /opt/app
chown deploy:deploy /opt/app
```

See docker-setup skill for Dockerfile and docker-compose.yml generation.

**Option B: Direct deployment (Node.js example):**
```bash
# Install Node.js via nvm
su - deploy
curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.39.7/install.sh | bash
source ~/.bashrc
nvm install --lts

# Clone and setup
cd /opt/app
git clone [repo-url] .
npm ci --production

# PM2 process manager
npm install -g pm2
pm2 start ecosystem.config.js
pm2 save
pm2 startup
```

**PM2 ecosystem config:**
```javascript
// ecosystem.config.js
module.exports = {
  apps: [{
    name: 'app',
    script: 'dist/index.js',
    instances: 'max',
    exec_mode: 'cluster',
    env: {
      NODE_ENV: 'production',
      PORT: 3000,
    },
    max_memory_restart: '500M',
    error_file: '/var/log/app/error.log',
    out_file: '/var/log/app/out.log',
    merge_logs: true,
  }],
};
```

### Step 5: Reverse Proxy & SSL

**Option A: Caddy (auto-SSL, simplest):**
```bash
apt install -y debian-keyring debian-archive-keyring apt-transport-https
curl -1sLf 'https://dl.cloudsmith.io/public/caddy/stable/gpg.key' | gpg --dearmor -o /usr/share/keyrings/caddy-stable-archive-keyring.gpg
curl -1sLf 'https://dl.cloudsmith.io/public/caddy/stable/debian.deb.txt' | tee /etc/apt/sources.list.d/caddy-stable.list
apt update && apt install -y caddy
```

```
# /etc/caddy/Caddyfile
example.com {
    reverse_proxy localhost:3000
    encode gzip zstd

    header {
        Strict-Transport-Security "max-age=63072000; includeSubDomains; preload"
        X-Content-Type-Options "nosniff"
        X-Frame-Options "SAMEORIGIN"
        Referrer-Policy "strict-origin-when-cross-origin"
    }

    log {
        output file /var/log/caddy/access.log
        format json
    }
}
```

**Option B: Nginx + Certbot:**
```bash
apt install -y nginx certbot python3-certbot-nginx
```

```nginx
# /etc/nginx/sites-available/app
server {
    listen 80;
    server_name example.com;
    return 301 https://$server_name$request_uri;
}

server {
    listen 443 ssl http2;
    server_name example.com;

    ssl_certificate /etc/letsencrypt/live/example.com/fullchain.pem;
    ssl_certificate_key /etc/letsencrypt/live/example.com/privkey.pem;
    ssl_protocols TLSv1.2 TLSv1.3;
    ssl_ciphers HIGH:!aNULL:!MD5;
    ssl_prefer_server_ciphers on;

    # Security headers
    add_header Strict-Transport-Security "max-age=63072000; includeSubDomains; preload" always;
    add_header X-Content-Type-Options "nosniff" always;
    add_header X-Frame-Options "SAMEORIGIN" always;

    location / {
        proxy_pass http://127.0.0.1:3000;
        proxy_http_version 1.1;
        proxy_set_header Upgrade $http_upgrade;
        proxy_set_header Connection 'upgrade';
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header X-Forwarded-Proto $scheme;
        proxy_cache_bypass $http_upgrade;
    }
}
```

```bash
# Get SSL certificate
certbot --nginx -d example.com
# Auto-renewal is configured automatically by certbot
```

### Step 6: Database Setup

**PostgreSQL:**
```bash
apt install -y postgresql postgresql-contrib

sudo -u postgres createuser --interactive appuser
sudo -u postgres createdb appdb -O appuser
sudo -u postgres psql -c "ALTER USER appuser PASSWORD 'CHANGE_ME_STRONG_PASSWORD';"

# Listen only on localhost (default — safe)
# For remote access, use SSH tunnel, never expose port 5432
```

**Redis:**
```bash
apt install -y redis-server

# Secure Redis
sed -i 's/^# requirepass.*/requirepass CHANGE_ME_STRONG_PASSWORD/' /etc/redis/redis.conf
sed -i 's/^bind .*/bind 127.0.0.1 ::1/' /etc/redis/redis.conf
systemctl restart redis-server
```

**Connection security:**
- Databases should only listen on `127.0.0.1`
- Use SSH tunnels for remote database access
- Never expose database ports to the internet
- Use strong, unique passwords stored in environment variables

### Additional Detailed Guidance

Read `references/operations-hardening-and-backups.md` when the task requires monitoring, backup, hardening, and output details. Keep the main workflow concise unless those details are needed.
