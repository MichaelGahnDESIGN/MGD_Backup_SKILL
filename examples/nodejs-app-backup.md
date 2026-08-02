# Praxisbeispiel: Node.js App sichern

## Szenario

Du hast eine Node.js/Express-App mit npm-Dependencies und möchtest sie schnell sichern — ohne `node_modules` (werden aus `package.json` rekonstruiert).

## 🚀 Schneller Ablauf

### 1. Backup erstellen

```bash
/backup-MASTER

# → Einzelnes Projekt
# → ~/projects/my-api-server
```

**Ergebnis:**
```
✅ my-api-server_v2.1.0_20260620.tar.bz2
💾 Größe: 3.2 MB (node_modules ausgeschlossen!)
```

### 2. Automatische Aufräumung (optional)

Nach 7 Tagen alte Backups löschen:

```bash
/backup-delete

# Automatisches Löschen von Backups älter als 7 Tage
# → Spart Speicher
```

## 🔄 Restore

```bash
/backup-restore

# Auswahl: my-api-server_v2.1.0_20260620
# Zielort: ~/projects/my-api-server-restored

# Das System:
# ✓ Entpackt
# ✓ npm install (aus package.json, ~1 Min)
# ✓ .env-Datei aus Secrets-Speicher (falls vorhanden)

# ✅ Server bereit zum Start!
# npm start
```

## 📋 Best Practices für Node.js

- ✅ `node_modules/` wird automatisch ausgeschlossen
- ✅ `.env` wird NICHT gesichert (Geheimnisse sicher)
- ✅ `.git/` wird ausgeschlossen (nutze GitHub für Code-History)
- ✅ `dist/` oder `build/` wird ausgeschlossen (wird durch Build rekonstruiert)
- ✅ Logs werden ausgeschlossen

## 🎯 Größenvergleich

```
Vor Filterung:  350 MB (mit node_modules)
Nach Filterung:  3.2 MB (nur Code)
Komprimiert:     2.1 MB (bzip2)
```

---

**[← Zurück zur Wiki-Übersicht](https://github.com/MichaelGahnDESIGN/MGD_Backup_SKILL/wiki/Praxisbeispiele)**
