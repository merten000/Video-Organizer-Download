# Bilder-Erzaehler

Ein kleines Programm, das aus bis zu 20 Bildern ein kurzes Video macht: die
Bilder laufen nacheinander durch, dazu laeuft eine gesprochene Erzaehlung
als Ton - entweder selbst getippt oder direkt von einer KI aus den Bildern
erstellt.

**Dies ist eine fruehe Version (0.x) eines privaten Hobbyprojekts.** Es gibt
KEINE Variante ganz ohne KI - das Vorlesen des Textes ist selbst schon eine
KI-Funktion und braucht immer einen eigenen API-Key.

## Screenshots

![Hauptfenster mit OpenAI](screenshots/1%20Programm%20start.PNG)

*Hauptfenster: KI-Anbieter und Stimme waehlen, Erzaehltext festlegen, Videoformat einstellen.*

![Google (Gemini) und Hochformat](screenshots/3%20Stimme%20und%20API%20Key%20einstellen.PNG)

*Mit Google (Gemini) als Anbieter und Hochformat fuer TikTok, YouTube Shorts und Reels.*

![Bilderliste](screenshots/4%20Bilder%20Hinzugef%C3%BCgt.PNG)

*Bis zu 20 Bilder in frei waehlbarer Reihenfolge.*

## Installation

`Bilder-Erzaehler-Setup.exe` doppelklicken -> Installations-Assistent ->
Zielordner waehlen (frei waehlbar) -> Installieren. Verknuepfungen liegen
danach im Startmenue und auf dem Desktop. Deinstallation ueber
Windows-Einstellungen -> "Apps" -> "Bilder-Erzaehler", oder ueber die
Verknuepfung "Deinstallieren" im Startmenue-Ordner.

### ffmpeg (wird fuers Video-Erstellen gebraucht)

Bilder-Erzaehler nutzt intern ffmpeg, um aus den Bildern + der Sprachausgabe
das fertige Video zusammenzubauen. Damit die Setup-Datei zum Download klein
bleibt, ist ffmpeg NICHT im Installer enthalten. Stattdessen prueft das
Programm beim ersten Klick auf "Video erstellen ...", ob ffmpeg schon
vorhanden ist - falls nicht, fragt es nach, ob es automatisch heruntergeladen
und eingerichtet werden soll (einmalig, braucht eine Internetverbindung).
Der Download ist gross (mehrere hundert Megabyte) und kann je nach
Verbindung einige Minuten dauern; eine genaue Groessenangabe steht bewusst
nicht in der Oberflaeche, damit sie nicht veraltet. Heruntergeladen wird
immer genau der im Programm fest hinterlegte ffmpeg-Build (feste
Download-Adresse), der vor der Installation ueber eine SHA-256-Pruefsumme und
die Dateigroesse auf Echtheit geprueft wird. Klappt der Download beim
ersten Versuch nicht (z. B. wegen eines kurzen Internet-Aussetzers), zeigt
das Programm eine verstaendliche Fehlermeldung und man kann es beim
naechsten Klick auf "Video erstellen ..." einfach noch einmal versuchen -
ganz ohne selbst etwas im Explorer herunterladen oder entpacken zu muessen.
ffmpeg wird dabei in den Installationsordner von Bilder-Erzaehler gelegt
und nicht rechnerweit eingerichtet; das Einrichten faellt also einmalig
pro Installation an.

## Bedienung

1. Programm oeffnen.
2. Ueber "Hinzufuegen ..." bis zu 20 Bilder auswaehlen. Mit "Nach oben"/"Nach
   unten" laesst sich die Reihenfolge aendern, in der sie im Video zu sehen
   sein werden - "Entfernen" nimmt ein ausgewaehltes Bild wieder raus.
3. Oben einen KI-Anbieter auswaehlen (OpenAI oder Google), darunter eine
   **Stimme** auswaehlen (jeder Anbieter hat mehrere zur Auswahl, mit
   kurzem Stil-Hinweis wie "warm" oder "freundlich") und den eigenen
   API-Key eintragen (siehe unten, wie man den bekommt). Wie natuerlich/
   menschlich eine Stimme klingt, ist Geschmackssache - am besten ein
   paar ausprobieren, bis eine passt.
