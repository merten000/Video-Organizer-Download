# Drittanbieter-Hinweise

## ffmpeg / ffprobe

Video-Organizer bringt **keine** ffmpeg-Programmdateien fest mit sich mit.
Stattdessen lädt `download_ffmpeg.bat` bei der Installation ffmpeg
automatisch von einer öffentlichen Windows-Build-Quelle (gyan.dev bzw. dessen GitHub-Spiegel GyanD/codexffmpeg; keine Build-Quelle des FFmpeg-Projekts selbst) herunter — dabei
wird eine fest eingetragene, datierte Release-Version (kein rotierender
"latest"-Marker) verwendet und die Datei per SHA-256-Prüfsumme gegen den
erwarteten Wert geprüft, bevor sie verwendet wird:

- Quelle: https://github.com/GyanD/codexffmpeg/releases (ffmpeg-Builds von
  gyan.dev, Übersicht: https://www.gyan.dev/ffmpeg/builds/). Fest
  eingetragen ist das dauerhaft bereitgestellte Release 8.1.2, Paket
  `ffmpeg-8.1.2-essentials_build.zip` (siehe `FFMPEG_VERSION`/`FFMPEG_ASSET`/
  `EXPECTED_SHA256` in `download_ffmpeg.bat`). Frühere Versionen dieses
  Projekts nutzten kurzlebige „autobuild“-Releases eines anderen Anbieters;
  diese werden nach einiger Zeit gelöscht und lieferten nur noch einen
  Download-Fehler (HTTP 404).
- Projekt: https://ffmpeg.org/ (Quellcode: https://ffmpeg.org/download.html)
- Lizenz: ffmpeg steht unter der GNU General Public License (GPL) bzw. je
  nach eingeschlossenen Komponenten der LGPL — der genaue Lizenztext liegt
  dem heruntergeladenen ffmpeg-Paket selbst bei (`LICENSE` im ZIP von
  gyan.dev; dieser Build ist GPL-v3-lizenziert), zusätzlich hier: https://ffmpeg.org/legal.html

Da ffmpeg bei jeder Installation frisch von dieser öffentlichen Build-Quelle bezogen
wird (statt fest in dieses Paket eingebettet zu sein), verteilt
Video-Organizer selbst keine ffmpeg-Binärdateien weiter — der Download
passiert direkt auf dem Rechner der Person, die Video-Organizer installiert.

Nach dem Download legt `download_ffmpeg.bat` den Lizenztext des
installierten ffmpeg-Builds zusätzlich als `FFMPEG-LICENSE.txt` direkt neben
`ffmpeg.exe`/`ffprobe.exe` im Zielordner ab (vorher wurde die im ZIP
enthaltene `LICENSE` beim Aufräumen des entpackten Downloads sofort
wieder gelöscht) — die Lizenz liegt damit dauerhaft griffbereit bei den
Programmdateien.

**Hinweis zur Weiterverteilung von ffmpeg (betrifft ausschließlich ffmpeg):**
Dieser Absatz bezieht sich nur auf ffmpeg und dessen eigene Lizenzbedingungen.
Wer ffmpeg.exe/ffprobe.exe selbst weiterverteilt (z. B. in ein eigenes ZIP
packt), verteilt damit ein GPL-lizenziertes Programm weiter und muss die dafür
geltenden ffmpeg-/GPL-Bedingungen beachten: Lizenzdatei mitliefern (liegt im
ffmpeg-ZIP bei), auf die Lizenz hinweisen und idealerweise einen Link zur
Quelle/zum Quellcode beifügen (siehe Links oben). Eine kurze Zeile wie

