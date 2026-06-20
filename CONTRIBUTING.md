# Beitragen zu MGD Backup-Skill

Danke, dass du einen Beitrag leisten möchtest! 🙏

## 🐛 Bugs melden

1. Öffne ein [Issue](https://github.com/MichaelGahnDESIGN/MGD-Backup-Skill/issues)
2. Beschreibe:
   - Was hast du gemacht?
   - Was ist schiefgelaufen?
   - Betriebssystem & Bash-Version
   - Beispiel-Kommando zum Reproduzieren

## 💡 Features vorschlagen

1. Diskutiere in einem [Issue](https://github.com/MichaelGahnDESIGN/MGD-Backup-Skill/issues)
2. Erkläre:
   - Wofür brauchst du das Feature?
   - Wie würde es funktionieren?
   - Gibt es Alternative?

## 🔨 Code beitragen

1. **Fork & Clone**
   ```bash
   git clone https://github.com/<dein-user>/MGD-Backup-Skill.git
   cd MGD-Backup-Skill
   ```

2. **Branch erstellen**
   ```bash
   git checkout -b feature/deine-feature-idee
   ```

3. **Änderungen machen**
   - Sauber & dokumentiert
   - Deutsches Deutsch (Umlaute ü/ö/ä)
   - Tests schreiben (falls möglich)

4. **Commit mit aussagekräftiger Message**
   ```bash
   git commit -m "feat: Neue Cloud-Integration für XYZ"
   ```

5. **Pull Request öffnen**
   - Beschreibe was du gemacht hast
   - Verlinke relevante Issues

## 📋 Style Guide

- **Sprache**: Deutsch mit sauberen Umlauten
- **Dateiname**: kebab-case (backup-install.md)
- **Variablen**: snake_case (backup_dir, retention_days)
- **Funktionen**: camelCase (backupCreate, restoreProject)
- **Kommentare**: Deutsch, kurz & aussagekräftig
- **Commits**: Imperativ ("add", "fix", "docs"), Präsens

## 🚀 Development Setup

```bash
# Abhängigkeiten (optional, für Entwicklung)
brew install shellcheck    # Bash-Linter
brew install yamllint      # YAML-Linter

# Vor Commit: Validierung
./scripts/validate.sh      # (wenn vorhanden)
```

## 📚 Dokumentation

- **Wiki**: `docs/wiki/` (Markdown)
- **README**: `README.md` (Übersicht)
- **Code-Kommentare**: Deutsch, prägnant

## ⚠️ Sensible Daten

**NIEMALS** in GitHub:
- `.env` Dateien
- API-Keys, Tokens, Secrets
- Persönliche Informationen
- Datenbank-Dumps
- Private SSH-Keys

→ Gehört in `.gitignore`!

## ✅ Bevor du PR öffnest

- [ ] Getestet lokal?
- [ ] Tests grün?
- [ ] Dokumentation aktualisiert?
- [ ] Commits sauber (kein Debugging-Zeug)?
- [ ] Keine Secrets in Commit?
- [ ] gegen `main` mergen?

---

**Fragen?** [GitHub Discussions](https://github.com/MichaelGahnDESIGN/MGD-Backup-Skill/discussions) oder [Issues](https://github.com/MichaelGahnDESIGN/MGD-Backup-Skill/issues).
