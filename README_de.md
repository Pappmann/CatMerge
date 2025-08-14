# CatMerge – Media-Dateien einfach zusammenfügen

[🇬🇧 English Version](README.md)

**CatMerge** ist ein Shell‑Skript, mit dem du Video‑ oder Audiodateien direkt im Dateimanager zusammenfügen kannst.  
Ideal für Aufnahmen von **Action‑Cams oder Drohnen**, die oft in 4‑GB‑Segmente geteilt werden.

Das Skript versucht zunächst ein **verlustfreies** Mergen mit `ffmpeg -c copy`.  
Falls das nicht möglich ist (z. B. variable Bitrate), fragt es nach **Neukodierung**.

---

## Voraussetzungen

- `ffmpeg`
- `yad` (für Dialoge; ohne `yad` gibt es Terminal‑Ausgaben)

Ubuntu/Debian:
```bash
sudo apt update
sudo apt install -y ffmpeg yad
```

Fedora:
```bash
sudo dnf install -y yad
# ffmpeg über RPM Fusion
sudo dnf install -y \\
  https://mirrors.rpmfusion.org/free/fedora/rpmfusion-free-release-$(rpm -E %fedora).noarch.rpm
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

> Das Skript ist **universell**: Es versteht Nautilus‑Umgebungsvariablen *oder* `%F`‑Argumente anderer Dateimanager.

---

## Integration je Dateimanager

### Nautilus (GNOME Files)
Ins *Scripts*‑Verzeichnis verlinken, damit es im Kontextmenü erscheint:

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
**Service‑Menü** anlegen, damit CatMerge im Rechtsklick‑Menü für Audio/Video erscheint:

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

Dolphin ggf. neu starten oder ab‑/anmelden.

### PCManFM‑Qt
File‑Manager‑Action erstellen:

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
**Benutzerdefinierte Aktion** (GUI) anlegen:  
- *Name:* `Merge Media Files (CatMerge)`  
- *Befehl:* `/bin/bash -lc '~/.local/bin/catmerge %F'`  
- *Darstellung:* bei **Audio‑** und **Video‑Dateien** aktivieren.

Oder per XML zu `~/.config/Thunar/uca.xml` hinzufügen (Datei wird ggf. angelegt):
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
    print "  <description>Ausgewählte Mediensegmente zusammenfügen</description>";
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

## Nutzung

1. Segmente auswählen (z. B. `…_001.MP4`, `…_002.MP4`, …).  
2. Rechtsklick → **Skripte/Aktionen** → **Merge Media Files**.  
3. Falls nötig, **Neukodierung** bestätigen.  
4. Die Ausgabedatei wird im selben Ordner gespeichert (Standardname enthält das Quelldatum).

---

## Lizenz

CC0 – Public Domain
