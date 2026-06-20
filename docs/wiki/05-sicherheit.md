# Sicherheit & Best Practices

## 🔒 Automatische Sicherheitsmaßnahmen

MGD-Backup-Skill implementiert mehrere Schutzmaßnahmen automatisch:

### 1. Secrets-Filterung

Diese Dateitypen werden IMMER ausgeschlossen:

```
.env, .env.local, .env.staging, .env.production
*.key, *.pem, id_rsa*, id_ed25519*
*.sql, *.sql.gz, *.dump
credentials.*, secrets.*, .aws/credentials
```

**Beispiel:**
```bash
# Das wird NICHT gesichert:
.env (enthält API_KEY=...)
~/.ssh/id_rsa (privater SSH-Key)
database_backup.sql (Produktiv-Daten)

# Das WIRD gesichert:
.env.example (Template, keine echten Secrets)
pubspec.yaml, package.json (Abhängigkeiten)
src/, app/, lib/ (Quellcode)
```

### 2. SHA256 Integrität

Jedes Backup bekommt einen SHA256-Hash:

```bash
my-app_v1.0_20260620.tar.bz2
SHA256: 3a7f8c2e9b4d1f6a5c8e2b9d7f1a4c6e...

# Bei Restore wird dieser überprüft:
✓ SHA256 matches original
```

Verhindert:
- ✅ Beschädigte Dateien (Festplatte-Fehler)
- ✅ Manipulation (jemand ändert Backup)
- ✅ Unvollständige Übertragungen (zu Cloud)

### 3. Audit-Logging

Alle Aktionen werden protokolliert:

```
~/.claude/backups/logs/2026-06-20.log:

[2026-06-20 10:30:45 INFO] Backup started: my-app
[2026-06-20 10:30:47 INFO] Excluded: .env (secrets)
[2026-06-20 10:30:50 INFO] Excluded: node_modules/ (reconstructible)
[2026-06-20 10:31:22 INFO] Backup finished: 48 MB
[2026-06-20 10:31:23 INFO] SHA256: 3a7f8c2e9b...
```

Logs enthalten **KEINE Secrets** (sind gefiltert).

## 🔐 Verschlüsselung (Optional)

Für zusätzliche Sicherheit: AES-256-Verschlüsselung aktivieren.

### Setup

```bash
/backup-install

# → Verschlüsselung aktivieren? ja
# → Verschlüsselungsschlüssel generieren

# Schlüssel wird gespeichert in:
~/.claude/backups/.encryption-key
```

### Verschlüsselte Backups

```bash
# Mit Verschlüsselung aktiv:
my-app_v1.0_20260620.tar.bz2.enc

# Ohne Verschlüsselung:
my-app_v1.0_20260620.tar.bz2
```

### Schlüssel-Sicherheit

⚠️ **KRITISCH:** Der Verschlüsselungsschlüssel ist dein **einziger** Zugriff auf verschlüsselte Backups.

```bash
# Schlüssel sichern (in Passwort-Manager oder externem Speicher)
cp ~/.claude/backups/.encryption-key ~/secure-storage/backup-key.txt

# Wenn Schlüssel verloren → Backups sind NICHT wiederherstellbar!
```

## 🌐 Cloud-Sicherheit

### AWS S3

```yaml
# Best Practice: IAM-Policy mit minimalem Zugriff
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": [
        "s3:PutObject",
        "s3:GetObject",
        "s3:ListBucket"
      ],
      "Resource": "arn:aws:s3:::my-backups/*"
    }
  ]
}
```

### Credentials-Rotation

```bash
# Alte AWS-Keys rotieren (monatlich):
# 1. Neue Keys im AWS Console erstellen
# 2. Lokal aktualisieren:
export AWS_ACCESS_KEY_ID=AKIA_NEW...
export AWS_SECRET_ACCESS_KEY=wJalr_NEW...

# 3. Alte Keys im AWS Console löschen
```

## 🛡️ Datenschutz (DSGVO/CCPA)

Wenn deine Backups **personenbezogene Daten** enthalten (z.B. User-Datenbanken):

### Rechte des Nutzers

```
Recht auf Vergessenwerden:
→ Backup mit Nutzerdaten muss gelöscht werden
→ Verwende: /backup-delete [backup-name]
```

### Backup-Verschlüsselung (Datenschutz)

```bash
/backup-install
# → Verschlüsselung: ja
# → Backups sind datenschutzkonform
```

### Backup-Standort

```
✅ In deinem Land (z.B. EU, für DSGVO)
✗ Nicht in Ländern mit unsicheren Datenschutz-Gesetzen
```

Beispiel:
```yaml
# DSGVO-konform: Backup zu deutschem Server
backup_dir: /mnt/german-nas/backups/

# Oder: AWS EU (Frankfurt)
aws:
  region: eu-central-1  # Frankfurt, Deutschland
```

## 🚨 Sicherheitsproblem gefunden?

Falls du ein Sicherheitsproblem entdeckst:

1. **NICHT öffentlich posten** (nicht als GitHub Issue)
2. **Sende private E-Mail** an: `security@example.com`
3. **Beschreibe genau:**
   - Was ist das Problem?
   - Wie kann man es reproduzieren?
   - Welche Auswirkung hat es?

Wir antworten innerhalb von **48 Stunden**.

## ✅ Sicherheits-Checkliste

- [ ] Backups haben Zugriffschutz (Filesystem-Permissions)?
- [ ] Cloud-Credentials sind rotiert (monatlich)?
- [ ] Verschlüsselungsschlüssel ist extern gesichert?
- [ ] Logs zeigen keine Secrets?
- [ ] Alte Backups werden gelöscht (Retention-Policy)?
- [ ] SHA256 wird regelmäßig überprüft?

---

**[← Zurück zu Wiki](../wiki.md)**
