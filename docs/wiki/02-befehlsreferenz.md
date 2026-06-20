# Befehlsreferenz — MGD Backup-Skill

Detaillierte Dokumentation aller 11 Befehle.

---

## 🟢 Anfänger-Befehle

### `/backup-install`

**Zweck:** Initialisiert das Backup-System mit Standardkonfiguration.

**Interaktive Schritte:**
1. Backup-Verzeichnis festlegen (default: `~/.claude/backups/`)
2. Aufbewahrungsdauer in Tagen (default: 30)
3. Kompression wählen (gzip, bzip2, xz — default: bzip2)
4. Logging aktivieren? (default: info)

**Ausgabe:**
```
✅ Backup-System initialisiert
📍 Verzeichnis: ~/.claude/backups/
⏰ Aufbewahrung: 30 Tage
🗜️  Kompression: bzip2
📝 Logging: info-Stufe

Nächster Schritt: /backup-MASTER
```

**Dateien, die erstellt werden:**
- `~/.claude/backup/config.yaml` — Konfiguration
- `~/.claude/backup/metadata.json` — Backup-Registry
- `~/.claude/backup/logs/` — Logging-Verzeichnis
- Backup-Zielverzeichnis

**Wiederholung:** `/backup-install` kann jederzeit erneut ausgeführt werden (überschreibt nicht).

---

### `/backup-info`

**Zweck:** Zeigt alle Backups mit Metadaten an.

**Optionen:**
```bash
/backup-info                    # Alle Backups
/backup-info --sort-size        # Größte zuerst
/backup-info --sort-date        # Älteste zuerst
/backup-info --filter flutter   # Nur Flutter-Projekte
```

**Ausgabe:**
```
Gesamt: 5 Backups (312 MB)

#  Name                          Size    Date        SHA256                  Project
1  flutter-app_v1.0_20260620    48 MB   2026-06-20  dd1794bebc...         ~/Projects/flutter-app
2  website_v2.5_20260619        156 MB  2026-06-19  a7f42af0...           ~/Projects/my-site
3  laravel-api_v3.0_20260618    92 MB   2026-06-18  5cff8f9d...           ~/Projects/api
4  backup-skill_v1.0_20260615   16 MB   2026-06-15  e74c6b3f...           ~/GitHub-Skills/MGD-Backup-Skill
5  old-project_v0.1_20260601    9 MB    2026-06-01  3fb950a2...           ~/Projects/archived
```

**Doppelklick-Restore:** `#1` eingeben für schnellen Restore.

---

### `/backup-test`

**Zweck:** Validiert alle Backups auf Integrität.

**Prüft:**
- ✓ SHA256-Hashsum (Dateiintegrität)
- ✓ Archiv-Struktur (tar/gzip/bzip2/xz)
- ✓ Lesbarkeit (kann entpackt werden?)
- ✓ Metadaten-Konsistenz
- ✓ Größenabweichungen (> 20% Größenänderung?)

**Ausgabe:**
```
🧪 Teste Backups...

✅ flutter-app_v1.0_20260620
   Size: 48 MB ✓
   SHA256: dd1794bebc... ✓
   Structure: tar.bz2 ✓
   Readable: ✓

⚠️  website_v2.5_20260619
   Size: 156 MB (⚠️ +18% seit letztem Backup)
   SHA256: a7f42af0... ✓
   Structure: tar.bz2 ✓
   Readable: ✓

❌ old-project_v0.1_20260601
   SHA256 MISMATCH! Backup beschädigt.
   Erwartet: 3fb950a2...
   Erhalten: 2c5a3f7b...
   → EMPFEHLUNG: Backup löschen, neu erstellen

Zusammenfassung:
  ✅ 4 Backups OK
  ⚠️  1 Backup mit Warnung
  ❌ 1 Backup beschädigt
```

---

### `/backup-delete`

**Zweck:** Löscht alte Backups mit intelligenter Rotation.

**Prozess:**
1. Zeigt alle Backups mit Größe/Datum
2. Fragt: "Wie viele Backups möchtest du behalten?"
3. Löscht die ältesten (mit Bestätigung)

