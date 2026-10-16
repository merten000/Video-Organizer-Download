# Video-Organizer

Ein kleines Programm, das Videos aus einem Ordner automatisch nach einem
Namensschema umbenennt und in einen Zielordner einsortiert. Zwei Bereiche:

- **Links:** alle Bedienelemente (Quellordner, Zielordner, Namensschema, KI-Option mit Anbieter und Standbildern, Verschieben/Kopieren, "Vorschau erstellen" und "Übernehmen")
- **Rechts:** nur die Vorschau der geplanten neuen Dateinamen/Ordner — **es wird
  nichts verändert, bis du links auf "Übernehmen" klickst.**
- **Oben links:** die kleinen Buttons "Werkzeuge" (Explorer-Rechtsklick-Menü) und
  "Rechte" (Impressum, Datenschutz, Lizenz, Haftungsausschluss).

## Was du bekommst

Das Programm wird ausschließlich als **`Video-Organizer-Setup.exe`** bereitgestellt —
ein normaler Windows-Installer mit Assistent: fragt nach dem Installationsort
(frei wählbar, kein fester Pfad), legt eine Start-Menü- und Desktop-Verknüpfung
an und trägt das Programm in Windows unter "Apps & Features" / "Programme und
Features" ein. Von dort (oder über die Verknüpfung "Deinstallieren") lässt es
sich jederzeit sauber wieder entfernen. Keine Admin-Rechte nötig, kein Python
nötig. Eine einzelne Programmdatei, Quellcode oder Build-Dateien gehören nicht
zum normalen Download.

## Voraussetzungen

- Für das Programm selbst: nichts weiter — läuft direkt unter Windows.
- Für die KI-Inhaltserkennung (optional, alle anderen Funktionen laufen
  auch ganz ohne sie):
  - **ffmpeg** — zieht die Standbilder (1 bis 3, siehe unten) aus dem Video, die die KI analysiert.
    `Video-Organizer-Setup.exe` lädt ffmpeg bei der Installation automatisch
    herunter (braucht dafür kurz Internet, ca. 100–150 MB) — normalerweise
    musst du dich um nichts weiter kümmern. Falls das mal nicht klappt (z. B.
    weil eine Firewall oder Antivirensoftware den Download blockiert — das
    kommt vor), installiert sich das Programm trotzdem ganz normal, nur die
    KI-Funktion bleibt dann zunächst aus. Sobald du sie das erste Mal
    versuchst, zeigt dir das Programm selbst ein Fenster mit einem Knopf, der
    die ffmpeg-Downloadseite öffnet, und einem zweiten Knopf, der genau den
    Ordner öffnet, in den die zwei heruntergeladenen Dateien (`ffmpeg.exe`,
    `ffprobe.exe`, aus dem `bin`-Unterordner des ZIPs) hineingehören.
  - Ein API-Key **eines** der beiden KI-Anbieter, den du im Programm einträgst:
    Anthropic (Claude) oder Google (Gemini). Den Anbieter wählst du im
    Programm aus; für den Anbieter ohne Key wird nichts benötigt.
  - Eine Internetverbindung während der Nutzung.

Ohne ffmpeg/API-Key funktioniert das Programm ganz normal weiter — die
KI-Stufe wird dann einfach übersprungen (mit Hinweis im Statustext), alle
anderen Namensschema-Optionen (Datum, Nummerierung, eigenes Muster) laufen
komplett offline.

**Falls die KI-Funktion trotz Häkchen und eingetragenem Key nichts tut:**
Das Programm zeigt nach "Vorschau erstellen" jetzt ein Fenster mit dem
genauen Grund an (ffmpeg fehlt, kein API-Key, oder die KI-Anfrage selbst ist
fehlgeschlagen — z.B. falscher Key, kein Guthaben, kein Internet). Einfach
den dort genannten Hinweis befolgen.

## Installation

