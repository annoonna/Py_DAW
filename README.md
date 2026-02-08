# Py_DAW
Python-basierte MIDI/NOTEN-DAW mit Fokus auf Notation, Scale-Snap und High-Performance Pre-Rendering. 




























gdb -batch -ex "run" -ex "bt" --args python3 main.py  


   Um Samples ungehindert vom Browser in das Arrangement zu ziehen, ohne dass die Clip-Ansicht (Clip-Arranger) den Platz einnimmt:
      1   Overlay aktiv lassen: Behalte deine gewohnte Overlay-Einstellung bei, um die Übersicht zu wahren.
      2   Clip-Ansicht ausblenden: Schalte nur das untere Fenster (Clip-/Device-View) aus, um die vertikale Arbeitsfläche im Arrangement zu vergrößern.
      3   Drag & Drop: Ziehe deine Samples nun direkt aus dem Browser auf eine Audiospur im freien Arrangement-Bereich.

So spielst du am MIDI-Keyboard 

MIDI verbinden: Audio → MIDI Settings → dein Keyboard Connect

Track scharf schalten: Links am Track ist „R“ (Record-Arm) → anklicken
(Bei uns ist das kein roter Kreis-Icon, sondern das R im Track-Header.)

Instrument laden (wichtig!): Unten auf Device klicken → im Sampler ein Sample laden:

Load Sample drücken oder

WAV/MP3 etc. auf den Sampler droppen

Piano-Roll „Record“ unten auf EIN lassen → !!!! (In Arbeit) jetzt hörst du dich, aber es wird nichts geschrieben !!!!!!


.....................................................
 Betreff: Anzeige-Fix für den Arranger (GPU-Waveforms)
„Hey, falls der Arranger bei dir nicht richtig dargestellt wird (z. B. Grafikfehler, leere Flächen oder Flackern), liegt das wahrscheinlich an den GPU-Voreinstellungen in Kombination mit deinem aktuellen Grafiktreiber.
Abhilfe:
Gehe einfach im Menü auf Ansicht und nimm den Haken bei ‚GPU Waveforms‘ raus.
Hintergrund für dich:
Das schaltet das Hardware-Rendering (Vulkan/OpenGL) für die Wellenformen aus und nutzt das stabile Software-Rendering. Das ist ein guter Fallback, falls die GPU-Shader-Zuweisung in PyQt6 auf deinem System noch nicht perfekt mit dem Buffer-Sharing harmoniert. Wenn wir das stabil haben wollen, müssten wir uns nochmal die QOpenGLWidget-Initialisierung ansehen.“     
.....................................................
apt update
apt install -y vulkan-tools libvulkan-dev vulkan-utility-libraries-dev spirv-tools mesa-vulkan-drivers
.....................................................

# Spezifische Treiber je nach Grafikkarte:
# Für NVIDIA:
sudo apt install -y nvidia-vulkan-common
# Für AMD/Intel:
sudo apt install -y mesa-vulkan-drivers

vulkaninfo | grep -i "device name"
# ODER für einen visuellen Test (das bekannte "Vulkan-Gears"):
vkcube

🎵 Philosophie-Hinweis: Die Noten-DAW (Py-DAW)
Fokus: Komposition statt Sound-Design
Im Gegensatz zu klassischen "Synthesizer-DAWs", die primär auf Klangmanipulation und Effektketten ausgelegt sind, versteht sich dieses System als reine Noten-DAW – ganz im Geiste von Rosegarden. https://www.rosegardenmusic.com/

Der Kern: Die DAW dient als Partitur- und Sequenzer-Umgebung, in der die musikalische Struktur (Noten, Rhythmik, Harmonien) im Vordergrund steht.    Workflow: Anstatt Sounds "zu schrauben", komponierst du in einer Umgebung, die auf die Logik klassischer Notenschrift und MIDI-Sequenzierung optimiert ist.
Entwicklungsstatus: Die Py-DAW (Python-basiert) befindet sich aktuell noch im Aufbau. Ziel ist es, die Flexibilität von Python zu nutzen, um eine hochgradig anpassbare Umgebung für Komponisten zu schaffen, die Musik eher "schreiben" als "produzieren". 

Status-Check: Da die Py-DAW noch im Aufbau ist, liegt der Fokus derzeit auf der stabilen Einbindung des Browsers und des Arrangers, um den Übergang von der Sample-Auswahl zur strukturellen Komposition so nahtlos wie möglich zu gestalten. 

