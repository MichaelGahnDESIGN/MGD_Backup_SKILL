# Konfiguration — skill.yaml & Setup

## Überblick

MGD-Backup-Skill wird über `skill.yaml` konfiguriert. Du kannst Backup-Verzeichnis, Aufbewahrungsdauer, Kompression, Cloud-Provider und vieles mehr anpassen.

## skill.yaml Struktur

### Standard-Config (nach Installation)

```yaml
backup:
  # Wo Backups gespeichert werden
  backup_dir: ~/.claude/backups/
  
  # Wie lange Backups behalten werden
  retention_days: 30
  
  # Kompressionsformat
  compression: bzip2  # gzip, bzip2, xz
  
  # Logging-Level
  log_level: info     # debug, info, warn, error

cloud:
  # Cloud-Provider (optional)
  provider: aws       # aws, azure, gcp
  enabled: false
  
  # AWS S3 Beispiel
  aws:
    bucket: my-backups
    region: eu-central-1
    # ACCESS_KEY_ID und SECRET_ACCESS_KEY aus ENV laden!

encryption:
  # Verschlüsselung (optional)
  enabled: false
  algorithm: aes-256
  # ENCRYPTION_KEY aus ENV laden!

schedule:
  # Geplante Backups (optional)
  enabled: false
  daily_at: "23:00"   # 23:00 UTC
  weekly_day: sunday
  weekly_at: "02:00"
```

## Einzelne Einstellungen

### `backup_dir`

Wo deine Backups gespeichert werden.

```yaml
# Standard (lokal)
backup_dir: ~/.claude/backups/

# Oder auf externe Festplatte
backup_dir: /mnt/external-drive/backups/

# Oder NAS (nach Montage)
backup_dir: /mnt/nas/backups/
```

### `retention_days`

Alte Backups automatisch löschen nach N Tagen.

```yaml
# Jeden Tag neues Backup, alte nach 7 Tagen weg
retention_days: 7

# Oder längerfristig (z.B. für Archive)
retention_days: 365  # 1 Jahr
```

### `compression`

Dateigröße vs. Geschwindigkeit.

```yaml
# Schnell, mittlere Kompression (empfohlen)
compression: gzip

# Langsamer, bessere Kompression (für große Dateien)
compression: bzip2

# Sehr langsam, beste Kompression
compression: xz
```

## Cloud-Konfiguration

### AWS S3

```yaml
cloud:
  provider: aws
  enabled: true
  
  aws:
    bucket: my-company-backups
    region: eu-central-1
    # Credentials aus Umgebungsvariablen:
    # export AWS_ACCESS_KEY_ID=...
    # export AWS_SECRET_ACCESS_KEY=...
```

### Azure Blob Storage

```yaml
cloud:
  provider: azure
  enabled: true
  
  azure:
    container: my-backups
    storage_account: mystorageaccount
    # Credentials aus Umgebungsvariablen:
    # export AZURE_STORAGE_CONNECTION_STRING=...
```

### GCP Cloud Storage

```yaml
cloud:
  provider: gcp
  enabled: true
  
  gcp:
    bucket: my-company-backups
    project_id: my-gcp-project
    # Credentials aus Umgebungsvariablen:
    # export GOOGLE_APPLICATION_CREDENTIALS=/path/to/key.json
```

## Verschlüsselung (optional)

Zusätzliche Sicherheit für sensitive Backups.

```yaml
encryption:
  enabled: true
  algorithm: aes-256  # AES-256-GCM
  # Schlüssel aus Umgebung:
  # export BACKUP_ENCRYPTION_KEY=your-256-bit-key
```

**Wichtig:** Verschlüsselungs-Keys MÜSSEN sicher gespeichert sein (z.B. in Passwort-Manager, nicht in Git).

## Logging

### Log-Level

```yaml
log_level: debug   # Ausführlich, für Debugging
log_level: info    # Standard, wichtige Ereignisse
log_level: warn    # Nur Warnungen und Fehler
log_level: error   # Nur Fehler
```

### Log-Dateien

Alle Logs werden gespeichert unter:
```
~/.claude/backups/logs/
├── 2026-06-20.log
├── 2026-06-19.log
└── ...
```

## Secrets NIEMALS in skill.yaml!

❌ **NICHT:**
```yaml
# NICHT TUN!
aws:
  access_key: AKIA...
  secret_key: wJalrXU...
```

✅ **STATTDESSEN:**
```yaml
# Credentials aus Umgebungsvariablen:
aws:
  region: eu-central-1
  # Lese AWS_ACCESS_KEY_ID und AWS_SECRET_ACCESS_KEY aus ENV
```

```bash
# Setze Secrets in .bashrc / .zshrc (lokal-only):
export AWS_ACCESS_KEY_ID=AKIA...
export AWS_SECRET_ACCESS_KEY=wJalrXU...
```

## Befehl-spezifische Overrides

Du kannst Standard-Config pro Befehl überschreiben:

```bash
# Dieses Backup zu Cloud, auch wenn cloud.enabled: false
/backup-cloud --provider aws --bucket special-backups

# Höhere Kompression für dieses Backup
/backup-MASTER --compression xz

# Längere Aufbewahrung
/backup-MASTER --retention 90
```

## Vollständiges Beispiel

```yaml
backup:
  backup_dir: ~/backups/
  retention_days: 30
  compression: bzip2
  log_level: info

cloud:
  enabled: true
  provider: aws
  aws:
    bucket: my-backups
    region: eu-central-1

encryption:
  enabled: false  # Optional

schedule:
  enabled: true
  daily_at: "23:00"
  weekly_day: sunday
  weekly_at: "02:00"

# Secrets aus Umgebung!
# export AWS_ACCESS_KEY_ID=...
# export AWS_SECRET_ACCESS_KEY=...
```

---

**[← Zurück zu Wiki](../wiki.md)**
