# CatMerge – Media-Dateien einfach zusammenfügen

[🇬🇧 English Version](README.md)

![Lizenz: CC0](https://img.shields.io/badge/Lizenz-CC0-lightgrey.svg) ![Shell](https://img.shields.io/badge/Geschrieben%20in-Bash-4EAA25.svg?logo=gnu-bash&logoColor=white) ![ffmpeg](https://img.shields.io/badge/Nutzt-ffmpeg-blue.svg) ![yad](https://img.shields.io/badge/GUI-yad-purple.svg) ![zenity](https://img.shields.io/badge/GUI-zenity-purple.svg)

**CatMerge** ist ein Shell-Skript, mit dem du Video- oder Audiodateien direkt im Dateimanager zusammenfügen kannst.  
Perfekt für **Action-Cams oder Drohnen**, die Aufnahmen in 4-GB-Segmente aufteilen.

Das Skript versucht, die Dateien **verlustfrei** mit `ffmpeg -c copy` zusammenzuführen.  
Falls dies nicht möglich ist (z. B. bei variabler Bitrate), wirst du gefragt, ob eine **Neukodierung** durchgeführt werden soll.

---

## Voraussetzungen

- `ffmpeg`
- `yad` oder `zenity` (für Dialoge; wenn keines vorhanden ist, wird ins Terminal ausgegeben)

Ubuntu/Debian:
```bash
sudo apt update
sudo apt install -y ffmpeg yad  # oder: zenity
```

Fedora:
```bash
sudo dnf install -y yad  # oder: zenity
# ffmpeg über RPM Fusion
sudo dnf install -y   https://mirrors.rpmfusion.org/free/fedora/rpmfusion-free-release-$(rpm -E %fedora).noarch.rpm
sudo dnf install -y ffmpeg
```

---

## Skript installieren (gemeinsamer Schritt)

```bash
git clone https://github.com/Pappmann/CatMerge.git
cd CatMerge
mkdir -p ~/.local/bin
cp "Merge Media Files" ~/.local/bin/catmerge
chmod +x ~/.local/bin/catmerge
```

> Das Skript ist **universell**: es akzeptiert Nautilus-Umgebungsvariablen *oder* `%F`-Argumente anderer Dateimanager.

---

## Integration in den Dateimanager

### Nautilus (GNOME Files)
```bash
mkdir -p ~/.local/share/nautilus/scripts
ln -sf ~/.local/bin/catmerge ~/.local/share/nautilus/scripts/"Merge Media Files"
nautilus -q
```

### Nemo (Cinnamon)
```bash
mkdir -p ~/.local/share/nemo/scripts
ln -sf ~/.local/bin/catmerge ~/.local/share/nemo/scripts/"Merge Media Files"
nemo -q || true
```

### Caja (MATE)
```bash
mkdir -p ~/.config/caja/scripts
ln -sf ~/.local/bin/catmerge ~/.config/caja/scripts/"Merge Media Files"
caja -q || true
```

### Dolphin (KDE)
```bash
mkdir -p ~/.local/share/kio/servicemenus
cat > ~/.local/share/kio/servicemenus/catmerge.desktop << 'EOF'
[Desktop Entry]
Type=Service
X-KDE-ServiceTypes=KonqPopupMenu/Plugin
MimeType=video/*;audio/*;
Actions=CatMerge;
Icon=media-tape
X-KDE-Submenu=CatMerge

[Desktop Action CatMerge]
Name=Merge Media Files (CatMerge)
Icon=media-tape
Exec=/bin/bash -lc '~/.local/bin/catmerge %F'
EOF
```

### PCManFM-Qt
```bash
mkdir -p ~/.local/share/file-manager/actions
cat > ~/.local/share/file-manager/actions/catmerge.desktop << 'EOF'
[Desktop Entry]
Type=Action
Name=Merge Media Files (CatMerge)
Icon=media-tape
Profiles=profile-zero;

[X-Action-Profile profile-zero]
MimeTypes=video/*;audio/*;
Exec=/bin/bash -lc '~/.local/bin/catmerge %F'
Name=Default profile
EOF
```

### Thunar (Xfce)
Eigene **Aktion** über die GUI hinzufügen:  
- *Name:* `Merge Media Files (CatMerge)`  
- *Befehl:* `/bin/bash -lc '~/.local/bin/catmerge %F'`  
- *Darstellungsbedingungen:* aktivieren für **Audio-Dateien** und **Video-Dateien**.

---

## Nutzung

### Im Dateimanager
1. Segmente auswählen (z. B. `…_001.MP4`, `…_002.MP4`, …).  
2. Rechtsklick → **Skripte**/**Aktionen** → **Merge Media Files**.  
3. Falls nötig, **Neukodierung** bestätigen.  
4. Ergebnisdatei wird im gleichen Ordner gespeichert (Standardname enthält das Ursprungsdatum).

### Im Terminal
CatMerge kann auch direkt auf der Kommandozeile genutzt werden:

```bash
# Dateien in natürlicher Sortierung zusammenfügen
catmerge datei1.mp4 datei2.mp4 datei3.mp4

# Oder mit Shell-Globs (automatisch sortiert)
catmerge *.MP4

# Funktioniert auch mit Audio
catmerge spur1.mp3 spur2.mp3
```

Das Log wird nach `/tmp/catmerge.log` geschrieben.

---

## Lizenz

CC0 – Public Domain
