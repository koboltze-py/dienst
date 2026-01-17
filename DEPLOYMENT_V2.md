# Deployment-Anleitung - Version 2.0

## 🚀 Änderungen auf GitHub pushen

### 1. Status prüfen
```powershell
git status
```

### 2. Alle Änderungen hinzufügen
```powershell
git add .
```

### 3. Commit erstellen
```powershell
git commit -m "Version 2.0: Kalender-Interface, neue Schichttypen (T/T10/N10), gebündelte Admin-Ansicht, verbesserter PDF-Export"
```

### 4. Zu GitHub pushen
```powershell
git push origin main
```

## 📦 Änderungen in dieser Version

### Neue Dateien:
- `templates/shift_request_form_new.html` - Kalender-Interface für Benutzer
- `templates/admin_dashboard_new.html` - Gebündelte Admin-Ansicht
- `migrate_shift_types.py` - Migrations-Skript für Schichttypen
- `README_V2.md` - Aktualisierte Dokumentation
- `DEPLOYMENT_V2.md` - Diese Datei

### Geänderte Dateien:
- `app.py`:
  - Route `/` nutzt jetzt `shift_request_form_new.html`
  - Route `/admin` nutzt jetzt `admin_dashboard_new.html`
  - `GET /api/shift-requests` filtert jetzt nach **Folgemonat** statt aktuellem Monat
  - `export_pdf()` erstellt jetzt Kalender-Layout statt Liste
  - Neue Imports für Kalender-Funktionen

### Migration (bereits ausgeführt):
- ✅ Schichttypen konvertiert: Nacht → N10
- Database bleibt kompatibel

## 🔍 Test-Checkliste vor Deployment

### Lokal testen:
- [ ] Server startet ohne Fehler
- [ ] Login funktioniert
- [ ] Kalender zeigt Folgemonat
- [ ] Klicken auf Tage rotiert durch T → T10 → N10
- [ ] Speichern funktioniert
- [ ] Admin-Dashboard zeigt gebündelte Ansicht
- [ ] Bestätigen direkt bei Diensten funktioniert
- [ ] PDF-Export erstellt Kalender-Layout
- [ ] Excel-Export funktioniert weiterhin
- [ ] Nachrichten-System funktioniert

### Nach GitHub-Push:
- [ ] Repository aktualisiert
- [ ] Render erkennt neuen Commit
- [ ] Auto-Deployment startet
- [ ] Build erfolgreich (ca. 2-3 Minuten)
- [ ] Service deployed

### Nach Render-Deployment:
- [ ] Website erreichbar unter render.com URL
- [ ] Login funktioniert
- [ ] Admin-Zugang funktioniert
- [ ] Neue Benutzer können sich registrieren
- [ ] Kalender-Interface funktioniert
- [ ] PDF-Export funktioniert
- [ ] Datenbank-Verbindung OK (PostgreSQL)

## ⚠️ Wichtige Hinweise

### Datenbank-Migration auf Render:
Da Render PostgreSQL nutzt, müssen Sie die Migration **nicht** manuell ausführen.
Die neuen Schichttypen (T, T10, N10) werden automatisch akzeptiert.

Falls alte Daten vorhanden sind:
```bash
# Über Render Shell
python migrate_shift_types.py
```

### Environment Variables (Render):
```
DATABASE_URL=postgresql://... (automatisch gesetzt)
```

### requirements.txt:
Alle Dependencies sind bereits korrekt:
- Flask==3.0.0
- Flask-CORS==4.0.0
- Flask-SQLAlchemy==3.1.1
- gunicorn==21.2.0
- psycopg2-binary==2.9.9
- openpyxl==3.1.5
- reportlab==4.2.5
- Pillow==10.4.0

## 🔄 Rollback-Plan

Falls Probleme auftreten:

### 1. Zu vorheriger Version zurück:
```powershell
git log --oneline  # Finde letzte gute Version
git revert HEAD    # Macht letzten Commit rückgängig
git push origin main
```

### 2. Alte Templates wiederherstellen:
```powershell
git checkout HEAD~1 -- templates/shift_request_form.html
git checkout HEAD~1 -- templates/admin_dashboard.html
# Dann in app.py die Template-Namen zurückändern
```

### 3. Render-Logs prüfen:
```
Dashboard → Logs → Deployment Logs
Dashboard → Logs → Service Logs
```

## 📊 Monitoring nach Deployment

### Zu überwachen:
1. **Response Times**: Sollten < 1s bleiben
2. **Error Rate**: Sollte 0% sein
3. **Database Connections**: Sollten stabil bleiben
4. **Memory Usage**: Sollte < 500MB bleiben

### Render Dashboard:
- **Deployments**: Prüfen Sie, ob Build erfolgreich
- **Logs**: Schauen Sie nach Fehlern
- **Metrics**: CPU, Memory, Network

## 🎯 Erfolgs-Kriterien

Das Deployment ist erfolgreich, wenn:
- ✅ Website erreichbar
- ✅ Login funktioniert
- ✅ Kalender lädt Folgemonat
- ✅ Schichten können ausgewählt werden
- ✅ Speichern funktioniert
- ✅ Admin kann Dienste bestätigen
- ✅ PDF-Export mit Kalender-Layout funktioniert
- ✅ Keine Fehler in Logs

## 📞 Support

Bei Problemen:
1. Prüfen Sie Render-Logs
2. Testen Sie lokal
3. Rollback falls kritisch
4. Kontaktieren Sie Support bei persistenten Problemen

---

**Version**: 2.0  
**Datum**: 17. Januar 2026  
**Autor**: GitHub Copilot  
**Status**: Ready for Deployment
