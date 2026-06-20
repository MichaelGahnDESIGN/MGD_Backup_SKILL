# Praxisbeispiel: Website (PHP/Laravel) sichern

## Szenario

Website auf einem Live-Server. Code + Datenbank-Dumps gehören zu verschiedenen Backup-Orten.

## 🚀 Ablauf

### 1. Lokal: Quellcode sichern

```bash
/backup-MASTER

# → Einzelnes Projekt
# → /Users/michaelgahn/Projects/my-website

# Ergebnis:
# ✅ my-website_v2.5_20260620.tar.bz2 (156 MB)
#    (beinhaltet: PHP-Code, Laravel-Config, Assets)
```

### 2. Live-Server: Datenbank-Dump sichern (manuell lokal)

```bash
# SSH auf Live-Server und Dump holen (MANUELL!):
ssh user@production.com "mysqldump myapp > /tmp/db.sql"
scp user@production.com:/tmp/db.sql ~/backups/myapp_db_20260620.sql

# WICHTIG: Secrets aus Dump entfernen!
/backup-godmode filter-secrets myapp_db_20260620.sql
```

### 3. Cloud-Backup (optional)

```bash
/backup-cloud

# → AWS S3
# → Bucket: my-backups
# Beide Dateien automatisch hochgeladen
```

## 📋 Wichtig für Live-Server

- ✗ `.env` NICHT sichern (Secrets!)
- ✗ API-Keys nicht in Quellcode
- ✓ Datenbank-Dumps separat & verschlüsselt
- ✓ User-Uploads separat sichern (falls groß)

---

**[← Zurück zu Beispiele](../examples.md)**