**Beispiel:**
```
Du hast 5 Backups (312 MB).
Wie viele möchtest du behalten? 3

Zu löschende Backups:
  ✗ backup-skill_v1.0_20260615 (16 MB) — 5 Tage alt
  ✗ old-project_v0.1_20260601 (9 MB) — 19 Tage alt

Gesamt zu löschen: 25 MB
Nach Löschung: 3 Backups (287 MB)

Fortfahren? (j/n) j

✅ Erledigt!
  ✓ 2 Backups gelöscht
  ✓ Metadaten aktualisiert
```

**Sicherheit:** Doppelte Bestätigung für Sicherheit.

---

### `/backup-uninstall`

**Zweck:** Deinstalliert das Backup-System sauber.

**Ablauf:**
```
⚠️  WARNUNG: Backup-System wird deinstalliert.

Was soll gelöscht werden?
  1. Nur Konfiguration (~/.claude/backup/)
  2. Konfiguration + Backups (~/.claude/backups/)
  3. Alles einschließlich Logs

Auswahl: 1

Diese Aktion kann NICHT rückgängig gemacht werden.
Fortfahren? (j/n) j

✅ Deinstallation abgeschlossen.
  ✓ Konfiguration gelöscht
  ✓ Skill deaktiviert
```

**Tipp:** Backups separat sichern bevor gelöscht!

---

## 🟡 Fortgeschrittene Befehle

### `/backup-restore`

**Zweck:** Guided Restore mit Abhängigkeiten-Installation.

**Schritte:**
1. Backup auswählen (Liste mit Größe/Datum/SHA256)
2. Zielort eingeben
3. Git rekonstruieren? (ja/nein)
4. Dependencies installieren? (flutter pub, npm, composer, etc.)
5. Verifikation vor Restore

**Beispiel:**
```
Welches Backup möchtest du wiederherstellen?

#  Name                          Size    Date
1  flutter-app_v1.0_20260620    48 MB   2026-06-20
2  website_v2.5_20260619        156 MB  2026-06-19

Auswahl: 1

Zielverzeichnis? ~/Projects/flutter-app-restored

Wiederherstellen:
  ✓ SHA256 prüfen
  ✓ Entpacken
  ✓ Git-Repository klonen (shallow)
  ✓ flutter pub get
  ✓ iOS/Android vorbereiten

✅ Restore abgeschlossen!
   Projekt: ~/Projects/flutter-app-restored
   Commit: 6fb4970
```

**Ziele:** Lokal, Docker, Remote-Server (SSH).

---

### `/backup-cloud`

**Zweck:** Einrichtet Cloud-Backup zu AWS S3, Azure, Google Cloud, etc.

**Provider-Auswahl:**
```
Welcher Cloud-Provider?
  1. AWS S3
  2. Azure Blob Storage
  3. Google Cloud Storage
  4. Wasabi (S3-kompatibel)
  5. DigitalOcean Spaces
  6. Minio (Self-Hosted)

Auswahl: 1

AWS S3 konfigurieren:

AWS_ACCESS_KEY: [eingeben, nicht gespeichert]
AWS_SECRET_KEY: [eingeben, nicht gespeichert]
S3_BUCKET: my-backups
S3_REGION: eu-central-1
ENCRYPTION: AES-256? (j/n) j

Test-Upload...
✅ Erfolgreich!
   Datei: test-backup_20260620.tar.bz2.enc
   Größe: 5.2 MB
   Verschlüsselt: ✓
```

**Automatisierung:** Kann geplant werden (`/backup-MASTER` → "Geplante Backups").

---

### `/backup-Snapshot`

**Zweck:** Sichert nur neue/geänderte Dateien seit letztem Backup.

**Wie es funktioniert:**
1. Vergleicht mit letztem Backup (Änderungsdatum)
2. Sichert nur neue/geänderte Dateien
3. Speichert Referenz zum Basis-Backup
4. Bei Restore: Basis + alle Snapshots zusammenbauen

**Beispiel:**
```
Letztes Backup: flutter-app_v1.0_20260620 (48 MB)
Heute geändert: 23 Dateien

Snapshot-Größe: ~2 MB (95% kleiner als Vollbackup!)
Zeit: ~5 Sekunden

✅ Snapshot erstellt!
   flutter-app_snapshot_20260621_001.tar.bz2
   Abhängigkeit: flutter-app_v1.0_20260620
```

