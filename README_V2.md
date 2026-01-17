# DRK Köln - Dienstwünsche System 📋

**Version 2.0** - Komplett überarbeitetes System mit Kalender-Interface

## 🆕 Neue Features (Januar 2026)

### 1. **Kalender-Interface** 📅
- **Monatsansicht** für den Folgemonat
- Klicken Sie auf einen Tag, um zwischen Schichttypen zu rotieren:
  - `T` → `T10` → `N10` → (leer) → `T` → ...
- Farbcodierung:
  - 🟡 **Gelb**: T (Tagdienst)
  - 🔵 **Blau**: T10 (Tagdienst 10h)
  - 🟣 **Indigo**: N10 (Nachtdienst 10h)
- Grüner Rahmen: Bestätigte Dienste

### 2. **Neue Schichttypen**
- **T**: Tagdienst (Standard)
- **T10**: Tagdienst 10 Stunden
- **N10**: Nachtdienst 10 Stunden

_(Alte Typen: Früh, Spät, Nacht wurden automatisch migriert)_

### 3. **Admin-Dashboard - Gebündelte Ansicht** 👥
- Dienstwünsche nach **Mitarbeiter gruppiert**
- Bestätigungs-Button **direkt neben jedem Dienst**
- Übersichtliche Darstellung mit:
  - Wochentag-Anzeige (Mo, Di, Mi, ...)
  - Datum formatiert (TT.MM.JJJJ)
  - Schichttyp als farbiger Tag
  - Bemerkungen falls vorhanden

### 4. **Verbesserter PDF-Export** 📄
- **Kalender-Layout** wie im Muster-PDF
- Zeigt alle Mitarbeiter als Zeilen
- Tage des Monats als Spalten
- Farbcodierung der Schichten
- Bestätigungs-Marker (✓)
- Legende für Schichttypen
- Automatischer Seitenumbruch bei vielen Mitarbeitern

## 🚀 Schnellstart

### 1. Server starten
```powershell
cd "C:\Users\DRKairport\OneDrive - Deutsches Rotes Kreuz - Kreisverband Köln e.V\Dateien von Erste-Hilfe-Station-Flughafen - DRK Köln e.V_ - !Gemeinsam.26\Nesk\Dienstwünsche"
.\.venv\Scripts\python.exe app.py
```

### 2. Browser öffnen
```
http://localhost:5000
```

### 3. Admin-Login
- **Benutzername**: `Groß`
- **Passwort**: `mettwurst`

## 📖 Benutzer-Anleitung

### Für normale Mitarbeiter:
1. **Registrieren** (falls noch nicht vorhanden)
2. **Anmelden** mit Benutzername und Passwort
3. **Kalender ansehen** für den Folgemonat
4. **Dienste auswählen**:
   - Klicken Sie auf einen Tag
   - Der Schichttyp rotiert: T → T10 → N10 → (leer)
   - Erneut klicken zum Ändern
5. **Speichern** mit dem grünen Button
6. **Nachrichten** an Admins senden (optional)

### Für Administratoren:
1. **Anmelden** als Admin
2. **Tab "Dienstwünsche"**:
   - Sehen Sie alle Wünsche gruppiert nach Mitarbeiter
   - Klicken Sie "Bestätigen" neben jedem Dienst
   - Bestätigte Dienste zeigen ✓ und sind grün hinterlegt
3. **Tab "Benutzerverwaltung"**:
   - Rollen ändern (Admin/Benutzer)
   - Passwörter zurücksetzen
4. **Tab "Nachrichten"**:
   - Lesen Sie Nachrichten von Mitarbeitern
   - Markieren Sie als gelesen
5. **Tab "Export"**:
   - **Excel**: Detaillierte Liste
   - **PDF**: Kalender-Layout mit allen Mitarbeitern
   - **JSON**: Rohdaten

## 🔧 Technische Details

### Datenbank-Migration
Falls Sie alte Daten haben (Früh/Spät/Nacht):
```powershell
.\.venv\Scripts\python.exe migrate_shift_types.py
```

### Deployment auf Render
1. Code zu GitHub pushen:
```powershell
git add .
git commit -m "Update mit Kalender-Interface"
git push origin main
```

2. Render deployed automatisch bei jedem Push

