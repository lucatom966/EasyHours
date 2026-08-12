# Zeiterfassung

Eine lokale, werbefreie Single-File-Web-App zur persönlichen Arbeitszeiterfassung – läuft komplett im Browser, ohne Server, ohne Konto, ohne Cloud. Alle Daten bleiben auf dem eigenen Gerät.

Die gesamte App (HTML, CSS, JavaScript) steckt in **einer einzigen Datei**: `zeiterfassung.html`. Es gibt keinen Build-Schritt, keine Abhängigkeiten zum Installieren – Datei öffnen, fertig.

---

## 🚀 Quickstart

1. **Ordnerstruktur anlegen**, z. B. so:

   ```
   EasyHours/
   ├── zeiterfassung.html
   ├── Wochen/            ← Excel-Wochenexporte
   ├── Backup/             ← JSON-Sicherungen
   └── Monthly Report/      ← PDF-Monatsberichte
   ```

2. Die `zeiterfassung.html`-Datei in den `EasyHours`-Ordner legen und per Doppelklick im Browser öffnen (Chrome oder Edge empfohlen, siehe [Browser-Kompatibilität](#browser-kompatibilität)).

3. In der App zu **Einstellungen → Ordner** wechseln und die drei Ordner einmalig zuordnen:
   - **Excel-Zielordner** → `Wochen`
   - **JSON-Backup-Zielordner** → `Backup`
   - **PDF-Zielordner** → `Monthly Report`

   Ab jetzt landen alle automatisch erzeugten Dateien direkt dort – ohne Download-Dialog.

4. Kurz durch die übrigen **Einstellungen** gehen und nach Bedarf anpassen:
   - **Buchungstypen** und **Standorte** auf die eigenen Bedürfnisse zuschneiden (umbenennen, Farben wählen, Reihenfolge per Drag & Drop oder ▲/▼ festlegen, eigene ergänzen oder löschen)
   - **Tastenkombinationen** durchsehen und bei Bedarf umbelegen
   - **Erinnerungen** einrichten, falls gewünscht (z. B. Erinnerung am Feierabend, falls noch kein Eintrag erfasst wurde)
   - **Darkmode** nach Geschmack

5. Los geht's – im Tab **Erfassen** den ersten Tag eintragen, oder im Tab **Timer** direkt live loslegen.

---

## Inhalt

- [Quickstart](#-quickstart)
- [Funktionen](#funktionen)
- [Wie die Speicherung funktioniert](#wie-die-speicherung-funktioniert)
- [Ordnerstruktur](#ordnerstruktur)
- [Browser-Kompatibilität](#browser-kompatibilität)
- [Datenschutz](#datenschutz)

---

## Funktionen

Die App gliedert sich in fünf Tabs: **Dashboard**, **Erfassen**, **Timer**, **Wochen** und **Einstellungen**.

### 📊 Dashboard – frei konfigurierbar
Ein Widget-basiertes Dashboard mit **über 40 Widgets** aus vier Kategorien, die frei kombiniert, in der Grösse (S/M/L) angepasst, per ▲/▼ neu angeordnet und wieder entfernt werden können. Jedes Widget zeigt bei grösserer Grösse automatisch mehr Detail (S = nur der Wert, M = zusätzlicher Kontext, L = zusätzlicher Mini-Trend). Alle Widgets in derselben Zeile sind gleich hoch.

| Kategorie | Auswahl an Widgets |
|---|---|
| **Kennzahlen** (25) | Arbeitszeit, Ø Std./Tag, Erfasste Tage, Top Standort, Top Buchungstyp, Längster Tag, Abwesenheitstage, Ø Kommen–Gehen, Serie in Folge, Vergleich zur Vorperiode, Ø Pausendauer, Längste/Kürzeste Pause, Unvollständige Tage, Buchungstyp-Vielfalt, Timer-Anteil, Timer-Sitzungen, Wochenend-Einsätze, Früh & Spät, Erfassungs-Quote, Nächster Freitags-Export, Aktivste/Ruhigste Woche, Wochen-Extreme, Standortwechsel |
| **Ziele & Saldo** | Ziel-Fortschritt (Tacho mit editierbarem Sollwert), Gleitzeit-Saldo (inkl. optionalem Startsaldo, in Std. **und** Tagen), Jahresfortschritt (Jahresziel-Pace) |
| **Verteilungen** | Standortverteilung, Buchungstyp-Verteilung, Wochentag-Muster, Ankunftszeiten, Standort-Rangliste, Referenz-Rangliste (meistgenutzte Betreffe/Ticket-Nrn.), Pausen nach Standort |
| **Verläufe** | Trend (Tag/Woche/Monat/Jahr, je nach Widget-Grösse mit mehr oder weniger Datenpunkten), Kalender-Heatmap (GitHub-Stil), Letzte Einträge, Buchungstyp-Trend (frei wählbarer Typ), Standort-Trend (frei wählbarer Standort) |

Viele Widgets haben einen eigenen, unabhängigen Zeitraum-Filter – z. B. lässt sich "Arbeitszeit diese Woche" neben "Arbeitszeit dieses Jahr" gleichzeitig anzeigen. Bei Widgets mit editierbaren Zielwerten (Ziel-Fortschritt, Gleitzeit-Saldo, Jahresfortschritt) erscheinen die Eingabefelder dafür nur im Dashboard-Bearbeitungsmodus, damit die normale Ansicht aufgeräumt bleibt.

### ⏱️ Erfassen
Formular für den Tageseintrag: Datum (über einen scrollbaren Kalenderstreifen, heutiger Tag stets blau umrandet), Standort, Kommen/Gehen-Zeiten, Mittagspause, Bemerkung sowie eine Aufteilung der Arbeitszeit nach Buchungstyp. Bereits hinzugefügte Aufteilungen lassen sich direkt inline bearbeiten. In jedem Feld speichert **Enter** automatisch den zugehörigen Button rechts daneben (z. B. "Tag speichern" oder "Hinzufügen").

### ⏲️ Timer
Zeit direkt live tracken, statt sie im Nachhinein zu schätzen:

- Buchungstyp und Betreff wählen, **Start** klicken – der Timer läuft auf Basis von Zeitstempeln weiter, auch wenn die Seite zwischenzeitlich geschlossen wird.
- **Stopp** friert die Zeit ein; von dort aus **Fortsetzen** (beliebig oft pausieren/weiterlaufen lassen, alle Segmente werden aufsummiert), **Speichern** oder **Verwerfen**.
- Vor dem Speichern zeigt eine Vorschau genau, was gebucht wird, z. B. *"wird gebucht als 0.3 Std. auf CR · 38523"*.
- Kleinste Buchungseinheit: **0.1 Std. (6 Minuten)**, auf-/abgerundet auf das nächste 6er-Vielfache.
- Gespeichert wird als Aufteilung auf den **heutigen Tag** – existiert dafür noch kein Eintrag, wird automatisch einer angelegt; existiert bereits einer, bleiben dessen Gesamtstunden/Zeiten unverändert.
- **Fokus-Modus**: Vollbild-Ansicht mit grosser Zeitanzeige, minimal ablenkend (per Button oder Tastenkombination, Esc zum Verlassen).
- **Custom Timer**: eigene Vorlagen aus Buchungstyp + Betreff zum Ein-Klick-Start, per Drag & Drop oder ▲/▼ sortierbar, beliebig hinzufügen/löschen (Löschen einer Vorlage lässt bereits gebuchte Arbeitszeit unangetastet).

### 📅 Wochen
Wochenweise Tabellenübersicht mit Navigation zwischen Kalenderwochen und Excel-Export (`.xlsx`) pro Woche.

### ⚙️ Einstellungen
Alle Bereiche sind klappbar, damit es übersichtlich bleibt:

- **Ordner** – drei unabhängige Zielordner (Excel, JSON-Backup, PDF-Monatsbericht), siehe [Ordnerstruktur](#ordnerstruktur)
- **Daten** – manuelles Backup erstellen, bestehendes Backup wieder einspielen
- **Darstellung** – Dark-/Light-Theme
- **Buchungstypen** – eigene Typen anlegen/umbenennen/löschen/farblich anpassen (natives Farbwahl-Element: Palette **und** Hex-Code), Reihenfolge per Drag & Drop (Griff ⋮⋮) oder ▲/▼-Pfeilen
- **Standorte** – ebenso frei verwaltbar; Standard ab Werk: Chur, Zürich, St. Gallen, Bern, Homeoffice (jederzeit auf Standard zurücksetzbar)
- **Tastenkombinationen** – **15 vordefinierte Shortcuts**, jede einzeln ein-/ausschaltbar und frei neu belegbar (inkl. Kombinationen mit Ctrl/Alt), siehe Tabelle unten
- **Erinnerungen** – beliebig viele, mit eigenem Titel/Text, optionalem Ton, siehe unten

Bereits erfasste Einträge bleiben von Umbenennungen/Löschungen bei Buchungstypen und Standorten immer unberührt.

#### Tastenkombinationen (Standardbelegung)

| Taste | Aktion |
|---|---|
| Leertaste | Timer starten / stoppen |
| 1 – 5 | Zu Dashboard / Erfassen / Timer / Wochen / Einstellungen wechseln |
| D | Darkmode umschalten |
| F | Fokus-Modus umschalten |
| Ctrl+S | Eintrag speichern (in Erfassen) |
| Ctrl+B | Backup jetzt erstellen |
| Ctrl+E | Neuer Eintrag (Formular leeren) |
| Ctrl+← / Ctrl+→ | Vorherige/Nächste Woche (in Wochen) |
| Ctrl+D | Dashboard-Bearbeitungsmodus umschalten |
| Ctrl+P | Monatsbericht (PDF) jetzt erstellen |

Kombinationen mit Ctrl/Alt funktionieren auch während in einem Textfeld getippt wird; einzelne Tasten (Leertaste, 1–5, D, F) sind währenddessen bewusst deaktiviert, um normales Tippen nicht zu stören.

#### Erinnerungen

Zwei Typen, beliebig oft und parallel nutzbar:

- **Tageserinnerung** – zu fixer Uhrzeit, an frei wählbaren Wochentagen, mit Bedingung *"kein Eintrag vorhanden"* oder *"weniger als X Std. erfasst"*
- **Timer-Dauer-Erinnerung** – nach frei wählbarer Minutenzahl, wahlweise **einmalig** oder **wiederholend** (feuert dann erneut alle X Minuten, solange der Timer läuft)

Beide mit eigenem Titel, eigenem Text und optionalem Signalton. Auslösung per Browser-Benachrichtigung (mit Fallback auf eine kurze Meldung in der App, falls Benachrichtigungen nicht erlaubt wurden).

### 📁 Automatischer Export & Backup
- **Excel** wird automatisch jeden **Freitag** beim Speichern eines Eintrags für die jeweilige Woche exportiert.
- **JSON-Backup** wird automatisch gespeichert, sobald ein **neuer, bisher nicht gebuchter Tag** erfasst wird (über Erfassen oder Timer) – vorausgesetzt, ein Backup-Zielordner ist konfiguriert (ohne Ordner bleibt es bewusst still, um nicht bei jedem neuen Tag einen Download auszulösen).
- **PDF-Monatsbericht** wird automatisch am **letzten Tag jedes Monats** erzeugt: farbiger Header, Kennzahlen-Übersicht, vollständige Tagestabelle mit Aufteilungen, Seitenzahlen – professionell aufbereitet für Ablage oder Weitergabe.

Sind die jeweiligen Zielordner konfiguriert, landen alle drei Dateiarten direkt dort (Chrome/Edge, File System Access API); ohne Ordner fällt der jeweilige Export auf einen normalen Download zurück.

---

## Wie die Speicherung funktioniert

Es gibt **keine Datenbank und keinen Server**. Die App speichert ausschliesslich im Browser des jeweiligen Geräts.

### `localStorage` (Haupt-Datenspeicher)

| Inhalt |
|---|
| Tageseinträge inkl. Aufteilungen |
| Dashboard-Widget-Layout und -Einstellungen |
| Buchungstypen (Namen + Farben) |
| Standorte (Namen + Farben) |
| Custom-Timer-Vorlagen |
| Tastenkombinationen (Belegung + Ein/Aus-Status) |
| Erinnerungen (alle Einstellungen) |
| Theme (Hell/Dunkel) |
| Zustand des laufenden/pausierten Timers |

`localStorage` ist an **Browser + Gerät + Ursprung** gebunden. Öffnest du die Datei in einem anderen Browser oder auf einem anderen Gerät, sind die Daten dort **nicht** automatisch vorhanden; wird der Browser-Verlauf geleert, gehen sie **verloren**.

➡️ **Deshalb regelmässig unter Einstellungen → Daten ein Backup erstellen** (oder einen Backup-Zielordner konfigurieren, siehe Quickstart) – besonders vor Browser-Updates, System-Neuinstallationen oder Geräte­wechsel. Das Backup enthält alles aus der Tabelle oben (mit Ausnahme der Ordner-Verknüpfungen selbst – siehe unten) in einer einzigen JSON-Datei und lässt sich jederzeit vollständig wieder einspielen.

### `IndexedDB` (nur für die drei Export-Zielordner)

Wird ein Zielordner unter Einstellungen ausgewählt, merkt sich der Browser diesen Ordner-Zugriff dauerhaft (Chrome/Edge, File System Access API). Die eigentlichen Dateien liegen dann ganz normal im gewählten Ordner auf der Festplatte – nicht im Browser.

> **Hinweis:** Diese Funktion steht nur in Chromium-Browsern zur Verfügung. In Firefox/Safari fällt der jeweilige Export automatisch auf einen normalen Datei-Download zurück; alle übrigen Funktionen sind davon nicht betroffen. Ordner-Verknüpfungen können aus Sicherheitsgründen nicht in ein Backup mit aufgenommen werden und müssen nach einer Wiederherstellung einmalig neu ausgewählt werden.

---

## Ordnerstruktur

### Lokale Ablage (empfohlen, siehe Quickstart)

```
EasyHours/
├── zeiterfassung.html      ← die App
├── Wochen/                 ← Excel-Zielordner
│   ├── Arbeitszeit_KW32_2026.xlsx
│   └── ...
├── Backup/                  ← JSON-Backup-Zielordner
│   ├── zeiterfassung_backup_2026-08-10.json
│   └── ...
└── Monthly Report/           ← PDF-Zielordner
    ├── Monatsbericht_Juli_2026.pdf
    └── ...
```

Jede Backup-Datei trägt das Tagesdatum im Namen – es überschreibt sich nichts, mit der Zeit entsteht eine Historie mehrerer Sicherungsstände.

### Repository (GitHub)

```
zeiterfassung/
├── README.md
├── zeiterfassung.html
└── LICENSE                 ← optional
```

Für GitHub Pages zusätzlich als `index.html` bereitstellen (Kopie oder Rename).

---

## Browser-Kompatibilität

| Funktion | Chrome / Edge | Firefox | Safari |
|---|---|---|---|
| Zeiterfassung, Dashboard, Timer, Wochenübersicht, Einstellungen | ✅ | ✅ | ✅ |
| Excel-/PDF-Export (Download) | ✅ | ✅ | ✅ |
| Export direkt in Zielordner (alle 3) | ✅ | ⚠️ Fällt auf Download zurück | ⚠️ Fällt auf Download zurück |
| Backup/Wiederherstellen (JSON) | ✅ | ✅ | ✅ |
| Browser-Benachrichtigungen (Erinnerungen) | ✅ | ✅ | ⚠️ Eingeschränkt (macOS/iOS-Vorgaben) |
| Darkmode / Tastenkombinationen / Fokus-Modus | ✅ | ✅ | ✅ |

---

## Datenschutz

Es werden keinerlei Daten an einen Server übertragen. Es gibt keine Analyse-, Tracking- oder Werbe-Skripte. Externe Ressourcen (jeweils per CDN nachgeladen, Internetverbindung beim ersten Öffnen bzw. für Excel-/PDF-Export erforderlich):

- [SheetJS/xlsx](https://cdnjs.cloudflare.com/) für den Excel-Export
- [jsPDF](https://cdnjs.cloudflare.com/) + [jsPDF-AutoTable](https://cdnjs.cloudflare.com/) für den PDF-Monatsbericht
- Google-Font *Inter*

Sämtliche Arbeitszeit-Daten verbleiben ausschliesslich lokal im Browser bzw. im selbst gewählten Export-Ordner.
