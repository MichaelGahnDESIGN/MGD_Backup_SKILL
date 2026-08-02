# Praxisbeispiel: Flutter-App sichern & wiederherstellen

## Szenario

Du hast eine Flutter-App (5 GB Rohdaten) und möchtest sie sichern — mit Quellcode, Assets, Konfiguration.

## 🚀 Vollständiger Ablauf

### 1. System einrichten (einmalig)

```bash
/backup-install

# Antworten:
# Backup-Verzeichnis: ~/.claude/backups/
# Aufbewahrung: 30 Tage
# Kompression: bzip2
# Logging: info
```

### 2. Erstes Vollbackup

```bash
/backup-MASTER

# Menü:
# → 1. Einzelnes Projekt
# → Ziel: ~/projects/my-flutter-app
```

**Live-Anzeige:**
```
📦 Backup wird erstellt...

Quelle: ~/projects/my-flutter-app (5.2 GB)

Filter:
  ✓ lib/, assets/, pubspec.yaml
  ✓ docs/, README.md
  ✗ .git/ (zu groß, nutze Git für History)
  ✗ .dart_tool/ (wird bei flutter pub get rekonstruiert)
  ✗ build/ (flutter build web erstellt neu)
  ✗ .env, *.key (Secrets-Filterung)
  ✗ PlayTest/ (Test-Artefakte)
  ✗ *.sql, *.dump (Datenbank-Dumps)

Berechnet: 420 MB (unkomprimiert)
Kompression: bzip2
Geschätzte Größe: 48 MB
Geschätzte Zeit: 90 Sekunden

[████████████████████] 100%
Verpacken... ✓
Komprimieren... ✓
SHA256 erzeugen... ✓

✅ Backup erstellt!

📄 my-flutter-app_v1.0.0_20260620.tar.bz2
💾 Größe: 48 MB
📍 Speicher: ~/.claude/backups/
🔐 SHA256: dd1794bebc49...
⏱️  Zeit: 92 Sekunden
📝 Git-Commit: 6fb4970f
```

### 3. Verifikation

```bash
/backup-test

# Ausgabe:
# ✅ my-flutter-app_v1.0.0_20260620
#    Size: 48 MB ✓
#    SHA256: dd1794bebc... ✓
#    Structure: tar.bz2 ✓
#    Readable: ✓
```

### 4. Backups anzeigen

```bash
/backup-info

# Ausgabe:
# Gesamt: 1 Backup (48 MB)
#
# #  Name                              Size    Date        SHA256
# 1  my-flutter-app_v1.0.0_20260620    48 MB  2026-06-20  dd1794bebc...
```

## 📅 Tägliche Snapshots (optional)

Nach dem Vollbackup könnten täglich **Snapshots** nur neue Dateien sichern:

```bash
/backup-Snapshot

# Output:
# Basis-Backup: my-flutter-app_v1.0.0_20260620
# Neue Dateien: 5
# Snapshot-Größe: 200 KB (99% kleiner!)
#
# ✅ snapshot_20260621_001.tar.bz2 erstellt
```

## 🔄 Restore (Szenario: Projekt gelöscht / Sicherung einspielen)

```bash
/backup-restore

# Auswahl: my-flutter-app_v1.0.0_20260620

# Zielort: ~/projects/my-flutter-app-restored

# Das System:
# ✓ SHA256 prüfen
# ✓ Entpacken (48 MB → 420 MB)
# ✓ Git klonen (Commit 6fb4970)
# ✓ Flutter Dependencies installieren
#   - flutter pub get (~2 Min)
# ✓ Weitere Dependencies (falls nötig)

# ✅ Restore abgeschlossen!
# 📁 Projekt: ~/projects/my-flutter-app-restored
# ✓ Alle Dateien wiederhergestellt
# ✓ Git synchronisiert
# ✓ Dependencies installiert
```

Das Projekt ist **sofort einsatzbereit** ohne manuelle Schritte!

## ☁️ Zu Cloud sichern (optional)

```bash
/backup-cloud

# Auswahl: AWS S3

# S3-Bucket: my-backups
# Region: eu-central-1

# Das System:
# ✓ Authentifiziert sich mit AWS-Credentials
# ✓ Lädt Archiv hoch
# ✓ Verifiziert SHA256 auf dem Server
# ✓ Erstellt Retention-Policy

# ✅ Zu Cloud hochgeladen!
```

## 📋 Checkliste für Flutter-Projekte

- ✅ `.env`, `*.key` ausgeschlossen? (Geheimnisse sicher)
- ✅ `.dart_tool/` ausgeschlossen? (wird rekonstruiert)
- ✅ `build/` ausgeschlossen? (wird neu gebaut)
- ✅ `*.sql`, Datenbank-Dumps ausgeschlossen?
- ✅ Git-Commit in Metadaten gespeichert? (für Restore)
- ✅ Kompression bzip2? (gut für Flutter-Größen)
- ✅ SHA256 gespeichert? (für Verifikation)

## 🎯 Häufige Flutter-Fragen

**F: Warum ist das Backup nur 48 MB, obwohl die App 5 GB ist?**  
A: `.dart_tool/` (2 GB), `build/` (2 GB), und andere temporäre Dateien werden ausgeschlossen. Das spart 99% Speicher, weil diese Verzeichnisse bei `flutter pub get` und `flutter build` automatisch rekonstruiert werden.

**F: Muss ich manuell `flutter pub get` ausführen?**  
A: Nein! `/backup-restore` erkennt automatisch, dass es ein Flutter-Projekt ist, und führt `flutter pub get` aus.

**F: Kann ich nur einzelne Dateien aus dem Backup wiederherstellen?**  
A: In der Basis-Version nein. Mit `/backup-GODMODE` möglich (fortgeschritten).

**F: Wie oft sollte ich Backups erstellen?**  
A: Täglich (Snapshot) + Wöchentlich (Vollbackup). Mit geplanten Backups automatisierbar.

---

**[← Zurück zur Wiki-Übersicht](https://github.com/MichaelGahnDESIGN/MGD_Backup_SKILL/wiki/Praxisbeispiele)**
