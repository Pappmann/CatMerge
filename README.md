# CatMerge – Easily Merge Media Files

[🇩🇪 Deutsche Version](README_de.md)

**CatMerge** is a shell script that lets you merge video/audio files right from your file manager.  
Ideal for **action cams or drones** that split recordings into 4‑GB segments.

The script tries to merge **losslessly** using `ffmpeg -c copy`.  
If that isn’t possible (e.g. variable bitrate), it asks to **re‑encode**.

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

Put the script somewhere in your home and make it executable. The following puts it into `~/.local/bin`:

```bash
mkdir -p ~/.local/bin
cp "Merge Media Files" ~/.local/bin/catmerge
chmod +x ~/.local/bin/catmerge
```

> The script is **universal**: it accepts Nautilus environment variables *or* `%F` arguments from other file managers.

---

## Integrate with your file manager

### Nautilus (GNOME Files)
Copy (or symlink) into the Nautilus *Scripts* folder, so it shows up in the context menu:

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
Create a **Service Menu** so CatMerge appears in the right‑click menu for video/audio:

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

Restart Dolphin or log out/in if needed.

### PCManFM‑Qt
Create a file‑manager action:

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
Create a **Custom Action** via GUI (recommended):  
- *Name:* `Merge Media Files (CatMerge)`  
- *Command:* `/bin/bash -lc '~/.local/bin/catmerge %F'`  
- *Appearance Conditions:* enable for **Audio Files** and **Video Files**.

Or append to `~/.config/Thunar/uca.xml` (creates file if missing):
```bash
mkdir -p ~/.config/Thunar
[[ -f ~/.config/Thunar/uca.xml ]] || { echo '<?xml version="1.0" encoding="UTF-8"?>' >~/.config/Thunar/uca.xml; echo '<actions>' >>~/.config/Thunar/uca.xml; echo '</actions>' >>~/.config/Thunar/uca.xml; }
tmp="$(mktemp)"
awk -v cmd="/bin/bash -lc '~/.local/bin/catmerge %F'" '
  BEGIN{added=0}
  /<\/actions>/ && !added {
    print "<action>";
    print "  <icon>media-tape</icon>";
    print "  <name>Merge Media Files (CatMerge)</name>";
    print "  <unique-id>catmerge-'"'"'"'"'"'""'"'"'"'"'"'</unique-id>";
    print "  <command>" cmd "</command>";
    print "  <description>Merge selected media segments</description>";
    print "  <patterns>*</patterns>";
    print "  <audio-files/>";
    print "  <video-files/>";
    print "</action>";
    added=1
  }
  {print}
' ~/.config/Thunar/uca.xml > "$tmp" && mv "$tmp" ~/.config/Thunar/uca.xml
```

---

## Usage

1. Select the media segments (e.g., `…_001.MP4`, `…_002.MP4`, …).  
2. Right‑click → **Scripts**/**Actions** → **Merge Media Files**.  
3. If required, confirm **re‑encoding**.  
4. Output is saved in the same folder (default name includes the source date).

---

## License

CC0 – Public Domain
