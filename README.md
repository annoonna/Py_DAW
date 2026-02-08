🎵 PyQt-DAW

Python-basierte MIDI/NOTEN-DAW
Fokus: Komposition statt Sound-Design. Eine hochperformante Umgebung für Partitur, Scale-Snap und MIDI-Sequenzierung – inspiriert von der Logik klassischer Notenschrift wie Rosegarden.

🚀 Philosophie & Vision
Im Gegensatz zu klassischen "Synthesizer-DAWs", die auf Klangmanipulation fokussiert sind, versteht sich die Py-DAW als reine Noten-DAW.
Der Kern: Die musikalische Struktur (Noten, Rhythmik, Harmonien) steht im Vordergrund.
Workflow: Komponieren in einer Umgebung, die auf die Logik von Partituren optimiert ist.
Entwicklungsstatus: Aktuell im Aufbau. Der Fokus liegt auf der stabilen Einbindung von Browser, Arranger und High-Performance Pre-Rendering.
🛠 Installation & Setup
Linux (Debian/Ubuntu/Kali)
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