> Dieses Paket enthält ffmpeg (https://ffmpeg.org), lizenziert unter der
> GNU GPL. Lizenztext: siehe beiliegende LICENSE bzw. https://ffmpeg.org/legal.html

im Begleittext reicht dafür in der Regel aus.

**Daraus ergibt sich keine Änderung der Lizenz von Video-Organizer.**
Für Video-Organizer (Programm, Installer und mitgelieferte Dateien) gilt
ausschließlich die beiliegende Lizenz (`LIZENZ.txt`): unveränderte Weitergabe
ist erlaubt, solange Lizenztext und alle Urheber-/Rechtehinweise (dazu gehören
auch diese Hinweise und die `LICENSE-*.txt`-Dateien) vollständig erhalten
bleiben.

## In `Video-Organizer.exe` eingebettete Komponenten

`Video-Organizer.exe` wird mit PyInstaller gebaut und enthält neben dem eigenen
Code (`video_organizer.py`, `renamer_logic.py`, `credential_store.py`,
`app_version.py`) die folgenden Fremdbestandteile. Die Liste wurde aus dem
Inhalt eines echten Test-Builds (Commit a2a50f3; PyInstaller 6.21.0,
Python 3.14.6, Windows) ermittelt, nicht geschätzt. Der Programm-Code selbst
verwendet keine weiteren Pakete außer der Python-Standardbibliothek
(inkl. Tkinter, `ctypes`, `urllib`).

| Komponente | Version | Wo enthalten | Lizenz | Lizenztext |
|---|---|---|---|---|
| Python (Laufzeit und Standardbibliothek) | 3.14.6 | `python314.dll`, Standardbibliothek, Erweiterungsmodule (`_socket`, `_ssl`, `_hashlib`, `_ctypes`, `_decimal`, `_queue`, `_bz2`, `_lzma`, `_zstd`, `select`, `unicodedata`, `_tkinter`) | Python Software Foundation License (zusätzliche Bedingungen für den Windows-Build siehe unten) | `LICENSE-Python.txt` |
| Tcl/Tk (für Tkinter) | 8.6.15 | `tcl86t.dll`, `tk86t.dll`, Skriptbibliotheken (`_tcl_data`, `_tk_data`) | Tcl/Tk-Lizenz (BSD-artig) | `LICENSE-Tcl-Tk.txt` |
| OpenSSL | 3.5.7 | `libcrypto-3.dll`, `libssl-3.dll` (HTTPS-Verbindungen zu den KI-Anbietern) | Apache License 2.0 | `LICENSE-OpenSSL.txt` |
| PyInstaller (inkl. Bootloader) | 6.21.0 | Bootloader und Laufzeit-Module in der EXE | GNU GPL v2 oder später mit Bootloader-Ausnahme (Einbetten in Programme beliebiger Lizenz ausdrücklich erlaubt) | `LICENSE-PyInstaller.txt` |
| zlib | 1.3.1 | `zlib1.dll` | zlib-Lizenz | `LICENSE-Weitere-Komponenten.txt` |
| libffi | (aus dem Python-Paket) | `libffi-8.dll` (für `_ctypes`) | MIT-Lizenz | `LICENSE-Weitere-Komponenten.txt` |
| libmpdec (mpdecimal) | 4.0.0 | in `_decimal.pyd` | BSD-2-Klausel | `LICENSE-Weitere-Komponenten.txt` |
| Zstandard (libzstd) | (aus dem Python-Paket) | in `_zstd.pyd` | BSD-Lizenz | `LICENSE-Weitere-Komponenten.txt` |
| bzip2 / libbzip2 | 1.0.8 | in `_bz2.pyd` | bzip2-Lizenz (BSD-artig) | `LICENSE-Weitere-Komponenten.txt` |
| liblzma (xz) | (aus dem Python-Paket) | in `_lzma.pyd` | gemeinfrei (public domain) | – |
| Microsoft Visual C++ Runtime | – | `VCRUNTIME140.dll` | Microsoft Distributable Code; Bedingungen im Abschnitt „Additional Conditions for this Windows binary build“ der Python-Lizenz | `LICENSE-Python.txt` (Abschnitt „Additional Conditions for this Windows binary build“) |

**Zu `VCRUNTIME140.dll` (Microsoft Visual C++ Runtime):** Die Datei stammt laut
Build-Protokoll aus dem Installationsordner von Python 3.14.6 für Windows
(`...\Python\pythoncore-3.14-64\VCRUNTIME140.dll`) und wurde von PyInstaller
unverändert in die EXE übernommen. Die `LICENSE.txt` dieser Python-Windows-
Installation enthält den Abschnitt „Additional Conditions for this Windows
binary build“, der den von Microsoft stammenden Distributable Code des
Windows-Builds regelt (u. a.: Hinweise von Microsoft nicht verändern, Microsoft-
Marken nicht verwenden, nur für Microsoft-Plattformen, nicht in schädlichen
oder täuschenden Programmen). `LICENSE-Python.txt` ist genau diese
`LICENSE.txt` im Originaltext, unverändert und vollständig; sie enthält außerdem
die Lizenztexte weiterer mit Python gelieferter Komponenten (u. a. bzip2,
libffi, Zstandard, Apache-2.0-Text, Tcl/Tk). Die Datei nennt `VCRUNTIME140.dll`
nicht ausdrücklich beim Namen; die Zuordnung beruht darauf, dass die DLL Teil der
Python-Windows-Installation ist und von Microsoft stammt. Die Microsoft-
Redistributable-Bedingungen selbst liegen nicht als eigener Text bei, die
Python-Lizenz verweist dafür auf die Dokumentation von Microsoft.

Nicht eingebettet: Die Universal C Runtime (UCRT, `ucrtbase.dll` und die
`api-ms-win-*`-Dateien) ist in diesem Build **nicht** in der EXE enthalten; sie
stammt vom Windows-System. Weitere Python-Pakete (z. B. Pillow) sind nicht
enthalten — Pillow wird nur vom internen Hilfsskript `make_icon.py` benötigt,
das nicht ausgeliefert wird.

Die Lizenztexte liegen als Originaltexte im Installationsordner neben dieser
Datei (`LICENSE-*.txt`) und wurden nicht umformuliert. Für diese Fremdkomponenten
gelten ausschließlich ihre eigenen Lizenzen; die Lizenz von Video-Organizer
(`LIZENZ.txt`) erweitert oder ersetzt sie nicht.