4. Erzaehltext festlegen - zwei Moeglichkeiten (in allen Textfeldern gibt es
   ausserdem ein Rechtsklick-Menue mit Ausschneiden, Kopieren, Einfuegen und
   Alles auswaehlen; Strg+C / Strg+V / Strg+X funktionieren wie gewohnt):
   - **"Text selbst eingeben ..."**: du tippst den kompletten Text, der
     vorgelesen werden soll.
   - **"Text von KI aus den Bildern erstellen lassen"**: die KI schaut sich
     die ausgewaehlten Bilder an und schlaegt selbst einen zusammenhaengenden
     Erzaehltext vor - der laesst sich danach im selben Fenster noch
     anpassen, bevor man ihn uebernimmt.
5. Unter **Videoformat** das Ausgabeformat waehlen: Standard (Bildschirm,
   1280x720, wie bisher), YouTube (16:9, 1920x1080), TikTok / YouTube Shorts /
   Reels (9:16, 1080x1920), Instagram (Feed, 4:5, 1080x1350), Facebook
   (Feed, 4:5, 1080x1350), WhatsApp-Status (9:16, 1080x1920) oder
   **Benutzerdefiniert** mit eigener Breite und Hoehe (gerade Zahlen von 128
   bis 3840). Die Bilder werden nie verzerrt, sondern proportional
   eingepasst und bei abweichendem Seitenverhaeltnis mit Balken aufgefuellt.
   Die Plattformen aendern ihre Empfehlungen gelegentlich; die Werte stehen
   gesammelt in `video_profiles.py` und lassen sich dort leicht anpassen.
6. Auf "Video erstellen ..." klicken, einen Speicherort fuer die
   MP4-Datei waehlen und warten (kann je nach Textlaenge und Internet ein
   paar Sekunden bis wenige Minuten dauern - der Ton wird zuerst erzeugt,
   danach das Video zusammengebaut).
7. Fertig - die Bilder laufen im Video automatisch gleich lang durch: die
   Gesamtlaenge der Sprachausgabe wird dabei gleichmaessig auf die Anzahl
   der Bilder verteilt.

**Format des erzeugten Videos (Social Media):** Jedes neu erzeugte Video ist
eine MP4-Datei mit H.264 (`yuv420p`, High-Profil, konstant 30 Bilder pro
Sekunde, Qualitaetscodierung) und AAC-Ton (AAC-LC, 48 kHz, Stereo, etwa
192 kbit/s). Die Datei ist fuer schnelles Abspielen/Hochladen vorbereitet
(Faststart) und uebernimmt keine Metadaten der Quelldateien. Das ist der
Standard, den TikTok, Instagram, Facebook, YouTube und WhatsApp am
zuverlaessigsten verarbeiten. Nach dem Erzeugen prueft das Programm mit
ffprobe, dass Video und Ton genau diesem Standard und der gewaehlten
Aufloesung entsprechen, bevor die fertige Datei gespeichert wird. Ob eine
Plattform das Video am Ende annimmt, haengt auch von deren eigenen Regeln
ab (z. B. Laengen- und Groessengrenzen); die Plattformen aendern ihre
Anforderungen gelegentlich.

## API-Key besorgen (optional waehlbar, aber fuer die Sprachausgabe zwingend noetig)

