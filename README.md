# 🗄️ PostgreSQL Backup Automation Script

This repository contains a **PostgreSQL database backup automation script** with support for:
- SSH tunneling (for remote databases)
- Multiple database backups
- Notification alerts (Email, Slack, Telegram)
- Log management and configurable directories

---

## ⚙️ Environment Configuration

All configurations are stored in the `.env` file.  
Below is an example of the environment file:

```bash
# === PostgreSQL Database Config ===
DB_HOST=
DB_PORT=
DB_USER=
DB_PASSWORD=
DB_NAMES=db1,db2,db3

# === Database Type (MYSQL/POSTGRESQL) ===
DB_TYPE=POSTGRESQL

# === Directory Settings ===
LOG_DIR=/data/logs
BACKUP_DIR=/data

# === Alert Methods ===
# Choose one or more: EMAIL, TELEGRAM, SLACK, ALL
ALERT_METHOD=EMAIL,SLACK

# === Email Settings ===
SMTP_FROM_NAME=Backup System
SMTP_FROM=
SMTP_TO=                  # Multiple recipients separated by commas
SMTP_SERVER=
SMTP_PORT=587
SMTP_USER=
SMTP_PASS=
SERVER="Prod Localhost"

# === Telegram Settings ===
TELEGRAM_TOKEN=""
TELEGRAM_CHAT_ID=""

# === Slack Settings ===
SLACK_WEBHOOK_URL=""
# If not set, @channel will be mentioned by default
SLACK_MENTION_ID=

# === SSH Tunnel (Optional) ===
SSH_TUNNEL_ENABLE=true
SSH_TUNNEL_HOST=          # Remote SSH host
SSH_TUNNEL_USER=          # SSH username
SSH_TUNNEL_PASS=          # SSH password
SSH_TUNNEL_PORT=          # SSH port
SSH_TUNNEL_LOCAL=         # Local port for DB forwarding
DB_REMOTE_PORT=           # Remote DB port
```

> 💡 You can copy `.env.example` to `.env` and adjust the values as needed:
```bash
cp .env.example .env
```

---

## 🚀 How to Run the Backup Script

### 1. Run Manually
From your script directory (e.g. `/home/scripts`):
```bash
cd /home/scripts
./backup
```

If the `.env` file is properly configured, the script will:
1. Create a backup for all databases listed in `DB_NAMES`
2. Save the dump to `BACKUP_DIR`
3. Write logs to `LOG_DIR`
4. Send notifications (Email, Slack, Telegram) according to your `ALERT_METHOD`

---

### 2. Schedule via Cron Job (Recommended)

To automate the backup daily at **19:16 (7:16 PM)**, add this cron job:

```bash
16 19 * * * cd /home/scripts && ./backup
```

You can edit your cron jobs using:
```bash
crontab -e
```

---

## 🧾 Logs & Backup Files

- **Logs:** stored in `LOG_DIR` (default: `/data/logs`)
- **Backup files:** stored in `BACKUP_DIR` (default: `/data`)
- Filenames typically include database name and timestamp for easy tracking.

Example:
```
/data/db1_2025-10-29_19-16.sql.gz
/data/logs/backup_2025-10-29.log
```

---

## 🧰 Dependencies

Make sure the server has:
- `bash` or `sh`
- `pg_dump` (for PostgreSQL backups)
- `gzip` (for compression)
- `curl` (for sending Slack/Telegram alerts)
- `mailx` or `sendmail` (for email alerts, optional)

You can install them on Debian/Ubuntu:
```bash
sudo apt update
sudo apt install postgresql-client gzip curl bsd-mailx zstd -y
```

---

## ✅ Example Flow Summary

```
1. Load environment variables from .env
2. Establish SSH tunnel (if enabled)
3. Run pg_dump for each database in DB_NAMES
4. Compress and save backups to BACKUP_DIR
5. Log all actions in LOG_DIR
6. Send alert notification (Email, Slack, Telegram)
```

---

## 📄 License

This project is licensed under the MIT License.

