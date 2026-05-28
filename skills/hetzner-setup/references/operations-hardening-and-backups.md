# Step 7: Monitoring

**Resource monitoring script:**
```bash
#!/bin/bash
# /opt/scripts/health-check.sh

# Disk usage alert (>85%)
DISK_USAGE=$(df / | awk 'NR==2{print $5}' | sed 's/%//')
if [ "$DISK_USAGE" -gt 85 ]; then
  echo "ALERT: Disk usage at ${DISK_USAGE}%"
  # Send notification (webhook, email, etc.)
  curl -X POST "https://hooks.slack.com/services/XXX" \
    -H 'Content-Type: application/json' \
    -d "{\"text\":\" Disk usage at ${DISK_USAGE}% on $(hostname)\"}"
fi

# Memory usage alert (>90%)
MEM_USAGE=$(free | awk '/Mem:/{printf "%.0f", $3/$2 * 100}')
if [ "$MEM_USAGE" -gt 90 ]; then
  echo "ALERT: Memory usage at ${MEM_USAGE}%"
  curl -X POST "https://hooks.slack.com/services/XXX" \
    -H 'Content-Type: application/json' \
    -d "{\"text\":\" Memory usage at ${MEM_USAGE}% on $(hostname)\"}"
fi

# Check if app is responding
HTTP_CODE=$(curl -s -o /dev/null -w "%{http_code}" http://localhost:3000/health)
if [ "$HTTP_CODE" != "200" ]; then
  echo "ALERT: App health check failed (HTTP $HTTP_CODE)"
  curl -X POST "https://hooks.slack.com/services/XXX" \
    -H 'Content-Type: application/json' \
    -d "{\"text\":\" App health check failed on $(hostname) — HTTP ${HTTP_CODE}\"}"
fi
```

```bash
# Add to crontab: every 5 minutes
crontab -e
*/5 * * * * /opt/scripts/health-check.sh >> /var/log/health-check.log 2>&1
```

**External uptime monitoring:**
- **UptimeRobot** (free tier: 50 monitors, 5-min checks)
- **Better Uptime** / **Hetrixtools** (free alternatives)
- Monitor: HTTPS endpoint, SSL expiry, response time

**Log management:**
```bash
# Logrotate for application logs
cat > /etc/logrotate.d/app << 'LREOF'
/var/log/app/*.log {
    daily
    missingok
    rotate 14
    compress
    delaycompress
    notifempty
    copytruncate
}
LREOF
```

### Step 8: Backup Strategy

**Hetzner snapshots (server-level):**
```bash
# Create snapshot via Hetzner CLI
hcloud server create-image --type snapshot --description "Weekly backup $(date +%F)" [server-id]

# Automate weekly snapshots
# Add to crontab (Sunday 3 AM):
0 3 * * 0 hcloud server create-image --type snapshot --description "auto-$(date +\%F)" [server-id]

# Retention: Keep last 4 snapshots, delete older
# Snapshot cost: ~$0.012/GB/month
```

**Database backups (daily):**
```bash
#!/bin/bash
# /opt/scripts/backup-db.sh

BACKUP_DIR="/opt/backups/db"
RETENTION_DAYS=14
DATE=$(date +%F_%H%M)

mkdir -p "$BACKUP_DIR"

# PostgreSQL
pg_dump -U appuser -h localhost appdb | gzip > "$BACKUP_DIR/appdb_${DATE}.sql.gz"

# Optional: Upload to off-site storage (Hetzner Storage Box or S3)
# rsync -avz "$BACKUP_DIR/" u123456@u123456.your-storagebox.de:backups/

# Cleanup old backups
find "$BACKUP_DIR" -type f -mtime +${RETENTION_DAYS} -delete

echo "Backup complete: appdb_${DATE}.sql.gz"
```

```bash
# Daily at 2 AM
0 2 * * * /opt/scripts/backup-db.sh >> /var/log/backup.log 2>&1
```

**Off-site backup options:**
- **Hetzner Storage Box**: Cheap, same datacenter, rsync/SFTP
- **Hetzner Object Storage**: S3-compatible, good for large files
- **Backblaze B2**: Ultra-cheap cloud storage, S3-compatible
- **BorgBackup**: Deduplication, encryption, efficient for incremental backups

### Step 9: Hardening Checklist

Final security hardening verification:

```
── SERVER HARDENING CHECKLIST ─────────────

SSH:
  [x] Root login disabled
  [x] Password authentication disabled
  [x] SSH port changed from 22
  [x] Key-based auth only
  [x] MaxAuthTries set to 3
  [x] Idle timeout configured

Firewall:
  [x] UFW enabled with deny-by-default
  [x] Only required ports open (SSH, HTTP, HTTPS)
  [x] No database ports exposed

Services:
  [x] Fail2ban active on SSH
  [x] Unattended security upgrades enabled
  [x] Unused services disabled
  [x] No unnecessary packages installed

Application:
  [x] App runs as non-root user (deploy)
  [x] Environment variables for secrets (not in code)
  [x] Database listens on localhost only
  [x] Redis password set, localhost only

Monitoring:
  [x] Disk/memory/CPU alerts configured
  [x] App health check endpoint monitored
  [x] External uptime monitoring active
  [x] Log rotation configured

Backups:
  [x] Database backed up daily
  [x] Server snapshots weekly
  [x] Off-site backup configured
  [x] Backup restoration tested
```

### Step 10: Output

Present the complete server setup:

```
━━━ HETZNER SERVER SETUP ━━━━━━━━━━━━━━━━━
Instance: [type] — [vCPU] vCPU, [RAM] GB RAM
Location: [datacenter]
OS: Ubuntu [version] LTS
Cost: ~$[X]/mo

── PROVISIONING SCRIPT ────────────────────
[complete setup script]

── APPLICATION DEPLOYMENT ─────────────────
Method: [Docker / Direct]
Process manager: [Docker / PM2]
Config: [ecosystem.config.js or docker-compose.yml]

── REVERSE PROXY ──────────────────────────
Server: [Caddy / Nginx]
SSL: Let's Encrypt (auto-renewal)
Config: [Caddyfile or nginx.conf]

── DATABASE ───────────────────────────────
[PostgreSQL/Redis setup]

── MONITORING ─────────────────────────────
Internal: [health check script + cron]
External: [uptime monitoring service]
Alerts: [notification channel]

── BACKUPS ────────────────────────────────
Database: Daily, [retention] days
Snapshots: Weekly, last [N] kept
Off-site: [storage solution]

── HARDENING ──────────────────────────────
[checklist with status]

── DEPLOYMENT COMMANDS ────────────────────
[quick reference for common operations]
```

## Inputs
- Workload type and expected traffic
- Application stack (language, framework)
- Database requirements
- Domain name
- Budget preference

## Outputs
- Instance type recommendation with justification
- Complete server provisioning script (SSH hardening, firewall, fail2ban, unattended upgrades)
- Application deployment (Docker or direct with PM2)
- Reverse proxy config (Caddy or Nginx) with auto-SSL
- Database setup (PostgreSQL, Redis) with security
- Monitoring script with disk/memory/app health alerts
- Backup strategy (snapshots + database dumps + off-site)
- Server hardening checklist
- Deployment quick-reference commands
