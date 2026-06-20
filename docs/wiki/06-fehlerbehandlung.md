# Fehlerbehandlung & FAQs

## Häufige Fehler

### ❌ "Permission denied" beim Backup-Erstellen

**Problem:**
```
Error: Permission denied: /root/.claude/backups/
```

**Ursache:** Backup-Verzeichnis existiert nicht oder Nutzer hat keine Schreibrechte.

**Lösung:**
```bash
# 1. Verzeichnis erstellen
mkdir -p ~/.claude/backups/

# 2. Schreibrechte prüfen
chmod 700 ~/.claude/backups/

# 3. Neu versuchen
/backup-MASTER
```

---

### ❌ "Disk space full" während Backup

**Problem:**
```
Error: No space left on device
Backup hat nur 2 GB Platz benötigt, aber noch 5 GB Quelle
```

**Ursache:** Zielverzeichnis hat nicht genug freier Speicher.

**Lösung:**
```bash
# 1. Freien Speicher prüfen
df -h ~/.claude/backups/

# 2. Alte Backups löschen
/backup-delete --days 7

# 3. Oder zu anderem Verzeichnis
/backup-MASTER --backup-dir /mnt/external-drive/backups/
```

---

### ❌ "Corrupt backup file" bei Restore

**Problem:**
```
Error: SHA256 mismatch
Expected: 3a7f8c2e...
Got:      9d4b1f6a...
```

**Ursache:** Backup-Datei ist beschädigt oder wurde manipuliert.

**Lösung:**
```bash
# 1. Backup neu testen
/backup-test my-app_v1.0_20260620.tar.bz2

# 2. Falls kaputt: Backup löschen
/backup-delete my-app_v1.0_20260620

# 3. Neues Backup erstellen
/backup-MASTER
```

---

### ❌ "Cloud upload failed" bei AWS S3

**Problem:**
```
Error: InvalidAccessKeyId
The AWS Access Key Id you provided does not exist
```

**Ursache:** AWS-Credentials sind falsch oder abgelaufen.

**Lösung:**
```bash
# 1. Credentials prüfen
echo $AWS_ACCESS_KEY_ID

# 2. Falls leer: Credentials setzen
export AWS_ACCESS_KEY_ID=AKIA...
export AWS_SECRET_ACCESS_KEY=wJalr...

# 3. Test
/backup-cloud --dry-run

# 4. Neu versuchen
/backup-cloud
```

---

### ❌ "Encryption key not found"

**Problem:**
```
Error: Encryption key not found at ~/.claude/backups/.encryption-key
```

**Ursache:** Verschlüsselung ist aktiv, aber Schlüssel fehlt.

**Lösung:**
```bash
# 1. Schlüssel hat Backup von dir?
# → Falls ja: Datei wiederherstellen
cp ~/secure-storage/backup-key.txt ~/.claude/backups/.encryption-key

# 2. Falls nein: Verschlüsselung deaktivieren
# (Alte Backups sind dann nicht mehr lesbar!)
/backup-install --encryption no
```

---

## ❓ Häufige Fragen

### F: Wie groß sollten Backups sein?

**A:** Hängt vom Projekt ab:

```
Frontend (React/Vue):     5–50 MB
Backend (Node/Django):    10–100 MB
Flutter-App:              30–150 MB
Datenbank (JSON export): 100 MB – 1 GB
```

Wenn Backup ungewöhnlich groß ist:
```bash
/backup-test --verbose
# Zeigt: "build/" (zu groß?), "node_modules/" (sollte ausgeschlossen sein)
```

---

### F: Kann ich selective Restore machen? (nur bestimmte Dateien)

**A:** Basis-Version: Nein (ganzes Backup oder nichts).

Mit `/backup-GODMODE`:
```bash
/backup-GODMODE extract my-app_v1.0_20260620 lib/
# Extrahiert nur lib/-Verzeichnis
```

---

### F: Wie oft sollte ich Backups machen?

**A:** Abhängig von Änderungsrate:

```
Aktive Entwicklung:  täglich (Snapshot)
Produktiv-Code:      wöchentlich (Vollbackup)
Kurze Prototypen:    bei Milestone
```

Mit Automatisierung:
```yaml
schedule:
  enabled: true
  daily_at: "23:00"      # Täglich 23 Uhr
  weekly_day: sunday
  weekly_at: "02:00"     # Sonntag 02 Uhr
```

---

### F: Sind Backups kompatibel zwischen Betriebssystemen?

**A:** Ja, aber mit Einschränkungen:

```
✅ Linux → Linux: vollständig kompatibel
✅ macOS → macOS: vollständig kompatibel
✅ Linux ↔ macOS: kompatibel (außer xattrs)
⚠️  Windows: Pfad-Probleme (\ vs /)
```

---

### F: Kann ich Backups verschlüsseln, wenn sie schon unverschlüsselt sind?

**A:** Nein direkt, aber:

```bash
# 1. Alte Backups umbenennen/archivieren
/backup-delete my-app_v1.0_20260620

# 2. Verschlüsselung aktivieren
/backup-install --encryption yes

# 3. Neue Backups sind verschlüsselt
/backup-MASTER
```

---

### F: Was ist der Unterschied zwischen Backup und Snapshot?

**A:**

```
Vollbackup (/backup-MASTER):
  → Komplettes Projekt
  → ~50 MB
  → Nutzbar für Restore

Snapshot (/backup-Snapshot):
  → Nur neue/geänderte Dateien seit letztem Vollbackup
  → ~2 MB
  → Muss mit Vollbackup kombiniert werden für Restore
```

Strategie:
```
Montag:  Vollbackup (50 MB)
Di-So:   Täglich Snapshot (2 MB pro Tag)
Nächste Woche: Neues Vollbackup
```

---

### F: Kann ich Backups von GitHub clonen statt Backups zu nutzen?

**A:** Kommt drauf an:

```
Quellcode:    Nutze GitHub Clone (git clone)
Assets/Doku:  Nutze Backup
Projektstruktur: Nutze Backup
Dependencies: Automatisch rekonstruiert (npm install, etc.)
```

Hybrid-Ansatz:
```bash
# Schnell & klein
git clone https://github.com/user/my-app.git

# Dann Dependencies
cd my-app && npm install

# Das ist oft besser als Backup für reinen Code
```

---

## 🔧 Debug-Mode

Wenn etwas nicht funktioniert:

```bash
# Verbose Output
/backup-MASTER --verbose

# Debug-Logging
/backup-MASTER --log-level debug

# Test-Modus (nichts ändern)
/backup-MASTER --dry-run
```

Logs prüfen:
```bash
tail -n 50 ~/.claude/backups/logs/2026-06-20.log
```

---

## 📞 Support

Wenn dein Problem hier nicht gelöst ist:

1. **Prüfe die Logs:**
   ```bash
   tail -f ~/.claude/backups/logs/*.log
   ```

2. **GitHub Issues:** [MGD-Backup-Skill Issues](https://github.com/MichaelGahnDESIGN/MGD-Backup-Skill/issues)

3. **E-Mail:** security@example.com (für Sicherheitsprobleme)

---

**[← Zurück zu Wiki](../wiki.md)**
