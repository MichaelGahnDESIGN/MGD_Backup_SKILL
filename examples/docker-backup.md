# Praxisbeispiel: Docker-Container sichern

## Szenario

Du hast einen Postgres-Container und eine Node.js-App in Docker und brauchst ein Backup.

## 🚀 Ablauf

### 1. Docker-Verzeichnis sichern

```bash
/backup-MASTER

# → Docker-Container
# Erkennt automatisch docker-compose.yml / Dockerfiles

# Sichert:
# ✓ docker-compose.yml
# ✓ Dockerfiles
# ✓ .env-Dateien (⚠️ durch Secrets-Filter geschützt)
# ✓ Volumes (falls lokal)
# ✗ laufende Container-Daten (brauchen Pause)
```

### 2. Laufenden Container pausieren & Dump holen

```bash
# MANUELL:
docker-compose pause postgres
docker exec postgres_container pg_dump -U user mydb > db.sql
docker-compose unpause postgres

# Lokal sichern:
/backup-MASTER
# → Backup erstellen (mit sql-Dump)
```

### 3. Restore (neuen Server aufsetzen)

```bash
/backup-restore

# → Container-Backup auswählen
# → Zielverzeichnis
# Das System:
# ✓ docker-compose.yml wiederherstellen
# ✓ Docker Images pullen
# ✓ docker-compose up starten
# ✓ Datenbank-Dump einspielen

# ✅ Container läuft!
```

---

**[← Zurück zu Beispiele](../examples.md)**
