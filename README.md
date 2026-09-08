# Wirkungslog

Persönliches Log für die Titrationsphase. Erfasst, wann ein Medikament wirkt,
wie lange, mit welcher Lücke zwischen den Fenstern, und wie sich vier
Verhaltensgrößen in jedem Wirkfenster verhalten.

Läuft komplett im Browser. Kein Server, kein Konto, keine Übertragung.
Alle Einträge liegen in der IndexedDB des jeweiligen Geräts.

---

## 1. Veröffentlichen

1. Neues Repository anlegen, zum Beispiel `wirkungslog`.
2. Diese Dateien in den Wurzelordner des Repositories legen:
   `index.html`, `manifest.webmanifest`, `sw.js`,
   `icon-180.png`, `icon-192.png`, `icon-512.png`, `icon-512-maskable.png`,
   `README.md`.
3. Im Repository: **Settings → Pages**, Source auf `Deploy from a branch`,
   Branch `main`, Ordner `/ (root)`, dann **Save**.
4. Nach ein bis zwei Minuten liegt die App unter
   `https://<dein-name>.github.io/wirkungslog/`.

Das Repository darf öffentlich sein. Veröffentlicht wird nur der Code,
die Einträge verlassen den Browser nie.

**Wichtig:** GitHub Pages liefert über HTTPS aus. Das ist Voraussetzung für
Service Worker und dauerhaften Speicher. Ein lokal per Doppelklick geöffnetes
`index.html` funktioniert nur eingeschränkt.

## 2. Auf dem Home-Bildschirm installieren

**iPhone:** Seite in Safari öffnen, Teilen-Symbol, `Zum Home-Bildschirm`.

Nicht nur ein Lesezeichen anlegen. Safari löscht Browserspeicher von Websites
nach sieben Tagen ohne Interaktion. Zum Home-Bildschirm hinzugefügte Web-Apps
sind davon ausgenommen. Nach der Installation einmal öffnen und unter
**Daten → Speicher** den dauerhaften Speicher anfordern.

**Mac / Windows:** In Chrome oder Edge über das Installationssymbol in der
Adressleiste.

## 3. Daten aus der Testfassung übernehmen

Die Testfassung im Claude-Artefakt und diese Fassung haben getrennte Speicher.
Das Format ist identisch, der Weg führt über die Sicherung:

1. In der Testfassung: **Daten → Sicherung exportieren (JSON)**.
   Falls der Download blockiert wird, den angezeigten Text kopieren.
2. Hier: **Daten → Einlesen**, Datei wählen oder den Text einfügen.

Der Import führt zusammen, er überschreibt nicht. Pro Tag gewinnt der neuere
Stand, ein leerer Tag ersetzt nie einen Tag mit Inhalt, und Tage, die nur lokal
existieren, bleiben unberührt.

## 4. Erfassen ohne die App zu suchen

Die App versteht zwei Adressparameter:

| Adresse | Wirkung |
|---|---|
| `...?log=3` | legt sofort einen Eintrag „deutlich" mit der aktuellen Uhrzeit an |
| `...?log=2` | dasselbe mit „wirkt" |
| `...?log=1` | dasselbe mit „lässt nach" |
| `...?log=0` | dasselbe mit „keine" |
| `...?quick=1` | öffnet das Schnell-erfassen-Fenster |

Das Fenster wird automatisch gewählt: ein laufendes hat Vorrang, sonst die
nächste noch nicht erfasste Einnahme. Der Parameter wird nach der Ausführung
aus der Adresse entfernt.

**Kurzbefehl auf dem iPhone einrichten**

1. App „Kurzbefehle" öffnen, neuen Kurzbefehl anlegen.
2. Aktion `URLs öffnen` hinzufügen, Adresse eintragen, zum Beispiel
   `https://<dein-name>.github.io/wirkungslog/?log=2`.
3. Kurzbefehl benennen, etwa „Wirkt".
4. Über Teilen `Zum Home-Bildschirm` legen, oder als Widget auf den
   Sperrbildschirm, oder auf die Aktionstaste.

Drei solche Kurzbefehle decken den Alltag ab. Die Notiz kannst du später in
der App am Eintrag ergänzen.

## 5. Sichern

Es gibt keine automatische Synchronisation. Das Handy ist das führende Gerät.

- **Daten → Sicherung exportieren (JSON)** — vollständige Kopie, wieder einlesbar
- **Daten → Einlesen** — führt eine Sicherung mit den lokalen Daten zusammen
- **Daten → Tabelle für den Arzt (CSV)** — eine Zeile pro Tag
- **Daten → Arztbericht anzeigen** — helle Druckansicht für A4

Einmal pro Woche exportieren. Die App erinnert daran, wenn die letzte Sicherung
sieben Tage her ist.

## 6. Aufbau

- **Tracking** — ein Tag: Tagesart, zwei Einnahmeblöcke mit Wirkverlauf und
  Wirkfenster, Tagesabschluss. Wischen wechselt den Tag.
- **Dashboard** — Durchschnittstag, Zeitleiste, Kennzahlen, Verhalten, Puls,
  Vergleich nach Dosis und Tagesart, Nebenwirkungen, Datenqualität.
  Zeitraum wählbar: 7 Tage, 30 Tage, alles oder frei.
- **Daten** — Export, Import, Arztbericht, Speicher, Gefahrenzone.
- **Plus-Knopf** — Schnell erfassen, von überall.

## 7. Konventionen

- Der Tag beginnt um 04:00 Uhr. Ein Eintrag um 00:20 zählt zum Vortag.
- Wirkbeginn und Wirkende werden aus deinen Einträgen **vorgeschlagen**
  (gestrichelt). Sobald du sie änderst, gelten sie als bestätigt. Beide Werte
  werden gespeichert.
- Wirkbeginn kann nie vor der Einnahme liegen, Wirkende nie vor dem Wirkbeginn.
- Ein Eintrag, der mehr als 90 Minuten nach seiner Uhrzeit erfasst wurde, gilt
  als nachgetragen und wird markiert. Der Anteil steht unter Datenqualität.
- „Keine Einnahme" ist ein eigener Zustand. Umschalten löscht nichts, das
  Fenster fällt nur aus der Auswertung.
- Puls im Fenster = Mittel aller Messungen zwischen einer und drei Stunden
  nach der Einnahme.

## 8. Grenze

Das Log zeigt gut, **wie** das Medikament über den Tag wirkt: Latenz,
Wirkdauer, Lücke zwischen den Fenstern. Ob es wirkt, kann es nicht beweisen,
weil es keinen Vergleichszeitraum ohne Medikament gibt und keine Verblindung.
Der einzige Vergleich, der in diese Richtung zeigt, ist die Ansicht nach Dosis.

Dosisentscheidungen trifft der Arzt. Das Log ist Input für dieses Gespräch.

## 9. Aktualisieren

Neue `index.html` ins Repository legen. Der Service Worker holt beim nächsten
Start die frische Version aus dem Netz und fällt nur offline auf den Cache
zurück. Bei größeren Änderungen die Zeile `const CACHE = "wirkungslog-v2"` in
`sw.js` hochzählen.

Deine Daten bleiben bei einer Aktualisierung erhalten, sie liegen in der
IndexedDB und nicht im Cache.
