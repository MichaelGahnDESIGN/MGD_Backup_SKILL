# Praxisbeispiel: Python/Django Project sichern

## Szenario

Django-App mit Datenbank — Quellcode separat von Produktiv-Daten sichern.

## 🚀 Zwei-Schritt Strategie

### Schritt 1: Code sichern (regelmäßig)

```bash
/backup-MASTER

# → Einzelnes Projekt
# → ~/projects/my-django-app
```

**Ergebnis:**
```
✅ my-django-app_v3.0_20260620.tar.bz2
💾 Größe: 8.5 MB
Ausschließt: venv/, __pycache__/, *.pyc, .env
```

### Schritt 2: Datenbank sichern (separat, manuell)

```bash
# Lokal:
python manage.py dumpdata > db_backup.json

# Oder PostgreSQL:
pg_dump mydb > db_backup.sql

# Dann manuell zu Backups-Ordner:
/backup-MASTER
# → db_backup.json oder db_backup.sql
```

## 🔄 Restore

```bash
# 1. Code restore
/backup-restore
# → my-django-app_v3.0_20260620
# → ~/projects/my-django-app-restored

# 2. Dependencies installieren
cd ~/projects/my-django-app-restored
python -m venv venv
source venv/bin/activate
pip install -r requirements.txt

# 3. Datenbank einspielen (manuell)
python manage.py loaddata db_backup.json
# oder
psql mydb < db_backup.sql

# ✅ App läuft!
python manage.py runserver
```

## 📋 Best Practices

- ✅ `venv/` oder `.venv/` wird ausgeschlossen (wird rekonstruiert)
- ✅ `__pycache__/`, `*.pyc` werden ausgeschlossen
- ✅ `.env` wird NICHT gesichert (Geheimnisse!)
- ✅ Datenbank **separat** sichern (nicht ins Code-Backup)
- ✅ `requirements.txt` IS im Backup (für pip install)

## 🎯 Größenvergleich

```
Code (ohne venv): 8.5 MB → komprimiert: 2.1 MB
Datenbank (JSON): 450 MB → komprimiert (gz): 45 MB
```

---

**[← Zurück zu Beispiele](../examples.md)**
