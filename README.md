# Wirkungslog

Persönliches Log für die Titrationsphase. Erfasst bis zu drei Einnahmen pro Tag,
wann sie wirken, wie lange, mit welcher Lücke zwischen den Fenstern, und wie
sich die Verhaltensgrößen in jedem Wirkfenster verhalten.

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

GitHub Pages liefert über HTTPS aus. Das ist Voraussetzung für Service Worker
und dauerhaften Speicher.

## 2. Auf dem Home-Bildschirm installieren

**iPhone:** Seite in Safari öffnen, Teilen-Symbol, `Zum Home-Bildschirm`.

Nicht nur ein Lesezeichen anlegen. Safari löscht Browserspeicher von Websites
nach sieben Tagen ohne Interaktion. Zum Home-Bildschirm hinzugefügte Web-Apps
sind davon ausgenommen. Nach der Installation einmal öffnen und unter
**Daten → Speicher** den dauerhaften Speicher anfordern.

## 3. Aktualisieren

1. In der laufenden App: **Daten → Sicherung exportieren (JSON)** und die Datei
   in iCloud Drive oder Dateien ablegen.
2. Im Repository nur `index.html` ersetzen (und diese `README.md`).
   `sw.js`, Manifest und Icons bleiben unverändert.
3. Ein bis zwei Minuten warten, die App im App-Umschalter schließen und neu
   öffnen. Erscheint noch die alte Fassung, nach einigen Minuten noch einmal.

Die Daten liegen in der IndexedDB, nicht im Cache, und bleiben bei einer
Aktualisierung erhalten.

### Wechsel von Version 1 auf Version 2

Beim ersten Start hebt Version 2 den Datenstand automatisch an:

- Vor der Umstellung legt sie eine unveränderte Kopie des alten Stands an
  (**Daten → Automatische Kopien → Vor dem Update**). Sie bleibt 30 Tage und
  lässt sich zurückspielen.
- Jede Einnahme, jeder Eintrag, jede Bewertung und jede Notiz bleibt erhalten.
- Pulswerte werden entfernt. Einträge, die nur aus einem Pulswert bestanden,
  entfallen mit. Die Kopie „Vor dem Update" enthält sie noch, bis sie abläuft
  oder gelöscht wird.
- Jeder Tag bekommt eine leere, optionale dritte Einnahme.
- Alte Sicherungsdateien (Version 1) lassen sich weiterhin einlesen und werden
  dabei ebenfalls angehoben.

Kann die App ihren Speicher nicht lesen, oder stammen die Daten aus einer
neueren Version, startet sie im Nur-Lesen-Modus und überschreibt nichts.

## 4. Erfassen ohne die App zu suchen

| Adresse | Wirkung |
|---|---|
| `...?log=3` | Eintrag „deutlich" mit aktueller Uhrzeit |
| `...?log=2` | Eintrag „wirkt" |
| `...?log=1` | Eintrag „lässt nach" |
| `...?log=0` | Eintrag „keine" |
| `...?event=on` | setzt den Wirkbeginn auf jetzt |
| `...?event=off` | setzt das Wirkende auf jetzt |
| `...?quick=1` | öffnet das Schnell-erfassen-Fenster |

Die Einnahme wird automatisch gewählt: ein laufendes Fenster hat Vorrang, sonst
die nächste noch nicht erfasste Einnahme. Bei `event=off` gewinnt das älteste
Fenster, das begonnen, aber noch kein Ende hat. Ein bereits gesetzter Wert wird
nie überschrieben. Der Parameter wird nach der Ausführung aus der Adresse
entfernt.

**Kurzbefehl auf dem iPhone**

1. App „Kurzbefehle", neuer Kurzbefehl.
2. Aktion `URLs öffnen`, Adresse eintragen, zum Beispiel
   `https://<dein-name>.github.io/wirkungslog/?event=on`.
3. Benennen, etwa „Wirkung spürbar".
4. Auf den Home-Bildschirm, als Widget auf den Sperrbildschirm oder auf die
   Aktionstaste legen.

