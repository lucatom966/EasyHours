# WorkHour-Tracker
Track you work ours easily on this webbased UI
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

### 📊 Dashboard – frei konfigurierbar
Ein Widget-basiertes Dashboard mit über 20 Widgets aus vier Kategorien, die frei kombiniert, in der Grösse (S/M/L) angepasst, per ▲/▼ neu angeordnet und wieder entfernt werden können:

| Kategorie | Widgets |
|---|---|
| **Kennzahlen** | Arbeitszeit, Ø Std./Tag, Erfasste Tage, Top Standort, Top Buchungstyp, Längster Tag, Abwesenheitstage, Ø Kommen–Gehen, Serie in Folge, Vergleich zur Vorperiode |
| **Ziele & Saldo** | Ziel-Fortschritt (Tacho-Anzeige mit editierbarem Sollwert), Gleitzeit-Saldo (kumuliert über alle Einträge) |
| **Verteilungen** | Standortverteilung (Donut), Buchungstyp-Verteilung (Donut), Wochentag-Muster, Ankunftszeiten-Histogramm, Standort-Rangliste |
| **Verläufe** | Trend (Tag/Woche/Monat/Jahr), Kalender-Heatmap (GitHub-Stil), Letzte Einträge (Mini-Tabelle) |

Viele Widgets haben einen eigenen, unabhängigen Zeitraum-Filter – z. B. lässt sich "Arbeitszeit diese Woche" neben "Arbeitszeit dieses Jahr" gleichzeitig anzeigen.

### ⏱️ Erfassen
Formular für den Tageseintrag: Datum (über einen scrollbaren Kalenderstreifen), Standort, Kommen/Gehen-Zeiten, Mittagspause, Bemerkung sowie eine optionale Aufteilung der Arbeitszeit nach Buchungstyp (z. B. Militär, Ferien, Kompens, Krank, OPS, LER, CR, IC, SR).

### 📅 Wochen
Wochenweise Tabellenübersicht mit Navigation zwischen Kalenderwochen und Excel-Export (`.xlsx`) pro Woche.

### 📁 Export-Zielordner
Statt jede Excel-Datei manuell aus dem Downloads-Ordner zu verschieben, kann einmalig ein Zielordner ausgewählt werden (über die File System Access API, Chrome/Edge). Danach landen sowohl manuelle Exporte als auch der automatische Freitags-Export direkt dort.

### 💾 Backup & Wiederherstellung
Alle Einträge lassen sich als JSON-Datei sichern und wieder importieren (z. B. für Geräte­wechsel oder als zusätzliche Sicherung).

### 🌗 Darkmode & 📱 PWA
Umschaltbares Dark-/Light-Theme sowie Meta-Tags für "Zum Homescreen hinzufügen" auf iOS/Android.

---

## Wie die Speicherung funktioniert

Es gibt **keine Datenbank und keinen Server**. Die App speichert ausschliesslich im Browser des jeweiligen Geräts, über zwei Web-Standard-Mechanismen:

### 1. `localStorage` (Haupt-Datenspeicher)

| Key | Inhalt |
|---|---|
| `zb-day-data-v1` | Alle erfassten Tageseinträge (JSON-Array) |
| `zb-dashboard-widgets-v2` | Layout, Grösse und Einstellungen der Dashboard-Widgets |
| `zb-theme` | Gewähltes Theme (`light` / `dark`) |

`localStorage` ist an **Browser + Gerät + Ursprung** (Datei-URL bzw. Domain) gebunden. Das bedeutet konkret:

- Öffnest du die Datei in einem anderen Browser oder auf einem anderen Gerät, sind die Daten dort **nicht** automatisch vorhanden.
- Wird der Browser-Cache/-Verlauf geleert oder die Datei im Inkognito-/privaten Modus geöffnet, gehen die Daten **verloren**.
- Es gibt keine automatische Cloud-Synchronisation.

➡️ **Deshalb regelmässig über den "Backup"-Button in der App eine JSON-Sicherung erstellen** (Kopfzeile → *Backup*), besonders vor Browser-Updates, System-Neuinstallationen oder Geräte­wechsel. Über *Wiederherstellen* lässt sich diese JSON-Datei jederzeit wieder einspielen (bestehende Einträge mit gleichem Datum werden dabei überschrieben).

