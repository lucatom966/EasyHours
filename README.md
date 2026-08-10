# Zeiterfassung

Eine lokale, werbefreie Single-File-Web-App zur persönlichen Arbeitszeiterfassung – läuft komplett im Browser, ohne Server, ohne Konto, ohne Cloud. Alle Daten bleiben auf dem eigenen Gerät.

Die gesamte App (HTML, CSS, JavaScript) steckt in **einer einzigen Datei**: `zeiterfassung.html`. Es gibt keinen Build-Schritt, keine Abhängigkeiten zum Installieren – Datei öffnen, fertig.

---

## Inhalt

- [Funktionen](#funktionen)
- [Wie die Speicherung funktioniert](#wie-die-speicherung-funktioniert)
- [Ordnerstruktur](#ordnerstruktur)
- [Nutzung](#nutzung)
- [Browser-Kompatibilität](#browser-kompatibilität)
- [Datenschutz](#datenschutz)

---

## Funktionen

Die App gliedert sich in fünf Tabs: **Dashboard**, **Erfassen**, **Timer**, **Wochen** und **Einstellungen**.

### 📊 Dashboard – frei konfigurierbar
Ein Widget-basiertes Dashboard mit über 20 Widgets aus vier Kategorien, die frei kombiniert, in der Grösse (S/M/L) angepasst, per ▲/▼ neu angeordnet und wieder entfernt werden können:

| Kategorie | Widgets |
|---|---|
| **Kennzahlen** | Arbeitszeit, Ø Std./Tag, Erfasste Tage, Top Standort, Top Buchungstyp, Längster Tag, Abwesenheitstage, Ø Kommen–Gehen, Serie in Folge, Vergleich zur Vorperiode |
| **Ziele & Saldo** | Ziel-Fortschritt (Tacho-Anzeige mit editierbarem Sollwert), **Gleitzeit-Saldo** – kumuliert über alle Einträge **plus einem editierbaren Startsaldo** (z. B. der Saldo aus der Zeit vor diesem Tool), Anzeige in Stunden **und** Tagen mit Dezimalstelle |
| **Verteilungen** | Standortverteilung (Donut), Buchungstyp-Verteilung (Donut), Wochentag-Muster, Ankunftszeiten-Histogramm, Standort-Rangliste |
| **Verläufe** | Trend (Tag/Woche/Monat/Jahr – jeweils eigene, unabhängig wählbare Zeiträume), Kalender-Heatmap (GitHub-Stil), Letzte Einträge (Mini-Tabelle) |

Viele Widgets haben einen eigenen, unabhängigen Zeitraum-Filter – z. B. lässt sich "Arbeitszeit diese Woche" neben "Arbeitszeit dieses Jahr" gleichzeitig anzeigen.

### ⏱️ Erfassen
Formular für den Tageseintrag: Datum (über einen scrollbaren Kalenderstreifen), Standort, Kommen/Gehen-Zeiten, Mittagspause, Bemerkung sowie eine Aufteilung der Arbeitszeit nach Buchungstyp. Bereits hinzugefügte Aufteilungen lassen sich **direkt inline bearbeiten** (Typ, Betreff, Stunden) – kein Löschen-und-neu-Anlegen mehr nötig.

### ⏲️ Timer
Zeit direkt live tracken, statt sie im Nachhinein zu schätzen:

- Buchungstyp und Betreff wählen, **Start** klicken – der Timer läuft, auch im Hintergrund weiter, wenn die Seite geschlossen und später wieder geöffnet wird (basiert auf Zeitstempeln, nicht auf einem blossen Zähler).
- **Stopp** friert die Zeit ein; von dort aus entweder **Fortsetzen** (pausieren und später weiterlaufen lassen – mehrfach möglich, alle Segmente werden aufsummiert), **Speichern** oder **Verwerfen**.
- Vor dem Speichern zeigt eine Vorschau genau, was gebucht wird, z. B. *"wird gebucht als 0.3 Std. auf CR · 38523"*.
- Kleinste Buchungseinheit: **0.1 Std. (6 Minuten)**, auf- oder abgerundet auf das nächstliegende 6er-Vielfache.
- Gespeichert wird als Aufteilung auf den **heutigen Tag** – exakt wie eine manuell hinzugefügte Aufteilung in "Erfassen". Existiert für heute noch kein Eintrag, wird automatisch einer angelegt (Kommen/Gehen = Timer-Start/-Stopp); existiert bereits einer, werden dessen Gesamtstunden/Zeiten nicht verändert.
- Es kann immer nur ein Timer gleichzeitig laufen.
- **Custom Timer**: eigene Vorlagen aus Buchungstyp + Betreff (z. B. "CR · 38523") zum Ein-Klick-Start, beliebig hinzufügen oder löschen. Löschen einer Vorlage entfernt nur die Vorlage – bereits gebuchte Arbeitszeit bleibt erhalten.

### 📅 Wochen
Wochenweise Tabellenübersicht mit Navigation zwischen Kalenderwochen und Excel-Export (`.xlsx`) pro Woche.

### ⚙️ Einstellungen
Zentrale Stelle für alles Konfigurative:

- **Ordner**: Excel-Zielordner (für den wöchentlichen Export) und JSON-Backup-Zielordner (siehe unten) – unabhängig voneinander wählbar.
- **Backup & Wiederherstellung**: manuelles Backup erzeugen, bestehendes Backup wieder einspielen.
- **Darstellung**: Dark-/Light-Theme umschalten.
- **Buchungstypen verwalten**: eigene Typen (z. B. CR, IC, OPS) hinzufügen, umbenennen oder löschen. Bereits erfasste Einträge behalten ihren ursprünglichen Typ unabhängig von späteren Änderungen an der Liste.

### 📁 Automatischer Export & Backup
- **Excel** wird automatisch jeden **Freitag** beim Speichern eines Eintrags für die jeweilige Woche exportiert.
- **JSON-Backup** wird automatisch gespeichert, **sobald ein neuer, bisher nicht gebuchter Tag** erfasst wird (über "Erfassen" oder über den Timer) – nicht nur einmal pro Woche.
- Ist im jeweiligen Zielordner ein Ordner ausgewählt (Chrome/Edge, File System Access API), landen beide Dateien **direkt dort**, ohne Download-Dialog. Ohne gewählten Ordner: Excel fällt weiterhin auf einen normalen Download zurück; das häufigere JSON-Backup wird bewusst **nur** ausgelöst, wenn ein Ordner konfiguriert ist, um nicht bei jedem neuen Tag einen Download-Popup zu erzeugen.

---

## Wie die Speicherung funktioniert

Es gibt **keine Datenbank und keinen Server**. Die App speichert ausschliesslich im Browser des jeweiligen Geräts, über zwei Web-Standard-Mechanismen:

### 1. `localStorage` (Haupt-Datenspeicher)

| Key | Inhalt |
|---|---|
| `zb-day-data-v1` | Alle erfassten Tageseinträge inkl. Aufteilungen (JSON-Array) |
| `zb-dashboard-widgets-v2` | Layout, Grösse und Einstellungen der Dashboard-Widgets |
| `zb-booking-types-v1` | Liste der Buchungstypen (Name + Farbe) |
| `zb-active-timer-v1` | Zustand des laufenden/gestoppten Timers (Typ, Betreff, Zeitstempel) |
| `zb-custom-timers-v1` | Gespeicherte Custom-Timer-Vorlagen |
| `zb-theme` | Gewähltes Theme (`light` / `dark`) |

`localStorage` ist an **Browser + Gerät + Ursprung** (Datei-URL bzw. Domain) gebunden. Das bedeutet konkret:

- Öffnest du die Datei in einem anderen Browser oder auf einem anderen Gerät, sind die Daten dort **nicht** automatisch vorhanden.
- Wird der Browser-Cache/-Verlauf geleert oder die Datei im Inkognito-/privaten Modus geöffnet, gehen die Daten **verloren**.
- Es gibt keine automatische Cloud-Synchronisation.

➡️ **Deshalb regelmässig unter Einstellungen → Backup ein JSON-Backup erstellen** (oder einen Backup-Zielordner konfigurieren, siehe unten), besonders vor Browser-Updates, System-Neuinstallationen oder Geräte­wechsel. Über *Wiederherstellen* lässt sich diese JSON-Datei jederzeit wieder einspielen (bestehende Einträge mit gleichem Datum werden dabei überschrieben).

### 2. `IndexedDB` (nur für die beiden Export-Zielordner)

| Datenbank | Store | Key | Inhalt |
|---|---|---|---|
| `zb-fs-handles` | `handles` | `exportDir` | Referenz (`FileSystemDirectoryHandle`) auf den gewählten Excel-Zielordner |
| `zb-fs-handles` | `handles` | `backupDir` | Referenz auf den gewählten JSON-Backup-Zielordner |

Wird ein Zielordner unter Einstellungen ausgewählt, merkt sich der Browser diesen Ordner-Zugriff dauerhaft (Chrome/Edge, File System Access API). Die eigentlichen Excel- und JSON-Dateien liegen dann **nicht** im Browser, sondern ganz normal als Dateien im gewählten Ordner auf der Festplatte/OneDrive.

> **Hinweis:** Diese Funktion steht nur in Chromium-Browsern (Chrome, Edge) zur Verfügung. In Firefox/Safari fällt der Export automatisch auf einen normalen Datei-Download zurück – alle übrigen Funktionen der App sind davon nicht betroffen.

### Zusammengefasst

```
┌─────────────────────────────┐
│  zeiterfassung.html          │  ← Anwendung (Code)
└──────────────┬───────────────┘
               │ läuft im Browser
               ▼
   ┌───────────────────────┐        ┌──────────────────────────┐
   │  localStorage          │        │  IndexedDB                │
   │  • Tageseinträge        │        │  • Ordner-Zugriff Excel   │
   │  • Dashboard-Layout      │        │  • Ordner-Zugriff Backup  │
   │  • Buchungstypen         │        └──────────────────────────┘
   │  • Timer-Zustand         │
   │  • Theme                │
   └───────────────────────┘
               │
               ▼ (automatisch bei neuem Tag, oder manuell)
   ┌───────────────────────┐
   │  Backup-JSON-Datei      │  ← im Backup-Zielordner oder als Download
   └───────────────────────┘
```

---

## Ordnerstruktur

### Repository (GitHub)

Die App besteht aus einer einzigen Datei und braucht keine komplexe Struktur:

```
zeiterfassung/
├── README.md              ← diese Datei
├── zeiterfassung.html      ← die App (öffnen und nutzen)
└── LICENSE                 ← optional
```

Für GitHub Pages einfach `zeiterfassung.html` zusätzlich als `index.html` bereitstellen (Kopie oder Rename), damit die Seite unter der Pages-URL direkt erreichbar ist:

```
zeiterfassung/
├── README.md
├── zeiterfassung.html
└── index.html               ← identische Kopie, für GitHub Pages
```

### Lokale Ablage / Export-Zielordner

Für die Nutzung im Arbeitsalltag (z. B. wie bei Inventx mit OneDrive) empfiehlt sich folgende Struktur, getrennt nach *App* und den beiden Ablageorten:

```
LF_Docs\
└── Rapportieren\
    ├── zeiterfassung.html    ← lokale Kopie der App (offline nutzbar)
    └── WebReport\
        ├── Archiv\           ← Excel-Zielordner (Freitags-Export, manueller Export)
        │   ├── Arbeitszeit_KW32_2026.xlsx
        │   ├── Arbeitszeit_KW33_2026.xlsx
        │   └── ...
        └── Backups\          ← JSON-Backup-Zielordner (automatisch bei jedem neuen Tag)
            ├── zeiterfassung_backup_2026-08-10.json
            ├── zeiterfassung_backup_2026-08-11.json
            └── ...
```

Beide Ordner werden einmalig über die entsprechenden Buttons unter **Einstellungen → Ordner** ausgewählt – die App merkt sich diese Pfade im jeweiligen Browser und legt künftige Excel- bzw. JSON-Dateien automatisch dort ab, ohne den Download-Dialog des Browsers zu benutzen. Da jede Backup-Datei das Tagesdatum im Namen trägt, überschreibt sich nichts – mit der Zeit entsteht eine Historie mehrerer Sicherungsstände.

---

## Nutzung

1. `zeiterfassung.html` herunterladen (bzw. Repository klonen).
2. Datei per Doppelklick im Browser öffnen – **kein Server, keine Installation nötig**.
3. Optional: Datei über den Browser als Lesezeichen oder über *"Zum Homescreen hinzufügen"* (iOS/Android) wie eine App ablegen.
4. Unter **Einstellungen** einmalig den Excel- und den JSON-Backup-Zielordner festlegen (Chrome/Edge empfohlen).
5. Bei Bedarf unter **Einstellungen → Buchungstypen** eigene Typen anlegen sowie einen Startsaldo im Gleitzeit-Saldo-Widget hinterlegen.

---

## Browser-Kompatibilität

| Funktion | Chrome / Edge | Firefox | Safari |
|---|---|---|---|
| Zeiterfassung, Dashboard, Timer, Wochenübersicht | ✅ | ✅ | ✅ |
| Excel-Export (Download) | ✅ | ✅ | ✅ |
| Excel-/JSON-Export direkt in Zielordner | ✅ | ⚠️ Fällt auf Download zurück | ⚠️ Fällt auf Download zurück |
| Backup/Wiederherstellen (JSON) | ✅ | ✅ | ✅ |
| Darkmode / PWA-Homescreen | ✅ | ✅ | ✅ |

---

## Datenschutz

Es werden keinerlei Daten an einen Server übertragen. Es gibt keine Analyse-, Tracking- oder Werbe-Skripte. Die einzige externe Ressource ist die [SheetJS/xlsx-Bibliothek](https://cdnjs.cloudflare.com/) für den Excel-Export sowie der Google-Font *Inter*, beide werden per CDN nachgeladen (Internetverbindung beim ersten Öffnen bzw. für den Excel-Export erforderlich). Sämtliche Arbeitszeit-Daten verbleiben ausschliesslich lokal im Browser bzw. im selbst gewählten Export-Ordner.
