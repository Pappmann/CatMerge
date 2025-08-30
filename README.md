# CatMerge – Easily Merge Media Files

[🇩🇪 Deutsche Version](README_de.md)

![License: CC0](https://img.shields.io/badge/License-CC0-lightgrey.svg) ![Shell](https://img.shields.io/badge/Made%20with-Bash-4EAA25.svg?logo=gnu-bash&logoColor=white)  
![ffmpeg](https://img.shields.io/badge/Uses-ffmpeg-blue.svg) ![yad](https://img.shields.io/badge/GUI-yad-purple.svg)

**CatMerge** is a shell script that lets you merge video/audio files right from your file manager.  
Ideal for **action cams or drones** that split recordings into 4-GB segments.

The script tries to merge **losslessly** using `ffmpeg -c copy`.  
If that isn’t possible (e.g. variable bitrate), it asks to **re-encode**.

---

## Requirements

- `ffmpeg`
- `yad` (for dialogs; script still prints to terminal if missing)

Ubuntu/Debian:
```bash
sudo apt update
sudo apt install -y ffmpeg yad
```

Fedora:
```bash
sudo dnf install -y yad
# ffmpeg via RPM Fusion
sudo dnf install -y   https://mirrors.rpmfusion.org/free/fedora/rpmfusion-free-release-$(rpm -E %fedora).noarch.rpm
sudo dnf install -y ffmpeg
```

---

## Install the script (common step)

```bash
git clone https://github.com/Pappmann/CatMerge.git
cd CatMerge
mkdir -p ~/.local/bin
cp "Merge Media Files" ~/.local/bin/catmerge
chmod +x ~/.local/bin/catmerge
```

> The script is **universal**: it accepts Nautilus environment variables *or* `%F` arguments from other file managers.

---

## Integrate with your file manager

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
Create a **Custom Action** via GUI:  
- *Name:* `Merge Media Files (CatMerge)`  
- *Command:* `/bin/bash -lc '~/.local/bin/catmerge %F'`  
- *Appearance Conditions:* enable for **Audio Files** and **Video Files**.

---

## Usage

### From your File Manager
1. Select the media segments (e.g., `…_001.MP4`, `…_002.MP4`, …).  
2. Right-click → **Scripts**/**Actions** → **Merge Media Files**.  
3. If required, confirm **re-encoding**.  
4. Output is saved in the same folder (default name includes the source date).

### From the Terminal
You can also run CatMerge directly on the command line:

```bash
# Merge files in natural sort order
catmerge file1.mp4 file2.mp4 file3.mp4

# Or use shell globs (sorted automatically)
catmerge *.MP4

# Works with audio too
catmerge track1.mp3 track2.mp3
```

Log output is written to `/tmp/catmerge.log`.

---

## License

CC0 – Public Domain
