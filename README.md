# MGD Backup-Skill

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)
[![Status: Active](https://img.shields.io/badge/Status-Active-brightgreen.svg)](#)
[![Language: Deutsch](https://img.shields.io/badge/Language-Deutsch-blue.svg)](#)

Ein Skill für **Claude Code**, **ChatGPT Codex** und andere KI-Assistenten, der lokale Projekte, Docker-Container, Live-Server und Cloud-Systeme über eine einheitliche Sammlung von Slash-Befehlen sichert.

## Das Problem

KI-Agenten arbeiten oft an vielen Projekten gleichzeitig, ohne dass es einen einheitlichen Weg gibt, sie zu sichern. Jedes Projekt bekommt eigene Ad-hoc-Skripte, niemand prüft automatisch, ob ein Backup tatsächlich wiederherstellbar ist, und Secrets (`.env`, Keys, Datenbank-Dumps) landen leicht versehentlich in der Sicherung. Der MGD Backup-Skill definiert dafür elf Slash-Befehle mit automatischer Secrets-Filterung und SHA256-Integritätsprüfung.

## Installation

> [!NOTE]
> **👤 FÜR ENTWICKLER** — Das Repository ist kein fertiges Programm, sondern eine Skill-Definition: `skill.yaml` beschreibt Befehle, Konfigurationsschema und Sicherheitsregeln, die Markdown-Dateien liefern die Anleitungen dazu. Es gibt keine Shell-Skripte im Repo — ein KI-Assistent führt die eigentlichen Bash-Befehle (`tar`, `curl`, `git` …) zur Laufzeit aus, gesteuert durch diese Dokumente.
>
> ```bash
> # Skill-Verzeichnis klonen
> git clone https://github.com/MichaelGahnDESIGN/MGD_Backup_SKILL.git ~/.claude/skills/backup
>
> # Oder manuell in Claude Code:
> # Settings → Skills → "MGD-Backup-Skill" → Enable
> ```

## Erste Schritte

> [!TIP]
> **🤖 FÜR KI-AGENTEN** — Einstieg für einen neuen Nutzer: zuerst `/backup-install` (einmalig, richtet Verzeichnisse und Konfiguration ein), danach `/backup-MASTER` (Haupt-Assistent für alle Backup-Aufgaben) oder `/backup-info` zur Übersicht bestehender Backups. Nutze ausschließlich die elf in `skill.yaml` definierten Befehle.
>
> ```bash
> # 1. Backup-System einrichten
> /backup-install
>
> # 2. Erstes Backup erstellen
> /backup-MASTER
>
> # 3. Backups anzeigen
> /backup-info
> ```

> [!WARNING]
> **⚠️ FALLSTRICK** — Ein schnellerer Befehl `/backup-quick` wird in `docs/wiki/01-anfaengerleitfaden.md` als Alternativweg angedeutet, existiert aber nicht — die Datei kommentiert ihn selbst als „noch nicht implementiert in dieser Basis-Version". Er taucht auch nicht in `skill.yaml` auf. Für einen Erstnutzer bleibt `/backup-MASTER` der einzige dokumentierte Weg zum ersten Backup.

## Die wichtigsten Befehle

| Befehl | Funktion | Schwierigkeit |
|--------|----------|---------------|
| `/backup-install` | Richtet Backup-Ordner und -System ein | Anfänger |
| `/backup-test` | Überprüft alle Backups auf Integrität | Anfänger |
| `/backup-delete` | Löscht alte Backups (mit Aufbewahrung) | Anfänger |
| `/backup-info` | Zeigt alle Backups mit Metadaten | Anfänger |
| `/backup-uninstall` | Deinstalliert Backup-System sauber | Anfänger |
| `/backup-MASTER` | Intelligenter Assistent für alle Backup-Vorgänge | Anfänger |
| `/backup-restore` | Assistent zum Wiederherstellen | Fortgeschritten |
| `/backup-cloud` | Richtet Cloud-Backup ein (AWS S3, Azure, Google Cloud) | Fortgeschritten |
| `/backup-Snapshot` | Sichert nur neue/geänderte Dateien | Fortgeschritten |
| `/backup-Assistent` | Interaktiver Assistent (Menü-gesteuert) | Fortgeschritten |
| `/backup-GODMODE` | Erweiterte Befehle und Entwickler-Tools (`dangerous: true`) | Experte |

Automatisch ausgeschlossen werden laut `skill.yaml` (`security.secret_filtering`): `*.env*`/`.env*`, `*.key`, `*.pem`, `*.sql`, `PlayTest*`.

> [!WARNING]
> **⚠️ FALLSTRICK** — Verschlüsselung ist **nicht** der Standard: `config_schema.encryption` in `skill.yaml` hat den Typ `boolean` mit Default `false`. Ein frisch installiertes System (`/backup-install` mit Standardwerten) erzeugt also unverschlüsselte Backups, auch wenn README und `SECURITY.md` Sicherheit als „höchste Priorität" bewerben. AES-256 muss aktiv über `/backup-install` (Frage „Verschlüsselung aktivieren?") oder in der Konfiguration eingeschaltet werden.

## Grenzen

- Keine ausführbaren Skripte im Repo: ohne einen KI-Agenten, der die Befehle in echte Bash-Aufrufe übersetzt, passiert nichts von allein.
- Verschlüsselung ist standardmäßig deaktiviert (siehe Fallstrick oben) — für sensible Daten muss sie aktiv eingeschaltet werden.
- Selektiver Restore einzelner Dateien ist in der Basisversion nicht vorgesehen; nur über den als `dangerous: true` markierten Befehl `/backup-GODMODE`.
- Native Datenbank-Backups (PostgreSQL/MySQL) sind laut `CHANGELOG.md` erst für v1.1+ geplant; aktuell müssen Datenbank-Dumps manuell erstellt werden (z. B. `pg_dump`, `mysqldump`).
- Wasabi, DigitalOcean Spaces und Minio tauchen im interaktiven Menü von `/backup-cloud` auf, sind aber unter `integrations:` in `skill.yaml` nicht als eigene Integration deklariert — nur AWS S3, Azure Blob Storage und Google Cloud Storage.
- Getestet auf macOS, Linux und Docker; unter Windows kann es laut Dokumentation zu Pfad-Problemen (`\` vs. `/`) kommen.

## Wiki

| Seite | Inhalt |
|---|---|
| [Home](https://github.com/MichaelGahnDESIGN/MGD_Backup_SKILL/wiki) | Überblick, alle elf Befehle, Fallstricke |
| [Schnellstart](https://github.com/MichaelGahnDESIGN/MGD_Backup_SKILL/wiki/Schnellstart) | Installation und erstes Backup |
| [Befehlsreferenz](https://github.com/MichaelGahnDESIGN/MGD_Backup_SKILL/wiki/Befehlsreferenz) | Alle elf Befehle im Detail |
| [Konfiguration](https://github.com/MichaelGahnDESIGN/MGD_Backup_SKILL/wiki/Konfiguration) | `skill.yaml`-Konfigurationsschema |
| [Cloud-Integration](https://github.com/MichaelGahnDESIGN/MGD_Backup_SKILL/wiki/Cloud-Integration) | AWS S3, Azure, Google Cloud |
| [Sicherheit](https://github.com/MichaelGahnDESIGN/MGD_Backup_SKILL/wiki/Sicherheit) | Secrets-Filterung, Verschlüsselung, Meldeweg |
| [Praxisbeispiele](https://github.com/MichaelGahnDESIGN/MGD_Backup_SKILL/wiki/Praxisbeispiele) | Flutter, Node.js, Django, Website, Docker, Multi-Projekt |
| [Fehlerbehebung](https://github.com/MichaelGahnDESIGN/MGD_Backup_SKILL/wiki/Fehlerbehebung) | Häufige Fehler und FAQ |
| [Mitwirken](https://github.com/MichaelGahnDESIGN/MGD_Backup_SKILL/wiki/Mitwirken) | Bugs melden, Beiträge einreichen, Style-Guide |

## Verwandte MGD Projekte

| Projekt | Beschreibung |
|---------|-------------|
| [MGD_ProjectClean_SKILL](https://github.com/MichaelGahnDESIGN/MGD_ProjectClean_SKILL) | Abschluss- und Aufräum-Workflow |
| [MGD_Software-Updater_SKILL](https://github.com/MichaelGahnDESIGN/MGD_Software-Updater_SKILL) | Software-Update-Systeme planen und implementieren |
| [MGD_DEV_SKILL](https://github.com/MichaelGahnDESIGN/MGD_DEV_SKILL) | Release, Sync, Backup und Wissensdokumentation |
| [MGD_AI-Basic-Projektordner_TOOL](https://github.com/MichaelGahnDESIGN/MGD_AI-Basic-Projektordner_TOOL) | Projektvorlage für KI-Agenten |

→ Alle öffentlichen Projekte: [github.com/MichaelGahnDESIGN](https://github.com/MichaelGahnDESIGN)

## Lizenz

MIT License — siehe [LICENSE](LICENSE)

---

## Impressum

Angaben gemäß § 5 DDG — Siehe [`IMPRESSUM.md`](IMPRESSUM.md).
