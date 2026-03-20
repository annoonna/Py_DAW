# 🎵 Py_DAW (ChronoScaleStudio) — Rust-First Installation, Build & Start

**Open-Source Digital Audio Workstation — Python/PyQt6 + Rust Audio-Engine**

---

## Ziel dieser Anleitung

Diese Anleitung ist auf den **aktuellen Rust-Workflow** ausgerichtet.
Sie trennt sauber zwischen:

- **einmaligem Setup**
- **normalem Start**
- **Debug-Start mit GDB**
- **optionalen Entwicklerbefehlen**

Wichtig: Seit der Umstellung auf Rust ist **`python3 setup_all.py --with-rust`** der zentrale Einstieg.
Nicht jeder Rust-Befehl muss bei jedem Start manuell ausgeführt werden.

---

## 📦 Projekt öffnen

```bash
cd Py_DAW_v0_0_20_68x_TEAM_READY
```

Falls du das Projekt gerade erst entpackt hast, zuerst in den entpackten Ordner wechseln.

---

## 🚀 Empfohlener Standardablauf

### 1. Setup ausführen (Python + Rust)

```bash
python3 setup_all.py --with-rust
```

Das ist der wichtigste Schritt. Dabei werden – je nach Projektstand – unter anderem vorbereitet:

- Python-Umgebung
- benötigte Python-Pakete
- Rust-Toolchain
- Rust-Engine-Build
- Projekt-Checks

Danach ist das Projekt in der Regel startbereit.

### 2. Virtuelle Umgebung aktivieren

```bash
source myenv/bin/activate
```

### 3. DAW starten

**Normaler Start:**

```bash
./start_daw.sh
```

**Debug-Start:**

```bash
./start_dawGDB.sh
```

---

## ✅ Sinnvoller Rust-Check nach dem Setup

Wenn du nach dem Setup prüfen willst, ob die Rust-Seite sauber gebaut ist:

```bash
cd pydaw_engine
cargo test
cargo build --release
cd ..
```

### Erwartung beim aktuellen Stand

```bash
cargo test
# Erwartet aktuell: 43 passed, 0 failed

cargo build --release
# Erwartet aktuell: 0 errors, 0 warnings
```

Hinweis: Die Anzahl der Tests kann sich mit neuen Versionen ändern. Entscheidend ist,
dass **alle Tests erfolgreich sind** und der Release-Build **fehlerfrei** durchläuft.

---

## 🧭 Startvarianten

### Variante A — Empfohlen

```bash
./start_daw.sh
```

Das ist der normale Alltagsstart.

### Variante B — Start mit GDB

```bash
./start_dawGDB.sh
```

Gut für Abstürze, Hänger oder Backtraces.

### Variante C — Manuell mit aktivierter Rust-Engine

```bash
env USE_RUST_ENGINE=1 python3 main.py
```

Diese Variante ist sinnvoll, wenn du bewusst ohne Startskript testen willst.

### Variante D — Manuell mit GDB und Rust-Engine

```bash
env USE_RUST_ENGINE=1 gdb -batch -ex "run" -ex "bt" --args python3 main.py
```

Das ist die direkte Debug-Variante für die Hauptanwendung.

---

## 🧪 Bridge / Python↔Rust gezielt testen

Wenn du gezielt die Bridge prüfen willst:

```bash
gdb -batch -ex "run" -ex "bt" --args python3 test_bridge.py
```

Oder – für die Hauptanwendung – direkt:

```bash
gdb -batch -ex "run" -ex "bt" --args python3 main.py
```

Wenn sichergestellt sein soll, dass die Rust-Engine aktiv ist, dann diese Variante nutzen:

```bash
env USE_RUST_ENGINE=1 gdb -batch -ex "run" -ex "bt" --args python3 main.py
```

---

## 🦀 Welche Rust-Befehle wirklich wann sinnvoll sind

### `python3 setup_all.py --with-rust`
**Nutzen:** Standard-Setup nach neuer ZIP oder frischem Checkout.

**Verwenden wenn:**
- du eine neue Projektversion entpackt hast
- `myenv` fehlt
- Rust oder Python-Abhängigkeiten fehlen
- du sicher alles sauber neu vorbereiten willst

---

### `cargo test`
**Nutzen:** Prüft die Rust-Engine funktional.

**Verwenden wenn:**
- du Rust-Code geändert hast
- du sicher sein willst, dass die Engine-Tests grün sind

---

### `cargo build --release`
**Nutzen:** Baut die optimierte Rust-Engine.

**Verwenden wenn:**
- du Rust-Code geändert hast
- nach einer neuen ZIP noch kein frischer Build existiert
- du explizit den Release-Build prüfen willst

---

### `cargo fix --bin "pydaw_engine" -p pydaw_engine --allow-dirty --allow-no-vcs`
**Nutzen:** Automatische Rust-Code-Korrekturen, soweit Cargo sie sicher anwenden kann.

