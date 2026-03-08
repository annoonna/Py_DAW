Code oder Blame Lesen.!!!!!!! README.md

🎵 PyQt-DAW 
Python-basierte MIDI/NOTEN-DAW
Fokus: Komposition statt Sound-Design. Eine hochperformante Umgebung für Partitur, Scale-Snap und MIDI-Sequenzierung – inspiriert von der Logik klassischer Notenschrift wie Rosegarden.
🚀 Philosophie & Vision
Im Gegensatz zu klassischen "Synthesizer-DAWs", die auf Klangmanipulation fokussiert sind, versteht sich die Py-DAW als reine Noten-DAW.
Der Kern: Die musikalische Struktur (Noten, Rhythmik, Harmonien) steht im Vordergrund.
Workflow: Komponieren in einer Umgebung, die auf die Logik von Partituren optimiert ist.
Entwicklungsstatus: Aktuell im Aufbau. Der Fokus liegt auf der stabilen Einbindung von Browser, Arranger und High-Performance Pre-Rendering.
🛠 Installation & Setup
Linux (Debian/Ubuntu/...)
Für das High-Performance Rendering und Audio-Backend sind System-Libraries erforderlich:
bash
# 1. Vulkan-Treiber & Tools installieren
sudo apt update
sudo apt install -y vulkan-tools libvulkan-dev vulkan-utility-libraries-dev spirv-tools mesa-vulkan-drivers

# 2. Grafikspezifische Treiber (optional)
# Für NVIDIA:
sudo apt install -y nvidia-vulkan-common
# Für AMD/Intel:
sudo apt install -y mesa-vulkan-drivers
Verwende Code mit Vorsicht.

Windows (Python 3.13 Fix)
Falls pip install -r requirements.txt bei python-rtmidi fehlschlägt (fehlender Compiler):
Visual Studio Build Tools herunterladen.
Workload "Desktopentwicklung mit C++" wählen.
Sicherstellen, dass MSVC v143 und das Windows SDK angehakt sind.
Nach Neustart erneut pip install ausführen.
🎹 Quick Start: So spielst du am MIDI-Keyboard
MIDI verbinden: Gehe zu Audio → MIDI Settings → Wähle dein Keyboard und klicke Connect.
Track scharf schalten: Klicke das [R] (Record-Arm) im jeweiligen Track-Header (links am Track).
Instrument laden:
Klicke unten auf Device.
Drücke Load Sample im Sampler oder droppe eine WAV/MP3-Datei direkt auf das Device-Window.
Monitoring: Stelle sicher, dass die Piano-Roll auf Record: EIN steht.
Hinweis (In Arbeit): Aktuell hörst du das Instrument live (Monitoring), Noten werden jedoch noch nicht permanent in den Clip geschrieben.
🖥️ Workflow-Tipps für das Arrangement
Freies Drag & Drop aus dem Browser
Um Samples ohne Platzmangel direkt in das Arrangement zu ziehen:
Overlay: Behalte dein gewohntes Overlay aktiv.
Ansicht optimieren: Blende die untere Clip-/Device-View aus, um maximale vertikale Arbeitsfläche im Arranger zu erhalten.
Import: Ziehe Samples aus dem Browser direkt auf eine Audiospur im freien Bereich.
Troubleshooting: Grafikfehler (Anzeige-Fix)
Falls der Arranger Grafikfehler, Flackern oder leere Flächen zeigt:
Gehe im Menü auf Ansicht.
Deaktiviere [ ] GPU Waveforms.
Hintergrund: Dies erzwingt stabiles Software-Rendering, falls die GPU-Shader-Zuweisung (Vulkan/OpenGL) noch nicht optimal mit deinem Treiber harmoniert.
🐞 Debugging & Entwicklung
Für Entwickler, die Fehlerberichte erstellen möchten:
bash
# Start der DAW mit Backtrace-Ausgabe bei Absturz
gdb -batch -ex "run" -ex "bt" --args python3 main.py
Verwende Code mit Vorsicht.

System-Check
Prüfe deine Vulkan-Hardware mit:
bash
vulkaninfo | grep -i "device name"
# oder visuell:
vkcube
Verwende Code mit Vorsicht.

Projekt-Status: 🛠 Under Construction
Die Py-DAW nutzt die Flexibilität von Python, um eine anpassbare Umgebung für Komponisten zu schaffen, die Musik eher "schreiben" als "produzieren".




WINDOWS_INSTALLATION.md
### Schritt 4: FluidSynth installieren (für MIDI-Playback)

FluidSynth wird benötigt, um MIDI-Noten mit SoundFonts (SF2) in Audio umzuwandeln.

#### 4.1 FluidSynth herunterladen

Gehe zu: https://github.com/FluidSynth/fluidsynth/releases/latest



sudo apt install qsynth fluidsynth fluid-soundfont-gm


...............................................
requirements.txt