**Video-Organizer-Setup.exe** doppelklicken → Installations-Assistent
öffnet sich → auf "Weiter" klicken → auf der Seite "Zielverzeichnis
auswählen" entweder den vorgeschlagenen Ordner übernehmen oder über
"Durchsuchen..." einen anderen Ort wählen (interne Platte, externe Platte,
USB-Stick, egal) → "Installieren" klicken → fertig. Das Programm findest du
danach im Startmenü und auf dem Desktop.

**Deinstallieren:** Windows-Einstellungen → "Apps" bzw. "Programme und
Features" → "Video-Organizer" suchen → "Deinstallieren". Alternativ über
die Verknüpfung "Deinstallieren" im Startmenü-Ordner "Video-Organizer".

## Benutzung

1. Programm starten (über Startmenü/Desktop-Verknüpfung nach der
   Installation; ggf. Windows-Sicherheitshinweis "trotzdem ausführen"
   bestätigen — siehe Hinweis unten).
2. Links **Quellordner** wählen (wo deine Videos liegen).
3. Links **Zielordner** wählen (wohin sortiert werden soll — beliebiger Pfad).
4. Namensschema anpassen oder Standard lassen (siehe Platzhalter unten).
5. Optional: "In Unterordner nach Datum gruppieren" aktivieren.
6. Optional: KI-Inhaltserkennung aktivieren, Anbieter (Anthropic oder Google Gemini) wählen und den passenden API-Key eintragen.
7. **"Vorschau erstellen"** klicken — rechts erscheint, was passieren würde.
8. Prüfen, ob die neuen Namen/Ordner passen.
9. **"Übernehmen"** klicken — erst jetzt werden Dateien wirklich verschoben/umbenannt (nach Sicherheitsabfrage).

## Namensschema — Platzhalter

| Platzhalter | Bedeutung | Beispiel |
|---|---|---|
| `{date}` | Datum der Datei (Änderungsdatum) | `2026-08-14` |
| `{time}` | Uhrzeit der Datei | `14-32-10` |
| `{index}` | Fortlaufende Nummer (3-stellig) | `001` |
| `{original}` | Ursprünglicher Dateiname ohne Endung | `IMG_0001` |
| `{ai}` | KI-Schlagwort (nur wenn aktiviert, sonst leer) | `Sonnenuntergang-am-See_` |
| `{ext}` | Dateiendung | `.mp4` |

Standard-Schema: `{ai}{date}_{index}{ext}` → z. B. `Sonnenuntergang-am-See_2026-08-14_001.mp4`
(ohne aktivierte KI bzw. ohne Ergebnis fällt `{ai}` einfach weg: `2026-08-14_001.mp4`)

Die KI-Beschreibung steht bewusst **vorne** im Dateinamen — so ist beim
Durchblättern im Explorer sofort erkennbar, was im Video zu sehen ist,
statt nur ein Datum.

Eigene Beispiele:
- `Stream_{date}_{index}{ext}` → `Stream_2026-08-14_001.mp4`
- `{ai}{original}{ext}` → `Sonnenuntergang-am-See_IMG_0001.mp4`

(Wichtig: `{ai}` bringt seinen trennenden Unterstrich selbst mit, aber nur
danach, nicht davor — darum am besten an den Anfang des Schemas setzen,
so wie es die Standard-Vorlage macht.)

Bei Namenskollisionen (zwei Dateien würden denselben Namen bekommen) hängt
das Programm automatisch `_2`, `_3` usw. an — es wird nie eine vorhandene
Datei überschrieben.

## Unterstützte Videoformate

`.mp4 .mov .mkv .avi .wmv .flv .webm .m4v .mts .ts`

## Wichtiger Hinweis zu Windows-Warnungen

Weder `Video-Organizer-Setup.exe` noch das damit installierte Programm sind digital
signiert (das kostet Geld/eine
Firmen-Zertifizierung, die es für ein privates Tool wie dieses nicht gibt).
Windows SmartScreen oder dein Virenscanner können deshalb beim ersten Start
eine Warnung anzeigen ("Unbekannter Herausgeber" o. Ä.). Das ist bei
unsignierten, selbst gebauten Programmen üblich — es ist aber KEIN Beweis
dafür, dass die Datei sauber ist, sondern lediglich ein Hinweis, dass
Windows die Herkunft nicht bestätigen kann. Bitte vor dem Ausführen selbst
prüfen, statt die Warnung einfach wegzuklicken:

