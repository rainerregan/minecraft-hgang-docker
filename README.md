# HGang Minecraft Server
Server ini menggunakan docker compose untuk memudahkan set up.

## Server Requirements
- CPU: Min 2 Core vCPU
- RAM: Min 4 GB RAM
- Storage: Min 10GB (30GB for more backup files)
- OS: Any OS that can runs Docker

## Cara Set Up
1. Pastikan server VPS sudah ter-install docker
2. Pastikan sudah set-up TCP shield dan terkonek ke CNAME domain. Cek di dashboardnya TCPShield
3. Pastikan backup sudah masuk ke folder `/data` untuk load world yang ada. Jika belum ada world, bisa jalankan langsung docker composenya.
4. Run `docker compose up -d` untuk menjalankan

## Set Up CRON Backup
1. Buat file shell untuk Backup. Pastikan SOURCE_DIR dan BACKUP_DIR sudah sesuai dengan clone git ini.

```bash
nano /usr/local/bin/backup_mc_data.sh
````

```sh
#!/bin/bash

# Set the paths
SOURCE_DIR="/root/apps/minecraft-exacode/data"
BACKUP_DIR="/root/apps/minecraft-exacode/backup_data"
DATE=$(date +\%F_\%H-\%M-\%S)
BACKUP_FILE="$BACKUP_DIR/mc_backup_$DATE.tar.gz"

# Create backup
tar -czf "$BACKUP_FILE" -C "$SOURCE_DIR" .

# Optional: Delete backups older than 7 days
find "$BACKUP_DIR" -type f -name "*.tar.gz" -mtime +7 -delete
```

2. Berikan execute permission ke file shell
```bash
chmod +x /usr/local/bin/backup_mc_data.sh
```

3. Input run file sh ke crontab
```bash
crontab -e
```

Input CRON schedule ke baris paling bawah
```
0 2 * * * /usr/local/bin/backup_mc_data.sh
```

CRON ini akan dijalankan setiap jam 2 pagi.

4. Pastikan CRON berjalan
```bash
grep "backup" /var/log/syslog
```

Jika berjalan, sistem akan log seperti ini:
```bash
Apr 10 02:00:01 hostname CRON[283343]: (root) CMD (/usr/local/bin/backup_mc_data.sh)
```