Zwei Kurzbefehle (`event=on`, `event=off`) decken Wirkbeginn und Wirkende ab.

## 5. Sichern

Es gibt keine Synchronisation. Das Handy ist das führende Gerät.

- **Sicherung exportieren (JSON):** vollständige Kopie, öffnet das
  Teilen-Fenster (Dateien, AirDrop, Mail). Ohne Teilen-Fenster wird die Datei
  heruntergeladen oder als Text angezeigt.
- **Einlesen:** führt eine Sicherung mit den lokalen Daten zusammen. Pro Tag
  gewinnt der neuere Stand, ein leerer Tag ersetzt nie einen Tag mit Inhalt.
- **Tabelle für den Arzt (CSV):** eine Zeile pro Tag, alle drei Einnahmen.
- **Arztbericht anzeigen:** helle Druckansicht für A4.
- **Automatische Kopien:** eine Kopie pro Tag im Gerät, die letzten sieben
  bleiben. Vor jedem Zurückspielen wird der aktuelle Stand gesichert
  („Vor der letzten Wiederherstellung"). Schützt vor Fehlbedienung, nicht vor
  Verlust des Geräts.

Einmal pro Woche exportieren. Die App erinnert nach sieben Tagen.

## 6. Aufbau

- **Tracking:** Tagesart, drei Einnahmekarten mit Wirkverlauf und Wirkfenster,
  Tagesabschluss. Einnahme 3 ist optional; leer gilt sie als nicht geplant.
  Wischen wechselt den Tag.
- **Dashboard:** Wirkprofil, Zeitleiste, Kennzahlen, Verhalten pro Fenster,
  mit und ohne Medikament, Vergleich nach Dosis und Tagesart, Nebenwirkungen,
  Datenqualität. Zeitraum: 7 Tage, 30 Tage, alles oder frei.
- **Daten:** Export, Import, Arztbericht, automatische Kopien, Speicher,
  Gefahrenzone.
- **Plus-Knopf:** Schnell erfassen, von überall. Mit Knöpfen für
  „Wirkung spürbar" und „Lässt nach".

## 7. Konventionen

- Der Tag beginnt um 04:00 Uhr. Ein Eintrag um 00:20 zählt zum Vortag, ein
  Fenster von 21:30 bis 00:40 dauert 190 Minuten.
- Wirkbeginn und Wirkende werden aus den Einträgen vorgeschlagen (gestrichelt):
  Beginn ist der erste Eintrag ab „wirkt", Ende der erste Eintrag ab „lässt
  nach" danach. Fehlt das Ende, bleibt die Wirkdauer „offen" und fließt nicht
  in die Kennzahlen ein.
- Sobald du einen Wert änderst oder mit „Jetzt" setzt, gilt er als bestätigt.
- Wirkbeginn kann nie vor der Einnahme liegen, Wirkende nie vor dem Wirkbeginn.
- Ein Eintrag, der mehr als 90 Minuten nach seiner Uhrzeit erfasst wurde, gilt
  als nachgetragen. Der Anteil steht unter Datenqualität.
- „Keine Einnahme" ist ein eigener Zustand. Umschalten löscht nichts.
- Ein Tag gilt als vollständig, wenn jede genommene Einnahme Wirkbeginn und
  Wirkende hat und jedes genutzte Fenster im Tagesabschluss bewertet ist.
- Die Zeitleiste trennt Abschnitte nur, wenn sich die Einzeldosis ändert.

## 8. Grenze

Das Log zeigt gut, **wie** das Medikament über den Tag wirkt: Latenz,
Wirkdauer, Lücke zwischen den Fenstern. Ob es wirkt, kann es nur begrenzt
zeigen: keine Verblindung, und Fenster ohne Medikament sind selten und nicht
zufällig gewählt.

Dosisentscheidungen trifft der Arzt. Das Log ist Input für dieses Gespräch.
