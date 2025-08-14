# CatMerge – Easily Merge Media Files

[🇩🇪 Deutsche Version](README_de.md)

**CatMerge** is a Nautilus script that allows you to merge video or audio files directly in your file manager.  
Perfect for recordings from **action cams or drones**, which are often split into 4 GB segments.

The script attempts to merge files **losslessly** using `ffmpeg -c copy`.  
If this is not possible (e.g., due to variable bitrate), you will be prompted to allow a **re-encoding**.

---

## Installation

**Install dependencies**

Ubuntu/Debian:
```bash
sudo apt install ffmpeg yad
```

Fedora:
```bash
sudo dnf install yad
sudo dnf install \\
  https://mirrors.rpmfusion.org/free/fedora/rpmfusion-free-release-$(rpm -E %fedora).noarch.rpm
sudo dnf install ffmpeg
```

**Install the script**
```bash
# Clone the repo
git clone https://github.com/Pappmann/CatMerge.git
cd CatMerge

# Make script executable
chmod +x "Merge Media Files"

# Copy to Nautilus scripts folder
mkdir -p ~/.local/share/nautilus/scripts
cp "Merge Media Files" ~/.local/share/nautilus/scripts/

# Restart Nautilus (or log out/in)
nautilus -q
```

---

## Usage

1. Select the files you want to merge in the file manager.  
2. Right-click → Scripts → **Merge Media Files**.  
3. If required, confirm re-encoding.  
4. The merged file will be saved in the same folder.

---

## License

The Unlicense