**Wichtig:** Das ist **kein normaler Startschritt**.

**Nur verwenden wenn:**
- du aktiv am Rust-Code arbeitest
- du Compiler-Hinweise/Warnungen bereinigen willst
- du bewusst Quellcode automatisch anpassen lassen möchtest

**Nicht nötig für den normalen Benutzer-Start.**

---

### `maturin develop --release`
**Nutzen:** Baut und installiert die Python-Bindings der Rust-Komponenten direkt in die aktive virtuelle Umgebung.

```bash
source myenv/bin/activate
cd pydaw_engine
maturin develop --release
```

**Nur verwenden wenn:**
- sich Python↔Rust-Bindings geändert haben
- du das Rust-Python-Modul gezielt neu ins venv installieren willst
- `test_bridge.py` oder Python-Imports nach Binding-Änderungen aktualisiert werden müssen

**Nicht bei jedem normalen Start nötig.**

---

## 🔁 Empfohlene Abläufe für typische Fälle

### Fall 1 — Neue ZIP erhalten

```bash
cd Py_DAW_v0_0_20_634_TEAM_READY
python3 setup_all.py --with-rust
source myenv/bin/activate
./start_daw.sh
```

---

### Fall 2 — Du willst nach dem Setup alles einmal sauber verifizieren

```bash
cd Py_DAW_v0_0_20_634_TEAM_READY
python3 setup_all.py --with-rust
source myenv/bin/activate
cd pydaw_engine && cargo test
cargo build --release
cd ..
./start_daw.sh
```

---

### Fall 3 — Du debugst einen Absturz der Hauptanwendung

```bash
cd Py_DAW_v0_0_20_634_TEAM_READY
source myenv/bin/activate
env USE_RUST_ENGINE=1 gdb -batch -ex "run" -ex "bt" --args python3 main.py
```

---

### Fall 4 — Du arbeitest aktiv an der Rust-Engine

```bash
cd Py_DAW_v0_0_20_634_TEAM_READY
source myenv/bin/activate
cd pydaw_engine
cargo test
cargo build --release
cargo fix --bin "pydaw_engine" -p pydaw_engine --allow-dirty --allow-no-vcs
maturin develop --release
cd ..
```

Danach z. B.:

```bash
./start_daw.sh
```

oder

```bash
env USE_RUST_ENGINE=1 python3 main.py
```

---

## 📋 Kompakter Spickzettel

```bash
# === STANDARD NACH NEUER ZIP ===
cd Py_DAW_v0_0_20_634_TEAM_READY
python3 setup_all.py --with-rust
source myenv/bin/activate
./start_daw.sh

# === DEBUG-START ===
./start_dawGDB.sh

# === RUST-ENGINE PRÜFEN ===
cd pydaw_engine
cargo test
cargo build --release
cd ..

# === HAUPTANWENDUNG MIT RUST-ENGINE DIREKT STARTEN ===
env USE_RUST_ENGINE=1 python3 main.py

# === HAUPTANWENDUNG MIT GDB + RUST-ENGINE ===
env USE_RUST_ENGINE=1 gdb -batch -ex "run" -ex "bt" --args python3 main.py

# === BRIDGE-TEST ===
gdb -batch -ex "run" -ex "bt" --args python3 test_bridge.py

# === NUR FÜR RUST-ENTWICKLUNG ===
cd pydaw_engine
cargo fix --bin "pydaw_engine" -p pydaw_engine --allow-dirty --allow-no-vcs
maturin develop --release
```

---

## 🧾 Kurzfazit

Für den normalen Alltag reicht jetzt fast immer:

```bash
python3 setup_all.py --with-rust
source myenv/bin/activate
./start_daw.sh
```

Für Debugging:

```bash
./start_dawGDB.sh
```

Für tieferes Rust-Debugging:

```bash
env USE_RUST_ENGINE=1 gdb -batch -ex "run" -ex "bt" --args python3 main.py
```

Und ganz wichtig:

- **`cargo fix` ist optional und nur für aktive Rust-Entwicklung gedacht**
- **`maturin develop --release` ist optional und nur für Binding-/Modul-Updates gedacht**
- **`cargo build --release` ist ein Verifikations-/Build-Schritt, kein dauernder Pflichtschritt vor jedem Start**

---

### Fall X — Mach es so - !!!

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

### Fall X — Mach es so -Du willst nach dem Setup alles einmal sauber verifizieren


```bash
source myenv/bin/activate

cd Py_DAW_v0_0_20_634_TEAM_READY

python3 setup_all.py --with-rust

cd pydaw_engine && cargo test

cargo build --release

cd ..

chmod +x start_daw.sh 

./start_daw.sh

oder 

chmod +x start_dawGDB.sh 

./start_dawGDB.sh
```


*Überarbeitete Fassung für den Rust-First-Workflow.*
