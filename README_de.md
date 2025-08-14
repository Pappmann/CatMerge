# CatMerge – Media-Dateien einfach zusammenfügen

[🇬🇧 English Version](README.md)

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
# Repo klonen
git clone https://github.com/Pappmann/CatMerge.git
cd CatMerge

# Skript ausführbar machen
chmod +x "Merge Media Files"

# In den Nautilus-Skriptordner kopieren
mkdir -p ~/.local/share/nautilus/scripts
cp "Merge Media Files" ~/.local/share/nautilus/scripts/

# Nautilus neu starten (oder ab-/anmelden)
nautilus -q

```
**Nutzung**

    Im Dateimanager die zusammenzufügenden Dateien markieren.

    Rechtsklick → Skripte → Merge Media Files.

    Falls nötig, Neukodierung bestätigen.

    Ergebnisdatei wird im selben Ordner gespeichert.

**Lizenz**

Die Unlizenz 