**Bei OpenAI:**
1. Eigenen API-Key auf [platform.openai.com](https://platform.openai.com/api-keys)
   erstellen (eigenes Konto noetig, dort auch ein kleines Guthaben
   hinterlegen - dafuer wird eine Visa-, Mastercard- oder Amex-Karte
   gebraucht, Debitkarten gehen auch; die genauen aktuell akzeptierten
   Zahlungsmethoden stehen in OpenAIs eigenen Abrechnungsbedingungen).
   **Wichtig:** das ist ein Key von OpenAI (beginnt mit `sk-` oder
   `sk-proj-`), nicht von Anthropic (die Keys von dort beginnen mit
   `sk-ant-` und funktionieren hier nicht). Auch wenn man sich mit
   demselben OpenAI-Konto einloggen kann wie bei ChatGPT: ChatGPT-Abo/
   -Guthaben und das API-Guthaben fuer die Programmierschnittstelle werden
   getrennt abgerechnet - ein ChatGPT-Abo allein reicht hier also nicht,
   es braucht zusaetzlich eigenes API-Guthaben.

**Bei Google:**
1. Eigenen API-Key auf [aistudio.google.com/apikey](https://aistudio.google.com/apikey)
   erstellen (eigenes Google-Konto noetig). Die hier genutzten Modelle
   (`gemini-3.8-flash` fuers Bild-Verstehen, `gemini-3.1-flash-tts-preview`
   fuer die Sprachausgabe) haben laut Googles eigener Preisseite (Link
   unten) einen kostenlosen Kontingent-Anteil, allerdings mit Einschraenkungen
   (u. a. wird der Inhalt dabei laut Google zur Produktverbesserung genutzt,
   anders als beim kostenpflichtigen Kontingent) - fuer produktiven Einsatz
   mit hoeheren Limits kann zusaetzlich ein Rechnungskonto mit
   Zahlungsmethode noetig sein. Ob, in welchem Umfang und zu welchen
   Bedingungen es kostenlose Kontingente gibt, aendert sich bei Google
   erfahrungsgemaess gelegentlich - massgeblich ist immer der aktuelle Stand
   auf der Google-Gemini-Preisseite (Link unten), nicht diese Beschreibung.

Den Key traegst du direkt im Hauptfenster ein. Er ist dort verborgen
(Sternchen); "Anzeigen" blendet ihn bei Bedarf ein. Ohne weiteres Zutun wird
er nirgends gespeichert und nur fuer die jeweilige Sitzung verwendet (beim
Neustart des Programms ist er wieder weg).

**Optional: Key speichern.** Nur wenn du auf "Speichern" klickst, legt das
Programm den Key im Windows-Anmeldeinformationsspeicher (Credential Manager)
deines Windows-Benutzerkontos ab - nicht in einer Textdatei, JSON- oder
INI-Datei. OpenAI und Google werden getrennt gespeichert; beim Wechsel des
Anbieters wird dessen gespeicherter Key eingetragen. Eine Zeile unter dem
Feld zeigt, ob fuer den gewaehlten Anbieter ein Key gespeichert ist.
"Loeschen" entfernt den gespeicherten Key wieder. Hinweis: Andere Programme
desselben Windows-Benutzerkontos koennen diesen Speicher grundsaetzlich
ebenfalls auslesen. Beim Deinstallieren wirst du gefragt, ob gespeicherte
API-Schluessel ebenfalls geloescht werden sollen (Standard: Nein).

**Kosten:** werden direkt zwischen deinem eigenen Konto beim gewaehlten
Anbieter und diesem Anbieter abgerechnet - nicht Teil dieses Programms. Die
genauen Preise haengen vom gewaehlten Anbieter/Modell ab und aendern sich
gelegentlich, aktuelle Zahlen stehen auf der
[OpenAI-Preisseite](https://openai.com/api/pricing/) bzw. der
[Google-Gemini-Preisseite](https://ai.google.dev/gemini-api/docs/pricing).

## Optionales Windows-Kontextmenue

Unter "Tools" gibt es die Einstellung "Bilder-Erzaehler im
Windows-Kontextmenue anzeigen" (standardmaessig aus, nur in der installierten
Version). Wenn eingeschaltet, erscheint beim Rechtsklick auf Bilddateien (PNG,
JPG/JPEG, BMP, GIF, WEBP - nicht bei Ordnern) der Eintrag "Mit
Bilder-Erzaehler oeffnen"; markierte Bilder werden beim Start uebernommen
(hoechstens 20; Windows blendet den Eintrag allerdings bei mehr als 15
markierten Dateien aus). Es werden keine Dateizuordnungen geaendert - Bilder oeffnen
sich weiter mit ihren bisherigen Programmen. Der Eintrag liegt nur im
eigenen Benutzerbereich der Registry (ohne Administratorrechte); beim
Ausschalten und beim Deinstallieren werden ausschliesslich diese eigenen
Eintraege entfernt.

## Unterstuetzte Bildformate

PNG, JPG/JPEG, BMP, GIF, WEBP - alles, was ueblicherweise als Foto oder
Grafik vorliegt (bei animierten GIFs wird nur die erste Einzelbild-Ebene
verwendet). Das Programm wandelt jedes ausgewaehlte Bild vor der weiteren
Verarbeitung intern automatisch in ein einheitliches Format um - sowohl vor
dem Zusammenbauen des Videos als auch, falls genutzt, vor dem Verschicken an
die KI zum automatischen Text-Erstellen. Das sorgt dafuer, dass auch Formate
wie BMP zuverlaessig funktionieren, selbst wenn ein einzelner KI-Anbieter
das Ausgangsformat eigentlich nicht direkt unterstuetzen wuerde.
Unterschiedliche Bildgroessen werden im Video automatisch passend eingepasst
(nicht verzerrt).

## Hinweis zum Datenschutz

Die vollstaendige Datenschutzerklaerung steht in `DATENSCHUTZ.md` (im
Programm unter "Datenschutz & Rechtliches"). Kurz gesagt:

Vor der ersten Uebertragung in einer Sitzung zeigt das Programm einen
Hinweisdialog (Anbieter, Inhalt, moegliche personenbezogene Daten, Kosten)
mit "Fortfahren" / "Abbrechen"; ohne "Fortfahren" wird nichts gesendet. Die
Zustimmung wird nicht gespeichert.

Der Entwickler erhebt keine Daten: keine Konten, kein Tracking, keine
Statistik, keine eigene Protokolldatei. Die ausgewaehlten Bilder (nur bei
"Text von KI ... erstellen lassen") und der Erzaehltext (immer, fuer die
Sprachausgabe) gehen **direkt von deinem Rechner an den gewaehlten
Anbieter - OpenAI oder Google**, zusammen mit deinem eigenen API-Key. Die
Bilder werden vorher in ein neues PNG umgewandelt (bei sehr grossen Bildern
verkleinert; Metadaten der Originaldatei werden nicht uebernommen). Fuer
die weitere Verarbeitung gelten die eigenen Datenschutz-/Nutzungsbedingungen
des Anbieters. Beim kostenlosen Google-Kontingent kann der Inhalt laut
Google zur Produktverbesserung genutzt werden (siehe "API-Key besorgen").

Der API-Key wird nur dann dauerhaft gespeichert, wenn du ausdruecklich auf
"Speichern" klickst - dann im Windows-Anmeldeinformationsspeicher deines
Benutzerkontos, nie in einer normalen Datei (siehe oben). Temporaere
Arbeitsdateien werden nach dem Export wieder geloescht.

Beim einmaligen Einrichten von ffmpeg (nur nach deiner Bestaetigung)
verbindet sich das Programm mit GitHub (`github.com`, Weiterleitungen zu
`githubusercontent.com`), um den hinterlegten ffmpeg-Build herunterzuladen;
dabei wird, wie bei jedem Download, deine IP-Adresse uebermittelt. Bilder,
Text oder Key werden dafuer nicht verschickt.

## Hinweis zu Windows-Warnungen

`Bilder-Erzaehler-Setup.exe` ist nicht digital
signiert. Eine SmartScreen-Warnung beim ersten Start ist deshalb normal bei
selbst gebauten Programmen wie diesem und fuer sich genommen kein Hinweis
auf Schadsoftware - umgekehrt ist eine fehlende Signatur aber auch keine
Garantie fuer Unbedenklichkeit. Der Quellcode ist nicht
oeffentlich.

## Rechtliche Hinweise

Zum Programm gehoeren `IMPRESSUM.md`, `DATENSCHUTZ.md`, `DISCLAIMER.md`
(Haftungsausschluss und Nutzungshinweise) und `THIRD-PARTY-NOTICES.md`
(Drittanbieter-Hinweise mit den beiliegenden Lizenztexten
`LICENSE-Python.txt`, `LICENSE-Tcl-Tk.txt`, `LICENSE-Pillow.txt`,
`LICENSE-OpenSSL.txt` und `LICENSE-PyInstaller.txt`). Im Programm sind sie unter "Datenschutz &
Rechtliches" lesbar.
