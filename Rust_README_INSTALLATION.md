# 🎵 Py_DAW (ChronoScaleStudio) — Installation & Start

**Open-Source Digital Audio Workstation — Python/PyQt6 + Rust Audio-Engine**

---

## 📋 Erstinstallation (Schritt für Schritt)

### 1. ZIP entpacken

```bash
unzip Py_DAW_v0_0_20_XXX_TEAM_READY.zip -d Py_DAW
cd Py_DAW
```

### 2. System-Pakete installieren (einmalig, braucht sudo)

```bash
sudo apt update
sudo apt install -y \
    python3 python3-venv python3-pip \
    pipewire pipewire-jack pipewire-alsa \
    qpwgraph \
    libasound2-dev pkg-config \
    build-essential curl
```

### 3. Automatisches Setup starten (Python + Rust)

```bash
python3 setup_all.py --with-rust
```

Das Skript macht **alles automatisch**:
- Python Virtual Environment erstellen
- Alle Python-Pakete installieren
- Audio-System prüfen (PipeWire, JACK, ALSA)
- Rust installieren (falls noch nicht vorhanden)
- Rust Audio-Engine bauen (`cargo build --release`)
- Alles testen + Status-Report zeigen

Der erste Build dauert 1-3 Minuten. Danach nur noch Sekunden.

### 4. Rust-Engine permanent aktivieren (einmalig)

```bash
echo 'export USE_RUST_ENGINE=1' >> ~/.zshrc
source ~/.zshrc
```

> Für Bash-Nutzer: `~/.bashrc` statt `~/.zshrc`

### 5. DAW starten

```bash
chmod +x start_daw.sh
./start_daw.sh
```

Fertig. Das Skript startet automatisch die Rust-Engine im Hintergrund,
aktiviert das venv und öffnet die DAW.

---

## 🚀 Start-Methoden

### Methode 1: Start-Skript (empfohlen)

```bash
./start_daw.sh
```

Erkennt automatisch ob die Rust-Engine gebaut ist, startet sie im Hintergrund,
aktiviert das venv und öffnet die DAW. Beim Schließen wird alles aufgeräumt.

### Methode 2: Manuell

```bash
source myenv/bin/activate
python3 main.py
```

### Methode 3: Mit GDB Debugger

```bash
source myenv/bin/activate
env USE_RUST_ENGINE=1 gdb -batch -ex "run" -ex "bt" --args python3 main.py
```

---

## 🦀 Rust Audio-Engine testen (zwei Terminals)

**Terminal 1 — Engine starten:**
```bash
source myenv/bin/activate
cd pydaw_engine
RUST_LOG=info ./target/release/pydaw_engine
```

**Terminal 2 — Test-Client:**
```bash
source myenv/bin/activate
cd pydaw_engine
python3 test_bridge.py
```

Erwartete Ausgabe:
```
✅ Connected!
📊 Master meters: L=0.0000 R=0.0000
▶️  Playhead: beat=0.00 playing=False
📊 Received 33 playhead + 67 meter events in ~1s
🎉 All tests passed!
```

---

## ⚠️ Wichtig: Nach jeder neuen ZIP

Die `target/` Ordner (Rust Build-Output) werden nicht in der ZIP mitgeliefert
(wäre 100+ MB). Du musst einmal pro neuem Ordner die Engine neu bauen:

```bash
python3 setup_all.py --with-rust
./start_daw.sh
```

Das geht schnell (~10-60 Sekunden) weil Rust + alle Dependencies schon installiert sind.

---

## 🛠️ Fehlerbehebung

### "ModuleNotFoundError: No module named 'PyQt6'"
```bash
source myenv/bin/activate
python3 setup_all.py --with-rust
```

### "Rust: cargo not found"
```bash
source ~/.cargo/env
python3 setup_all.py --with-rust
```

### "No audio output device found"
```bash
systemctl --user status pipewire
aplay -l
qpwgraph
```

### "JACK server is not reachable"
```bash
pw-jack python3 main.py
```

### Status prüfen
```bash
python3 setup_all.py --check
```

---

## 📊 Befehle-Spickzettel

```bash
# === SETUP (einmalig + nach jeder neuen ZIP) ===
python3 setup_all.py --with-rust    # Alles installieren + Rust bauen
python3 setup_all.py --check        # Status prüfen

# === STARTEN ===
./start_daw.sh                      # Alles automatisch (empfohlen)

# === RUST ENGINE TESTEN ===
cd pydaw_engine
RUST_LOG=info ./target/release/pydaw_engine   # Engine starten (Terminal 1)
python3 test_bridge.py                         # Test-Client (Terminal 2)

# === RUST MANUELL BAUEN ===
cd pydaw_engine
cargo build --release               # Release (optimiert)

# === AUDIO ===
qpwgraph                            # Audio-Routing visualisieren
pw-jack python3 main.py             # Mit PipeWire-JACK Bridge
```

---

*Version: v0.0.20.635 — 2026-03-19*
