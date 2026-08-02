# Sicherheitsrichtlinien — MGD Backup-Skill

Dieses Projekt behandelt sensible Daten. **Sicherheit ist nicht verhandelbar.**

## 🔒 Sicherheits-Versprechen

Dieser Skill wurde mit folgenden Prinzipien entwickelt:

1. **Keine Secrets in GitHub** — `.env`, API-Keys, Tokens gehören NICHT ins Repository
2. **Automatische Filterung** — Sensible Dateitypen werden automatisch ausgeschlossen
3. **Integritätsprüfung** — Alle Backups sind SHA256-gehasht und verifizierbar
4. **Optionale Verschlüsselung** — AES-256-Encryption optional (nicht erzwungen, um Performance zu bewahren)
5. **Audit-Logging** — Alle Backup-Aktionen werden protokolliert (lokal, NICHT an Dritte)

## 📋 Sicherheits-Checkliste

### Automatisch gefiltert (immer ausgeschlossen)

Diese Dateien werden **IMMER** aus Backups ausgeschlossen:

```yaml
# Environment & Secrets
- .env
- .env.local
- .env.*

# Schlüssel & Zertifikate
- '*.key'
- '*.pem'
- id_rsa*

# Datenbanken (produk­tiv sensibel)
- '*.sql'
- '*.sql.gz'
- '*.dump'

# Entwicklungs-Artefakte (lokal-only)
- .dart_tool/
- build/
- node_modules/
- vendor/
- __pycache__/

# Test-Daten (lokal-only)
- PlayTest*
- test-output/
- test-backups/
```

### Richtlinien beim Backup-Erstellen

**Nachdem du ein Backup mit `/backup-MASTER` erstellst:**

✅ **DO:**
- Verwende nur Quellcode ohne `.env`-Dateien
- Nutze `git clone` für Repository-History (sauberer als Backups)
- Sichern Du Datenbanken separat + verschlüsselt
- Überprüfe mit `/backup-test`, dass alles OK ist

❌ **DON'T:**
- Keine Datenbank-Dumps mit echten Benutzerdaten
- Keine API-Keys in Quellcode
- Keine SSH-Keys oder `.pem`-Dateien
- Keine Build-Verzeichnisse (zu groß, rekonstruierbar)

## 🔐 Verschlüsselung

Für zusätzliche Sicherheit: AES-256-Encryption aktivieren

```bash
/backup-install
# → Encryption aktivieren? ja

# Alle zukünftigen Backups sind verschlüsselt:
# my-app_v1.0_20260620.tar.bz2.enc
```

**Wichtig:** Encryption-Key MUSS an sicherem Ort gespeichert sein (nicht in GitHub!).

## 📊 Audit-Log

Alle Backup-Aktionen werden protokolliert:

```
~/.claude/backup/logs/
├── 2026-06-20.log
│   ├── 10:30:45 — Backup erstellt: my-app_v1.0_20260620 (48 MB)
│   ├── 10:35:20 — Backup getestet: OK (SHA256 ✓)
│   └── 11:00:00 — Zu Cloud hochgeladen: AWS S3 ✓
└── 2026-06-19.log
```

**Logs enthalten KEINE Secrets** (`.env`-Inhalte etc. werden gefiltert).

## 🚨 Sicherheitsproblem gefunden?

Falls du ein Sicherheitsproblem entdeckst (z.B. Secrets werden nicht gefiltert):

1. **NICHT öffentlich posten** — [Sende eine private E-Mail](mailto:michaelgahndesign@gmail.com)
2. Beschreibe das Problem detailliert
3. Gib ein Reproduktions-Szenario an
4. Wir werden schnell reagieren

## ✅ Sicherheits-Audit

Dieses Projekt wurde geprüft auf:

- ✅ Keine hardcodierten Secrets in Quellcode
- ✅ Keine `.env`-Dateien im Git
- ✅ `.gitignore` deckt alle sensiblen Dateitypen ab
- ✅ Keine großen Dateien (die auf Dump-Daten hindeuten)
- ✅ Saubere Dokumentation (keine Beispiel-Secrets)

## 📚 Weiterführend

- [CONTRIBUTING.md](CONTRIBUTING.md) — Code-Beiträge & Style Guide
- [Sicherheit & Best Practices](docs/wiki/05-sicherheit.md) — Detaillierte Sicherheits-Anleitung

---

**Noch Fragen zur Sicherheit?** [GitHub Issues](https://github.com/MichaelGahnDESIGN/MGD_Backup_SKILL/issues) oder [E-Mail](mailto:michaelgahndesign@gmail.com).