**Ideal für:** Tägliche/stündliche Backups.

---

## 🔴 Assistenten & Experten

### `/backup-MASTER`

**Zweck:** Haupt-Assistent für alle Backup-Aufgaben.

**Hauptmenü:**
```
🎯 Hauptmenü — Was möchtest du tun?

  BACKUP ERSTELLEN
    1. Einzelnes Projekt
    2. Mehrere Projekte
    3. Docker-Container
    4. Geplante Backups einrichten

  BACKUP VERWALTEN
    5. Backups ansehen (/backup-info)
    6. Backups testen (/backup-test)
    7. Alte Backups löschen (/backup-delete)

  WIEDERHERSTELLEN
    8. Restore durchführen
    9. Zu Cloud wiederherstellen

  EINSTELLUNGEN
    10. System konfigurieren
    11. Cloud-Provider hinzufügen

Auswahl (1-11): 1
```

Spricht Nutzer-Sprache, keine Terminals-Befehle nötig.

---

### `/backup-Assistent`

**Zweck:** Wie `/backup-MASTER`, aber mit erweiterten Optionen.

Zusätzliche Menü-Items:
- Snapshot-Zeitpläne konfigurieren
- Encryption aktivieren/deaktivieren
- Backup-Filter anpassen (z.B. welche Dateien ausschließen)
- Cloud-Verbindung testen
- Backup-Reports erstellen

---

### `/backup-GODMODE`

**Zweck:** Entwickler/Experten-Tools.

**Warnung:**
```
⚠️  GODMODE aktiviert!

Du brauchst Expertenwissen für diese Befehle.
Ohne Wissen können Backups beschädigt werden!

Verfügbare Befehle:
```

**Befehle:**
```bash
# Tar-Optionen direkt steuern
/backup-godmode tar --exclude="*.log" --exclude="node_modules" /path/to/project

# Rohes Scripting (Cron/Systemd)
/backup-godmode schedule-cron "0 2 * * *" /backup-Snapshot

# Custom-Filter schreiben
/backup-godmode create-filter my-flutter-filter
   [editable YAML mit Regeln]

# Encryption/Decryption manuell
/backup-godmode encrypt my-backup.tar.bz2 --key-file my-key.pem
/backup-godmode decrypt my-backup.tar.bz2.enc --key-file my-key.pem

# Einzelne Dateien extrahieren
/backup-godmode extract flutter-app_v1.0_20260620.tar.bz2 \
   PROJEKT/WORKSPACE/app/lib/main.dart

# Metadaten editieren (gefährlich!)
/backup-godmode edit-metadata backup-id-123

# Backup-Repair (versucht beschädigte Backups zu reparieren)
/backup-godmode repair old-project_v0.1_20260601.tar.bz2
```

**KEINE Sicherheit:** Nutzer trägt volle Verantwortung!

---

## 📊 Befehl-Vergleich

| Befehl | Schwierigkeit | Zeit | Größe | Auto? |
|--------|--------------|------|-------|-------|
| `/backup-MASTER` | Anfänger | 5 min | Normal | Möglich |
| `/backup-install` | Anfänger | 2 min | — | — |
| `/backup-info` | Anfänger | 10 sec | — | — |
| `/backup-test` | Anfänger | 1 min | — | — |
| `/backup-delete` | Anfänger | 30 sec | — | — |
| `/backup-restore` | Fortgeschr. | 10 min | Normal | — |
| `/backup-cloud` | Fortgeschr. | 15 min | Normal | Möglich |
| `/backup-Snapshot` | Fortgeschr. | 2 min | Klein | Möglich |
| `/backup-Assistent` | Fortgeschr. | 10 min | — | — |
| `/backup-GODMODE` | Experte | Variabel | Variabel | Nein |
| `/backup-uninstall` | Anfänger | 5 sec | — | — |

---

**[← Zurück zu Anfängerleitfaden](01-anfaengerleitfaden.md) | [Weiter zu Konfiguration →](03-konfiguration.md)**
