# CatMerge – Merge Media Files

**CatMerge** ist ein Nautilus‑Skript, mit dem du ausgewählte Mediendateien direkt im Dateimanager zusammenführen kannst.  
Es versucht zunächst, die Dateien **verlustfrei ohne Neukodierung** zu mergen (`ffmpeg -c copy`). Falls das nicht möglich ist
(z. B. bei variabler Bitrate oder inkonsistenten Streams), fragt das Skript, ob eine **Neukodierung** durchgeführt werden soll,
zeigt einen Fortschrittsdialog an und schreibt die Ausgabedatei.

---

## Funktionen

- Zusammenfügen mehrerer ausgewählter **Video- und Audiodateien**
- **Verlustfreies** Mergen, wenn möglich (Stream Copy)
- Automatische **Erkennung**, ob Neukodierung nötig ist (mit Bestätigungsdialog)
- **Fortschrittsanzeige** via `yad` (pulsierend bei Stream‑Copy)
- **Speicherplatzprüfung** vor Start
- **Natürliche Sortierung** nach Dateinamen (`_001`, `_002`, …)
- Ausgabedatei erhält **Erstellungsdatum** der ersten Datei im Namen
- **Überschreibabfrage** mit Timeout (Standard: überschreiben)
- **Zielordner- & Dateinamen‑Dialog** (mit 5 s Timeout und Standardwerten)
- Getestet unter **Ubuntu** und **Fedora** (GNOME/Nautilus)

Unterstützte Formate (Beispiele): `mp4`, `mkv`, `mov`, `mp3`, `wav`, `flac`, `ogg`  
> Hinweis: Für verlustfreies Mergen müssen die Streams **kompatibel** sein (gleicher Codec, Samplerate/Framerate, Channel‑Layout etc.).

---

## Voraussetzungen

- [`ffmpeg`](https://ffmpeg.org/)
- [`yad`](https://sourceforge.net/projects/yad-dialog/)

> Optional: `zenity` (nur als Fallback, falls du das Skript entsprechend erweiterst).

### Installation unter Ubuntu/Debian
```bash
sudo apt update
sudo apt install -y ffmpeg yad
```

### Installation unter Fedora
```bash
# yad
sudo dnf install -y yad

# ffmpeg (über RPM Fusion Free bereitgestellt)
sudo dnf install -y \
  https://mirrors.rpmfusion.org/free/fedora/rpmfusion-free-release-$(rpm -E %fedora).noarch.rpm
sudo dnf install -y ffmpeg
```

---

## Installation des Skripts

1) Skript nach `~/.local/share/nautilus/scripts` kopieren und ausführbar machen:
```bash
mkdir -p ~/.local/share/nautilus/scripts
cp catmerge.sh ~/.local/share/nautilus/scripts/"Merge Media Files"
chmod +x ~/.local/share/nautilus/scripts/"Merge Media Files"
```

2) Nautilus neu starten (oder ab- und wieder anmelden):
```bash
nautilus -q
```

3) Nutzung: Im Dateimanager mehrere Mediendateien markieren → Rechtsklick → **Skripte** → **Merge Media Files**.

---

## Nutzung & Verhalten

- **Zielordner wählen:** Beim Start kannst du den Zielordner auswählen (Timeout 5 s → Quelle wird genutzt).
- **Zieldateiname ändern:** Voreingestellter Dateiname kann angepasst werden (Timeout 5 s → Standardname).
- **Reihenfolge:** Dateien werden alphabetisch/natürlich sortiert (z. B. `…_001` bis `…_009`).
- **Stream‑Copy vs. Re‑Encode:**  
  - Wenn `ffmpeg -c copy` möglich ist → verlustfrei & schnell.  
  - Wenn nicht → Dialog fragt nach Neukodierung (H.264/AAC bei Video, passende Codecs bei Audio).  
- **Fortschritt:** Bei Copy ein pulsierender Balken, bei Re‑Encode prozentualer Fortschritt.
- **Überschreiben:** Existiert die Zieldatei, fragt das Skript mit 5‑Sekunden‑Timeout (Standard: **Ja**).

---

## Troubleshooting

- **„Line 1: string required / Invalid data …“**  
  Das passiert, wenn in der ffmpeg‑Liste eine leere Zeile steht. Die aktuelle Version filtert leere Zeilen aus.

- **Es passiert „nichts“ / kein Fenster:**  
  Prüfe das Log:  
  ```bash
  xdg-open /tmp/nautilus_merge_ffmpeg.log  # oder
  cat /tmp/nautilus_merge_ffmpeg.log
  ```
  Sieh nach, ob Abhängigkeiten fehlen oder ffmpeg einen Fehler meldet.

- **ffmpeg fehlt auf Fedora:**  
  RPM Fusion Free aktivieren (siehe Abschnitt „Installation unter Fedora“).

- **Leistung / Dauer:**  
  Neukodierung ist CPU‑intensiv und kann je nach Hardware lange dauern. Für sehr große Dateien erst mit Kopien testen.

---

## Deinstallation

```bash
rm -f ~/.local/share/nautilus/scripts/"Merge Media Files"
```

---

## Lizenz

Dieses Projekt steht unter der **GNU General Public License v3.0**.  
Siehe die Datei [`LICENSE`](LICENSE) für Details.

---

## Danksagung

- [FFmpeg](https://ffmpeg.org/) – das Schweizer Taschenmesser für Audio/Video
- [YAD](https://sourceforge.net/projects/yad-dialog/) – einfache GUI‑Dialoge für Shell‑Skripte