### 2. `IndexedDB` (nur für den Export-Zielordner)

| Datenbank | Store | Key | Inhalt |
|---|---|---|---|
| `zb-fs-handles` | `handles` | `exportDir` | Referenz (`FileSystemDirectoryHandle`) auf den gewählten Export-Ordner |

Wird ein Zielordner über *"Zielordner wählen"* ausgewählt, merkt sich der Browser diesen Ordner-Zugriff dauerhaft (Chrome/Edge, File System Access API). Die eigentlichen Excel-Dateien liegen dann **nicht** im Browser, sondern ganz normal als `.xlsx`-Dateien im gewählten Ordner auf der Festplatte/OneDrive.

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
   │  • Tageseinträge        │        │  • Ordner-Zugriff für     │
   │  • Dashboard-Layout      │        │    Excel-Export           │
   │  • Theme                │        └──────────────────────────┘
   └───────────────────────┘
               │
               ▼ (manuell, empfohlen)
   ┌───────────────────────┐
   │  Backup-JSON-Datei      │  ← über "Backup"-Button exportiert
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

Für die Nutzung im Arbeitsalltag (z. B. wie bei Inventx mit OneDrive) empfiehlt sich folgende Struktur, getrennt nach *App* und *Ablage der Wochenreports*:

```
LF_Docs\
└── Rapportieren\
    ├── zeiterfassung.html    ← lokale Kopie der App (offline nutzbar)
    └── WebReport\
        └── Archiv\           ← Zielordner, der in der App ausgewählt wird
            ├── Arbeitszeit_KW32_2026.xlsx
            ├── Arbeitszeit_KW33_2026.xlsx
            └── ...
```

Der Ordner unter `WebReport\Archiv` wird einmalig über den Button *"Zielordner wählen"* (Tab **Wochen**) ausgewählt – die App merkt sich diesen Pfad im jeweiligen Browser und legt künftige Excel-Exporte automatisch dort ab, ohne den Download-Dialog des Browsers zu benutzen.

> Backups (JSON) sind reine Sicherungsdateien der Rohdaten und unabhängig vom Export-Zielordner. Empfehlenswert ist ein zusätzlicher Unterordner, z. B. `WebReport\Backups\`, in dem die JSON-Sicherungen regelmässig manuell abgelegt werden.

---

## Nutzung

1. `zeiterfassung.html` herunterladen (bzw. Repository klonen).
2. Datei per Doppelklick im Browser öffnen – **kein Server, keine Installation nötig**.
3. Optional: Datei über den Browser als Lesezeichen oder über *"Zum Homescreen hinzufügen"* (iOS/Android) wie eine App ablegen.
4. Im Tab **Wochen** einmalig den Export-Zielordner festlegen (Chrome/Edge empfohlen).
5. Regelmässig ein Backup über den Button *Backup* in der Kopfzeile erstellen.

---

## Browser-Kompatibilität

| Funktion | Chrome / Edge | Firefox | Safari |
|---|---|---|---|
| Zeiterfassung, Dashboard, Wochenübersicht | ✅ | ✅ | ✅ |
| Excel-Export (Download) | ✅ | ✅ | ✅ |
| Excel-Export direkt in Zielordner | ✅ | ⚠️ Fällt auf Download zurück | ⚠️ Fällt auf Download zurück |
| Backup/Wiederherstellen (JSON) | ✅ | ✅ | ✅ |
| Darkmode / PWA-Homescreen | ✅ | ✅ | ✅ |

---

## Datenschutz

Es werden keinerlei Daten an einen Server übertragen. Es gibt keine Analyse-, Tracking- oder Werbe-Skripte. Die einzige externe Ressource ist die [SheetJS/xlsx-Bibliothek](https://cdnjs.cloudflare.com/) für den Excel-Export sowie der Google-Font *Inter*, beide werden per CDN nachgeladen (Internetverbindung beim ersten Öffnen bzw. für den Excel-Export erforderlich). Sämtliche Arbeitszeit-Daten verbleiben ausschliesslich lokal im Browser bzw. im selbst gewählten Export-Ordner.
