# CatMerge – Media-Dateien einfach zusammenfügen

**CatMerge** ist ein Nautilus-Skript, mit dem du Video- oder Audiodateien direkt im Dateimanager zusammenfügen kannst.  
Perfekt für Aufnahmen von **Action-Cams oder Drohnen**, die oft in 4 GB-Segmente geteilt werden.

Das Skript versucht, die Dateien **verlustfrei** mit `ffmpeg -c copy` zusammenzuführen.  
Falls dies nicht möglich ist (z. B. bei variabler Bitrate), wirst du gefragt, ob eine **Neukodierung** durchgeführt werden soll.

---

## Installation
**Voraussetzungen installieren**

Ubuntu/Debian:
```bash
sudo apt install ffmpeg yad
```
Fedora:
```bash
sudo dnf install yad
sudo dnf install \
  https://mirrors.rpmfusion.org/free/fedora/rpmfusion-free-release-$(rpm -E %fedora).noarch.rpm
sudo dnf install ffmpeg
```

**Script installieren**
```bash
mkdir -p ~/.local/share/nautilus/scripts
cp catmerge.sh ~/.local/share/nautilus/scripts/"Merge Media Files"
chmod +x ~/.local/share/nautilus/scripts/"Merge Media Files"
nautilus -q
```
**Nutzung**

    Im Dateimanager die zusammenzufügenden Dateien markieren.

    Rechtsklick → Skripte → Merge Media Files.

    Falls nötig, Neukodierung bestätigen.

    Ergebnisdatei wird im selben Ordner gespeichert.

**Lizenz**

GPL-3.0 – siehe LICENSE