### Dateien-Struktur
```
Dienstwünsche/
├── app.py                          # Haupt-Anwendung (Flask Backend)
├── migrate_shift_types.py          # Migrations-Skript
├── requirements.txt                # Python-Dependencies
├── instance/
│   └── dienstwuensche.db          # SQLite Datenbank (lokal)
└── templates/
    ├── login.html                  # Login/Registrierung
    ├── shift_request_form_new.html # Kalender-Interface (Benutzer)
    └── admin_dashboard_new.html    # Admin-Bereich (gebündelt)
```

## 🎨 Features im Detail

### Kalender-Interface
- **Automatische Berechnung** des Folgemonats
- **Wochentag-Header** (Mo-So)
- **Klick-Rotation** durch Schichttypen
- **Visuelle Hervorhebung**:
  - Gelb (T), Blau (T10), Indigo (N10)
  - Grüner Rahmen bei Bestätigung
- **Speichern-Logik**:
  - Löscht alte, nicht-bestätigte Einträge
  - Erstellt neue Einträge für ausgewählte Tage
  - Behält bestätigte Dienste bei

### Admin-Dashboard
- **Gruppierung** nach Mitarbeiter
- **Sortierung** alphabetisch
- **Inline-Bestätigung** direkt beim Dienst
- **Farbcodierung** der Schichttypen
- **Wochentags-Anzeige** neben Datum

### PDF-Export
- **Landscape A4** Format
- **Kalender-Grid**:
  - Spalte 1: Mitarbeiter-Name
  - Spalten 2-32: Tage des Monats
- **Header**:
  - Tagesnummern
  - Wochentags-Kürzel
- **Zellen**:
  - Farbcodierung (T/T10/N10)
  - Bestätigungs-Marker (✓)
- **Legende** am Seitenende
- **Automatischer Seitenwechsel**

## 📊 API-Endpunkte

### Für Benutzer:
- `GET /api/shift-requests` - Lade eigene Dienstwünsche (Folgemonat)
- `POST /api/shift-requests` - Erstelle Dienstwunsch
- `DELETE /api/shift-requests/<id>` - Lösche Dienstwunsch
- `GET /api/messages` - Lade Nachrichten
- `POST /api/messages` - Sende Nachricht

### Für Admins:
- `GET /api/admin/users` - Alle Benutzer
- `POST /api/admin/users/<id>/toggle-admin` - Rolle ändern
- `POST /api/admin/users/<id>/reset-password` - Passwort zurücksetzen
- `POST /api/admin/shift-requests/<id>/confirm` - Dienst bestätigen/ablehnen
- `GET /api/admin/export` - JSON Export
- `GET /api/admin/export/excel` - Excel Export
- `GET /api/admin/export/pdf` - PDF Export (Kalender)

## 🔐 Sicherheit

- **Session-basierte Authentifizierung**
- **SHA-256 Passwort-Hashing**
- **Role-Based Access Control** (RBAC)
- **CSRF-Protection** (Flask-integriert)
- **Validierung** aller Eingaben

## 🐛 Bekannte Einschränkungen

1. **Nur Folgemonat**: Dienstwünsche können nur für den Folgemonat eingereicht werden
2. **Ein Dienst pro Tag**: Pro Tag kann nur eine Schicht ausgewählt werden
3. **Bestätigte Dienste**: Können vom Benutzer nicht mehr gelöscht werden
4. **Nachträgliche Änderungen**: Bestätigte Dienste können nur vom Admin geändert werden

## 📝 Changelog

### Version 2.0 (17. Januar 2026)
- ✅ Kalender-Interface statt Datum-Picker
- ✅ Neue Schichttypen (T, T10, N10)
- ✅ Gebündelte Admin-Ansicht nach Mitarbeiter
- ✅ Inline-Bestätigung direkt bei Diensten
- ✅ PDF-Export mit Kalender-Layout
- ✅ Automatische Daten-Migration
- ✅ Folgemonat-Filter in API
- ✅ Verbesserte UI mit Farbcodierung

### Version 1.x (Januar 2026)
- ✅ Basis-System mit Flask
- ✅ User-Registrierung
- ✅ Admin-Dashboard
- ✅ Message-System
- ✅ Excel/PDF/JSON Export
- ✅ Wochentags-Anzeige
- ✅ Render-Deployment

## 🆘 Support

Bei Fragen oder Problemen:
1. Prüfen Sie die Logs im Terminal
2. Kontrollieren Sie die Browser-Konsole (F12)
3. Schauen Sie in `instance/dienstwuensche.db`
4. Kontaktieren Sie den System-Administrator

## 📜 Lizenz

© 2026 DRK Köln - Erste-Hilfe-Station Flughafen
Internes Tool - Nicht zur Weitergabe bestimmt