# --- Audio / MIDI ---
JACK-Client>=0.5.5
sounddevice>=0.4.6
soundfile>=0.12.1
mido>=1.3.2
python-rtmidi>=1.5.8
pydub>=0.25.1
PySide6>=6.6
pyfluidsynth>=1.3
mutagen>=1.47
tomli-w>=1.0.0
typing_extensions>=4.9
scipy>=1.11
librosa>=0.10



# --- Core ---
PyQt6>=6.6
numpy>=1.26

# --- Graphics (optional) ---
# The Arranger GPU Waveform overlay currently uses PyOpenGL when enabled.
PyOpenGL>=3.1.7
PyOpenGL-accelerate>=3.1.7; platform_system=="Linux"

# Linux Vulkan default (Qt Quick / RHI preparation)
# NOTE: Qt's Vulkan support primarily depends on system packages
# (libvulkan1, mesa-vulkan-drivers, vulkan-tools). This Python package is
# optional and mainly useful for future Vulkan tooling.
vulkan>=1.3.275.1; platform_system=="Linux"

# Optional: WebGPU (wgpu-native) - uses Vulkan on Linux.
# This is a future-proof option for replacing OpenGL overlays.
wgpu>=0.19.0; platform_system=="Linux"

# --- Python 3.13 compatibility (audioop removed) ---
audioop-lts
...................................................................................
# Audio Plugins auf macOS (LV2, LADSPA, DSSI, VST)

Diese Anleitung beschreibt die Pfade, Installationsmethoden und die Einrichtung via Homebrew für Linux-native Plugin-Formate auf macOS.

## 1. Standard-Verzeichnisse (Manuelle Installation)

Kopiere Plugin-Ordner oder Dateien in die folgenden Verzeichnisse. Falls ein Ordner nicht existiert, erstelle ihn manuell.

| Format | Endung | Systemweiter Pfad (für alle Nutzer) | Benutzer-Pfad (nur für dich) |
| :--- | :--- | :--- | :--- |
| **LV2** | `.lv2` | `/Library/Audio/Plug-Ins/LV2` | `~/Library/Audio/Plug-Ins/LV2` |
| **VST3** | `.vst3` | `/Library/Audio/Plug-Ins/VST3` | `~/Library/Audio/Plug-Ins/VST3` |
| **VST2** | `.vst` | `/Library/Audio/Plug-Ins/VST` | `~/Library/Audio/Plug-Ins/VST` |
| **LADSPA**| `.so`/`.dylib` | `/Library/Audio/Plug-Ins/LADSPA` | `~/.ladspa` |
| **DSSI**  | `.so` | `/Library/Audio/Plug-Ins/DSSI` | `~/.dssi` |

---

## 2. Installation via Homebrew (`brew`)

Einige Basis-Bibliotheken und Plugin-Sammlungen lassen sich direkt über das Terminal installieren.

### Basis-SDKs & Support
```bash
brew install lv2
brew install ladspa-sdk
brew install suil # Für LV2-GUIs wichtig
Plugin-Beispiele
brew install mda-lv2
brew install lsp-plugins-lv2 # Falls im Tap verfügbar

3. Umgebungsvariablen setzen (Wichtig für DAWs)
Damit Programme wie Ardour, Reaper oder Audacity die Plugins finden, die über Homebrew installiert wurden, sollten die Pfade in der Shell-Konfigurationsdatei (z.B. ~/.zshrc) hinterlegt werden.
Öffne die Konfiguration: nano ~/.zshrc und füge hinzu:
# LV2 Pfade
export LV2_PATH=$LV2_PATH:/usr/local/lib/lv2:/opt/homebrew/lib/lv2:~/Library/Audio/Plug-Ins/LV2

# LADSPA Pfade
export LADSPA_PATH=$LADSPA_PATH:/usr/local/lib/ladspa:/opt/homebrew/lib/ladspa:/Library/Audio/Plug-Ins/LADSPA
Aktivieren mit: source ~/.zshrc

4. Problembehandlung (Gatekeeper)
macOS blockiert oft Plugins, die nicht von verifizierten Entwicklern stammen.
Plugin kopieren/installieren.
DAW starten (Plugin-Scan schlägt ggf. fehl).
Systemeinstellungen > Datenschutz & Sicherheit öffnen.
Ganz unten bei "Sicherheit" auf "Dennoch öffnen" für die entsprechende Plugin-Datei klicken.
DAW erneut scannen lassen.
5. Nützliche Befehle (Terminal)
Cache von ChronoScaleStudio löschen:
rm ~/.cache/ChronoScaleStudio/lv2_probe_cache.json
Symlink von Homebrew-Plugins zu System-Ordnern:
ln -s $(brew --prefix)/lib/lv2/* ~/Library/Audio/Plug-Ins/LV2/
Soll ich dir noch die **spezifischen Befehle** für einen bestimmten Plugin-Adapter (wie z.B. einen VST-zu-AU-Wrapper) heraussuchen?

.........................................................................................................................................