- Nur die Datei `Video-Organizer-Setup.exe` verwenden und möglichst aus der
  offiziellen Quelle beziehen. Die unveränderte Weitergabe durch andere ist
  laut Lizenz erlaubt — wenn du die Datei nicht direkt von dort hast,
  vergleiche deshalb unbedingt die Prüfsumme (nächster Punkt).
- Falls im Release ein SHA-256-Prüfwert angegeben ist, die heruntergeladene
  Datei damit vergleichen (z. B. mit `certutil -hashfile Video-Organizer-Setup.exe SHA256`
  unter Windows).

Erst wenn Quelle und ggf. Prüfsumme passen, die Warnung über "Weitere
Informationen" bzw. "Trotzdem ausführen" bestätigen.

## Sicherheit

- Standardmäßig ist "Verschieben" ausgewählt (Originaldatei landet im
  Zielordner, ist am alten Ort weg). Wähle "Kopieren", wenn das Original
  am alten Ort erhalten bleiben soll.
- Die Vorschau greift nie auf die Festplatte schreibend zu — erst
  "Übernehmen" führt Änderungen aus, und selbst dann kommt vorher eine
  Sicherheitsabfrage.

## Die KI-Inhaltserkennung: Anbieter, Kosten & eigener API-Key

Die KI-Funktion ist komplett optional und hat **nichts mit deinem Claude/
Cowork-Konto oder dessen Nutzung zu tun**. Sie braucht einen eigenen,
separaten Zugang bei **einem** der beiden Anbieter (Auswahl im Programm):

**Anthropic (Claude):**

