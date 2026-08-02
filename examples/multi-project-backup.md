# Praxisbeispiel: Mehrere Projekte auf einmal sichern

## Szenario

Du hast 3 Projekte (`web-app`, `api-server`, `docs`) und möchtest sie alle in einem Durchgang sichern.

## 🚀 Mehrfach-Backup

```bash
/backup-MASTER

# Menü:
# → 3. Mehrere Projekte
# → Projekt 1: ~/projects/web-app
# → Projekt 2: ~/projects/api-server
# → Projekt 3: ~/projects/docs

# Das System:
# ✓ Backups alle nacheinander
# ✓ Jedes mit eigenem Archive
# ✓ Gemeinsames Manifest
```

**Ergebnis:**
```
✅ web-app_v1.0_20260620.tar.bz2 (45 MB)
✅ api-server_v2.1_20260620.tar.bz2 (12 MB)
✅ docs_v1.5_20260620.tar.bz2 (3 MB)

Manifest: backup_manifest_20260620.json
├── web-app: SHA256: abcd1234...
├── api-server: SHA256: efgh5678...
└── docs: SHA256: ijkl9012...

Gesamt: 60 MB
```

## 📋 Backup-Liste

```bash
/backup-info

# Ausgabe:
# Projekte: 3
# Größe: 60 MB
# Erstellt: 2026-06-20 14:32

# #  Name                              Size    Type
# 1  web-app_v1.0_20260620             45 MB   web
# 2  api-server_v2.1_20260620          12 MB   backend
# 3  docs_v1.5_20260620                 3 MB   docs
```

## 🔄 Einzelnes Projekt wiederherstellen

```bash
/backup-restore

# Auswahl: api-server_v2.1_20260620
# Zielort: ~/projects/api-server-restored

# ✅ Nur dieses Projekt wird wiederhergestellt
```

## ☁️ Alle zu Cloud sichern

```bash
/backup-cloud

# Auswahl: AWS S3
# Bucket: my-backups
# Ordner: /2026-06-20-full-backup

# Das System:
# ✓ Uploaded alle 3 Archive
# ✓ Verifiziert SHA256
# ✓ Erstellt Cloud-Manifest

# ✅ Alle 3 Projekte zu AWS S3 hochgeladen
```

## 💡 Use-Cases

- **Täglich:** Alle Projekte um 23:00 Uhr sichern
- **Vor Release:** Multi-Backup vor produktivem Deploy
- **Team-Backup:** Mehrere Repos eines Teams archivieren
- **Migration:** Mehrere Apps auf neuen Server migrieren

---

**[← Zurück zur Wiki-Übersicht](https://github.com/MichaelGahnDESIGN/MGD_Backup_SKILL/wiki/Praxisbeispiele)**
