# Anfängerleitfaden — MGD Backup-Skill

Willkommen! Dieser Leitfaden führt dich durch die ersten Schritte mit dem Backup-Skill.

## 📋 Voraussetzungen

- Claude Code oder ChatGPT Codex
- Bash/Zsh (macOS/Linux) oder PowerShell (Windows)
- Mindestens 100 MB freier Speicherplatz für Backups

## 🚀 Installation

### Schritt 1: Skill aktivieren

```bash
# Claude Code CLI
claude config add-skill https://github.com/MichaelGahnDESIGN/MGD-Backup-Skill

# Oder manuell:
# Settings → Skills → "MGD-Backup-Skill" → Enable
```

### Schritt 2: System einrichten

```bash
/backup-install
```

Folge den Fragen:
1. **Backup-Verzeichnis**: Wo sollen Backups gespeichert sein? (Default: `~/.claude/backups`)
2. **Aufbewahrung**: Wie lange Backups aufbewahren? (Default: 30 Tage)
3. **Kompression**: Welcher Algorithmus? (Default: bzip2 — klein + schnell)

✅ Das System ist jetzt bereit!

## 💾 Erstes Backup erstellen

### Option A: Mit dem Assistenten (empfohlen)

```bash
/backup-MASTER
```

1. Wähle: **"Backup erstellen"**
2. Wähle: **"Lokales Projekt"**
3. Wähle dein Projektverzeichnis (z.B. `~/Projects/my-app`)
4. Das System:
   - Zeigt eine Vorschau (welche Dateien, Größe)
   - Fragt nach Bestätigung
   - Erstellt das Backup mit SHA256-Verifikation
   - Zeigt den finalen Status

Das war's! Dein erstes Backup ist erstellt.

### Option B: Direkt (wenn du weißt, was du tust)

```bash
# Noch nicht implementiert in dieser Basis-Version
# In zukünftigen Versionen möglich:
# /backup-quick /path/to/project
```

## 📊 Backups ansehen

```bash
/backup-info
```

Zeigt:
```
Backup-Name          | Größe  | Datum      | SHA256                                | Projekt
─────────────────────────────────────────────────────────────────────────────────
my-app_v1.0_20260620 | 48 MB  | 2026-06-20 | dd1794bebc... | ~/Projects/my-app
my-site_v0.5_20260619 | 156 MB | 2026-06-19 | a7f42af0...  | ~/Projects/my-site
```

## ✅ Backup testen

```bash
/backup-test
```

Validiert alle Backups auf:
- ✓ SHA256-Integrität
- ✓ Archiv-Struktur
- ✓ Lesbarkeit

Falls Fehler gefunden: **Backup ist beschädigt!** Lösche es und erstelle ein neues.

## 🔄 Backup wiederherstellen

### Szenario: Ich habe mein Projekt gelöscht und möchte es zurückholen

```bash
/backup-restore
```

1. Wähle das Backup aus der Liste
2. Gib den Zielort ein (z.B. `~/Projects/my-app-restored`)
3. Das System:
   - Entpackt das Archiv
   - Klont Git-Repository (falls vorhanden)
   - Installiert Dependencies (flutter pub, npm, composer, etc.)
   - Zeigt den Status

Das wiederhergestellte Projekt ist bereit!

## 🗑️ Alte Backups löschen

```bash
/backup-delete
```

1. Das System zeigt dir alle Backups
2. Fragt: **"Wie viele Backups möchtest du behalten?"** (z.B. 5)
3. Löscht automatisch die ältesten (mit Bestätigung)

**Beispiel:**
```
Du hast 8 Backups. Du möchtest 5 behalten.
Die 3 ältesten werden gelöscht:
  ✗ my-app_v0.1_20260601
  ✗ my-app_v0.2_20260605
  ✗ my-app_v0.3_20260610

Fortfahren? (j/n) j
✅ Erledigt!
```

## 🛑 System entfernen

Wenn du den Skill nicht mehr brauchst:

```bash
/backup-uninstall
```

Das entfernt:
- ✗ Konfigurationsdateien
- ✗ Skill selbst (optional: auch deine Backups)

## 🎯 Typische Szenarien

### Szenario 1: Täglicherweise Backups

```bash
/backup-MASTER
# → "Automatische Backups einrichten"
# → "Täglich um 02:00 Uhr"
# → "30 alte Backups automatisch löschen"
```

Das System erstellt automatisch Backups und löscht alte.

### Szenario 2: Vor großer Änderung

```bash
/backup-Snapshot
# → Erstellt schnelles inkrementelles Backup
# → Nur geänderte Dateien seit letztem Backup
```

Schneller und kleiner als volles Backup.

### Szenario 3: Mehrere Projekte sichern

```bash
/backup-MASTER
# Wähle: "Mehrere Projekte"
# Gib alle Pfade an
# Das System sichert der Reihe nach
```

## 🔧 Häufige Fragen

**F: Wo werden meine Backups gespeichert?**  
A: Standardmäßig in `~/.claude/backups/`. Im `setup` kannst du es ändern.

**F: Sind meine Secrets (API-Keys, Passwörter) in den Backups?**  
A: **NEIN!** Der Skill filtert automatisch `.env`, `*.key`, `*.pem` und andere sensible Dateien aus.

**F: Kann ich Backups zu meinem NAS kopieren?**  
A: Ja! Mit `/backup-cloud` oder manuell nach Abschluss des Backups.

**F: Wie groß werden meine Backups?**  
A: Abhängig vom Projekt. Beispiel: Ein Flutter-Projekt mit 500 MB wird zu ~50 MB (bzip2-Kompression).

**F: Kann ich einzelne Dateien aus einem Backup wiederherstellen?**  
A: In der Basis-Version: Nein. Mit `/backup-GODMODE` möglich.

## 📚 Nächste Schritte

- Lies die [Befehlsreferenz](02-befehlsreferenz.md) für alle Details
- Erkunde [Cloud-Integration](04-cloud-integration.md) für Remote-Backups
- Schau [Sicherheit & Best Practices](05-sicherheit.md) für Verschlüsselung

---

**Probleme?** Öffne ein [Issue auf GitHub](https://github.com/MichaelGahnDESIGN/MGD-Backup-Skill/issues).