1. Auf [console.anthropic.com](https://console.anthropic.com) einen Account anlegen (falls noch nicht vorhanden).
2. Dort unter "Billing" ein kleines Guthaben aufladen (Anthropic verlangt
   ein Mindestguthaben, oft ab 5 USD, das dann verbraucht wird — kein Abo).
3. Unter "API Keys" einen neuen Key erstellen (eine lange Zeichenkette,
   beginnt mit `sk-ant-...`) und kopieren.
4. Diesen Key im Video-Organizer in das Feld unter der KI-Checkbox einfügen.

**Google (Gemini):**

Wichtig vorab — das regelt Google in den
[Gemini-API-Bedingungen](https://ai.google.dev/gemini-api/terms) (Stand
28.04.2026), bitte selbst nachlesen:

- Gemini API und Google AI Studio sind laut Google für professionelle bzw.
  geschäftliche Entwicklerzwecke gedacht, nicht für Verbrauchernutzung.
- Wenn ein API-Client Nutzern im **EWR, der Schweiz oder dem Vereinigten
  Königreich** bereitgestellt wird, dürfen laut Google **nur Paid Services**
  verwendet werden.
- Die Gemini API ist laut Google nur dann ein Paid Service, wenn sie über ein
  **Cloud-Projekt mit aktivem Abrechnungskonto (Billing)** genutzt wird.
- Laut Google musst du **mindestens 18 Jahre alt** sein, um die Gemini API
  zu nutzen, und sie darf nicht Teil eines Programms sein, das sich an
  unter 18-Jährige richtet oder von ihnen voraussichtlich genutzt wird. Die
  Gemini-Funktion dieses Programms ist deshalb nur für Personen ab 18 Jahren
  gedacht; beim Aktivieren bestätigst du das mit „OK“.
- Für Nutzer im EWR/Schweiz/Vereinigten Königreich gelten laut Google die
  Datenregeln der bezahlten Dienste (keine Nutzung deiner Eingaben zur
  Produktverbesserung) auch für kostenlose Kontingente. Außerhalb dieser
  Regionen können Eingaben in der unbezahlten Nutzung laut Google zur
  Verbesserung von Google-Produkten verwendet werden.

Wenn du in Deutschland/EWR arbeitest, nutze Gemini deshalb nur mit einem Key
aus einem Google-Cloud-Projekt mit aktivem Billing — einen reinen
AI-Studio-Gratis-Zugang ohne Billing richtet dieses Programm nicht ein und
empfiehlt ihn dafür nicht. Wer das nicht möchte, nutzt stattdessen Anthropic.

1. In [Google AI Studio](https://aistudio.google.com) mit einem Google-Konto anmelden.
2. Einen Gemini-API-Key erstellen — und zwar für ein Cloud-Projekt, für das
   Billing aktiviert ist (siehe oben).
3. Im Video-Organizer den Anbieter "Google (Gemini)" wählen und den Key in
   das Feld unter der KI-Checkbox einfügen.

Das Programm weist dich beim Aktivieren der KI-Funktion ebenfalls auf diese
Punkte hin. Preise und Kontingente legt Google fest und kann sie ändern.

Für beide Anbieter gilt: Der Key wird nur für die laufende Sitzung im
Arbeitsspeicher gehalten, oder — nur wenn du "Speichern" wählst — im
Windows-Anmeldeinformations-Manager deines Rechners (getrennt je Anbieter).

**Welche Bilder werden analysiert?** Das Programm zieht die Standbilder
selbst mit ffmpeg aus dem Video — unabhängig vom gewählten Anbieter. Im
Feld "Standbilder" stehen *Auto*, *1*, *2* und *3* zur Wahl (maximal 3):

- **Auto** (Standard): Bis 30 Sekunden Länge 1 Bild aus der Mitte, bis
  120 Sekunden 2 Bilder (bei 33 % und 66 %), darüber 3 Bilder (bei 20 %,
  50 % und 80 %).
- **1 / 2 / 3**: feste Anzahl, gleichmäßig über das Video verteilt.

Mehrere Bilder erkennen den Inhalt besser, wenn sich ein Video verändert,
aber es werden entsprechend mehr Bilddaten übertragen. Bei Dateien, deren
Länge sich nicht ermitteln lässt, nimmt das Programm nur 1 Bild bei
Sekunde 3. Es werden keine Dateinamen oder Pfade mitgeschickt.

**Kosten:** Das Programm nutzt bei Anthropic bewusst das günstigste
Claude-Modell (Haiku), bei Google ein kleines, günstiges Gemini-Modell
(Flash-Lite). Je Video fallen bis zu 3 Bilder an, also etwas mehr als bei
einem Bild. Die genauen, aktuellen Preise legen die Anbieter selbst fest
(siehe deren Preisseiten, z. B.
[anthropic.com/pricing](https://www.anthropic.com/pricing)) — schau vor
größeren Mengen kurz dort nach oder probier es zuerst mit 1–2 Videos aus.

Ohne eingetragenen Key bleibt diese Funktion einfach inaktiv (kostenlos),
alles andere im Programm funktioniert unabhängig davon ganz normal.

## Lizenz & Rechtliches

Video-Organizer darf kostenlos privat und geschäftlich genutzt und
unverändert weitergegeben werden, solange Lizenztext und Rechtehinweise
vollständig erhalten bleiben. Veränderungen am Programm, Quellcode oder an den
mitgelieferten Dateien sind nur nach vorheriger schriftlicher Rücksprache
gestattet. Maßgeblich ist `LIZENZ.txt`. Nach der Installation liegen im
Programmordner außerdem `IMPRESSUM.txt`, `DATENSCHUTZ.txt`,
`DISCLAIMER.PDF.pdf` (Haftungsausschluss; Textfassung: `DISCLAIMER.md`),
`THIRD-PARTY-NOTICES.md` (Drittanbieter-Hinweise) und die zugehörigen
Lizenztexte `LICENSE-*.txt`. Die wichtigsten davon erreichst du im Programm
über den Button „Rechte“.

## Weiterentwicklung

Das Programm ist bewusst als Basis gebaut ("erstmal da sein, Verbesserungen
später"). Mögliche spätere Erweiterungen: Mehrfachauswahl mehrerer
Quellordner, Undo-Funktion, Speichern von Voreinstellungen (inkl. API-Key).
