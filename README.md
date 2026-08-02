# MGD Backup-Skill

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)
[![Status: Active](https://img.shields.io/badge/Status-Active-brightgreen.svg)](#)
[![Language: Deutsch](https://img.shields.io/badge/Language-Deutsch-blue.svg)](#)

Ein universeller, produktionsreifer Backup-Skill für **Claude Code**, **ChatGPT Codex** und andere KI-Assistenten. Sichere lokale Projekte, Docker-Container, Live-Server und Cloud-Systeme mit einer einheitlichen Schnittstelle.

## ✨ Features

- **Vollständige Backup-Verwaltung**: Erstellen, Testen, Verifizieren, Wiederherstellen
- **Mehrere Ziele**: Lokal, SMB/NAS, Docker, Live-Server, Cloud (AWS S3, Azure, etc.)
- **Intelligente Filterung**: Lokal-only-Dateien (Secrets, Datenbank-Dumps, Caches) automatisch ausgeschlossen
- **Sicherheit**: SHA256-Hashing, Integritätsprüfungen, optional verschlüsselt
- **Automatisierung**: Geplante Backups, Snapshots nur für neue Dateien, Rotation
- **Assistenten**: Guided Setup, Restore-Wizard, Cloud-Integration
- **Mehrsprachig**: Deutsch + Englisch (erweiterbar)

## 🚀 Schnelleinstieg

### Installation (lokal)

```bash
# Skill-Verzeichnis klonen
git clone https://github.com/MichaelGahnDESIGN/MGD_Backup_SKILL.git ~/.claude/skills/backup

# Oder manuell in Claude Code:
# Settings → Skills → "MGD-Backup-Skill" → Enable
```

### Erste Schritte

```bash
# 1. Backup-System einrichten
/backup-install

# 2. Erstes Backup erstellen
/backup-MASTER

# 3. Backups anzeigen
/backup-info
```

## 📖 Befehle

| Befehl | Funktion | Schwierigkeit |
|--------|----------|---------------|
| `/backup-install` | Richtet Backup-Ordner und -System ein | Anfänger |
| `/backup-test` | Überprüft alle Backups auf Integrität | Anfänger |
| `/backup-delete` | Löscht alte Backups (mit Aufbewahrung) | Anfänger |
| `/backup-info` | Zeigt alle Backups mit Metadaten | Anfänger |
| `/backup-restore` | Assistent zum Wiederherstellen | Fortgeschritten |
| `/backup-cloud` | Einrichtet Cloud-Backup (AWS S3, Azure, etc.) | Fortgeschritten |
| `/backup-uninstall` | Deinstalliert Backup-System sauber | Anfänger |
| `/backup-MASTER` | Intelligenter Assistent für alle Backup-Vorgänge | Anfänger |
| `/backup-Snapshot` | Sichert nur neue/geänderte Dateien | Fortgeschritten |
| `/backup-Assistent` | Interaktiver Assistent (Menü-gesteuert) | Anfänger |
| `/backup-GODMODE` | Erweiterte Befehle und Entwickler-Tools | Experte |

## 📚 Dokumentation

- **[Anfängerleitfaden](docs/wiki/01-anfaengerleitfaden.md)** — Installation, erste Backup, Restore
- **[Befehlsreferenz](docs/wiki/02-befehlsreferenz.md)** — Alle Befehle im Detail
- **[Konfiguration](docs/wiki/03-konfiguration.md)** — Backup-Quellen, Ziele, Filter einrichten
- **[Cloud-Integration](docs/wiki/04-cloud-integration.md)** — AWS S3, Azure, Google Cloud
- **[Sicherheit & Best Practices](docs/wiki/05-sicherheit.md)** — Encryption, Authentifizierung, Audit
- **[Fehlerbehandlung](docs/wiki/06-fehlerbehandlung.md)** — Häufige Probleme und Lösungen
- **[API & Entwicklung](docs/wiki/07-entwicklung.md)** — Skill erweitern, Custom-Ziele

## 🔒 Sicherheit

Dieser Skill wurde mit **Sicherheit als höchste Priorität** entwickelt:

- ✅ **KEINE sensiblen Daten in GitHub**: Secrets, Keys, Tokens gehören in `.env` (`.gitignore`)
- ✅ **Automatische Filterung**: `.env`, `*.key`, `*.pem`, `*.sql`, Datenbank-Dumps, PlayTest-Artefakte
- ✅ **Integritätsprüfung**: SHA256-Hashing für alle Backups
- ✅ **Verschlüsselung optional**: AES-256 für sensitive Backups
- ✅ **Audit-Logging**: Alle Backup-Aktionen werden protokolliert

**⚠️ Wichtig**: Überprüfe deine Konfiguration in `.claude/backup/config.yaml` — stelle sicher, dass keine Secrets in den Backup-Quellen landen!

## 💡 Beispiele

### Lokales Projekt sichern

```bash
/backup-MASTER
# → Wähle "Lokales Projekt"
# → Wähle Projektverzeichnis
# → Automatischer Backup mit SHA256-Verifikation
```

### Zu SMB/NAS sichern

```bash
/backup-cloud
# → Wähle "SMB/NAS"
# → smb://nas-server/backups eingeben
# → Benutzer/Passwort (wird NICHT gespeichert)
# → Automatisches Backup
```

### Scheduled Backups

```bash
/backup-install
# → Wähle "Geplante Backups einrichten"
# → Zeitplan: täglich 02:00 Uhr
# → Aufbewahrung: 7 Tage
```

## 🛠️ Requirements

- **Claude Code** (v1.0+) oder **ChatGPT Codex**
- **Bash/Zsh** (für lokale Backups)
- **curl** oder **aws-cli** (für Cloud-Backups, optional)
- **tar/gzip/bzip2** (für Kompression)

## 📋 Lizenz

MIT License — siehe [LICENSE](LICENSE)

## 🤝 Beitrag

Bugs melden oder Features vorschlagen? Öffne ein [Issue](https://github.com/MichaelGahnDESIGN/MGD_Backup_SKILL/issues) oder ein [Pull Request](https://github.com/MichaelGahnDESIGN/MGD_Backup_SKILL/pulls).

## 📞 Support

- **Dokumentation**: [docs/wiki/](docs/wiki/)
- **Beispiele**: [examples/](examples/)
- **Issues**: [GitHub Issues](https://github.com/MichaelGahnDESIGN/MGD_Backup_SKILL/issues)

---

## Verwandte MGD Projekte

| Projekt | Beschreibung |
|---------|-------------|
| [MGD-ProjectClean-Skill](https://github.com/MichaelGahnDESIGN/MGD_ProjectClean_SKILL) | Abschluss- und Aufräum-Workflow |
| [MGD-App-Updater-Skill](https://github.com/MichaelGahnDESIGN/MGD_Software-Updater_SKILL) | Software-Update-Systeme planen und implementieren |
| [MGD-DEV-Skill](https://github.com/MichaelGahnDESIGN/MGD_DEV_SKILL) | Release, Sync, Backup und Wissensdokumentation |
| [MGD-AI-Basic-Projektordner](https://github.com/MichaelGahnDESIGN/MGD_AI-Basic-Projektordner_TOOL) | Projektvorlage für KI-Agenten |

→ Alle öffentlichen Projekte: [github.com/MichaelGahnDESIGN](https://github.com/MichaelGahnDESIGN)

---

## Impressum

Angaben gemäß § 5 DDG — Siehe [`IMPRESSUM.md`](IMPRESSUM.md).
