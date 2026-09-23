# ICSim Python Port for Windows and Linux

A modern Python implementation of the original **ICSim (Instrument Cluster Simulator)** by ZombieCraig.

The original https://github.com/zombieCraig/ICSim is a C/SDL2 + SocketCAN based automotive instrument cluster simulator used for CAN bus training, reverse engineering, and security exercises. While the original project works well on Linux, it does not run natively on Windows.

This project reimplements ICSim in Python and adds several quality-of-life improvements while preserving compatibility with the original training concepts.

---

## Features

### Core ICSim Functionality

- Instrument cluster simulation using **pygame**
- Speedometer with animated needle
- Left and right turn signal indicators
- Door lock / unlock indicators
- Keyboard controls for vehicle functions
- CAN logging with real-time updates
- Hidden feature: CAN message highlighting when signal values change
- Hidden feature: ASCII decoding in CAN log
- CAN traffic recording

### Cross-Platform CAN Support

- Windows: Python-CAN VirtualBus
- Linux: SocketCAN `vcan0`
- Automatic backend selection based on operating system

### Training Features

- Randomized CAN IDs and byte positions
- Seed-based challenge generation
- Difficulty levels
- Background CAN noise generation
- Challenge mode exercises

### Integrated CAN Console

The simulator includes a built-in CAN injection console in the lower-right corner of the UI.

Features:

- Send arbitrary CAN messages
- Standard and extended CAN IDs
- Copy / paste support
- Real-time validation
- Immediate visibility in the CAN log

Example:

```text
7FF 01 02 03 04 05 06 07 08
```

### Integrated UDS / ISO-TP ECU

An embedded diagnostic ECU is included with challenge-specific hidden stuff

This allows diagnostics exercises without requiring a second application.

---

# Screenshots

icsim-py_screenshot.jpg

---

# Requirements

- Python 3.10+
- pygame-ce
- python-can
- pyperclip

Install dependencies:

```bash
pip install python-can pygame-ce pyperclip
```

---

# Linux Setup

Create a virtual CAN interface:

```bash
sudo modprobe vcan

sudo ip link add dev vcan0 type vcan

sudo ip link set up vcan0
```

Verify:

```bash
ip link show vcan0
```

Run:

```bash
python icsim_py.py
```

---

# Windows Setup

No additional CAN drivers are required.

The simulator automatically uses Python-CAN's built-in virtual CAN backend.

Run:

```bash
python icsim_py.py
```

---

# Project Layout

```text
icsim_py/
│
├── icsim_py.py
│
├── icsim-py_screenshot.jpg
│
└── README.md
```

---

# Controls

| Key | Function |
|-------|----------|
| Up Arrow | Accelerate |
| Down Arrow | Brake |
| Left Arrow | Left Turn Signal |
| Right Arrow | Right Turn Signal |
| Ctrl+(S, D, F, or G) | Toggle Door State |
| Ctrl+(E or X) | Toggle Hood / Trunk |
| Ctrl+R | Start/Stop Recording |
| ESC | Exit |

---

# CAN Send Console

The CAN Send widget is located in the lower-right corner.

### Supported Format

```text
ARBID DATA DATA DATA DATA DATA DATA DATA DATA
```

Example:

```text
123 01 02 03 04 05 06 07 08
```

Extended ID example:

```text
10000001 AA BB CC DD
```

### Allowed Arbitration IDs

Standard CAN:

```text
000 - 7FF
```

Extended CAN:

```text
10000000 - 1FFFFFFF
```

IDs outside these ranges are rejected.

---

## Clipboard Support

Select the CAN Send field and use:

```text
Ctrl+C
```

Copy current frame

```text
Ctrl+V
```

Paste frame from clipboard

Example:

```text
730 03 22 F1 90 00 00 00 00
```

---

# UDS Exercises

The integrated ECU listens for requests on:

```text
Challenge: Send CAN messages to get the UDS server ID
```
---

## Enter Extended Session

---

## Read VIN

Request:

```text
Challenge: Send CAN messages to get the full VIN response
```

Response:

```text
62 F1 90 ...
```
---

# Randomization

The simulator supports deterministic randomization for training.

Example:

```bash
python icsim_py.py -s 1401717626
```
---

# Difficulty Levels

### Level 1

Basic training mode.

Only relevant CAN messages appear.

```bash
python icsim_py.py -l 1
```

### Level 2

Challenge mode.

Additional background CAN traffic is generated to simulate a realistic vehicle network.

```bash
python icsim_py.py -l 2
```

---

# Recording CAN Traffic

Press:

```text
Ctrl+R
```

to begin recording.

Captured traffic is written to a timestamped log file.

Recordings can later be analyzed.

---

# Differences from the Original ICSim

| Feature | Original ICSim | Python Port |
|----------|----------|----------|
| Linux Support | Yes | Yes |
| Windows Support | No | Yes |
| SocketCAN | Yes | Yes |
| Built-in Virtual Bus | No | Yes |
| Integrated CAN Sender | No | Yes |
| Integrated UDS ECU | No | Yes |
| Copy/Paste Support | No | Yes |
| Python Extensibility | No | Yes |

---

# Intended Use Cases

This project is intended for:

- Automotive cybersecurity training
- CAN bus reverse engineering
- Capture-the-Flag events
- Vehicle network demonstrations

---

# Disclaimer

This simulator is intended for education, security research, and training purposes.

It simulates CAN bus traffic and diagnostic services and should not be connected to production vehicle systems without appropriate safeguards.

---

# Acknowledgements

Original ICSim:

- ZombieCraig
- https://github.com/zombieCraig/ICSim

Python Port:

- Cross-platform CAN support via Python-CAN
- UI implementation using pygame-ce
- Additional diagnostics and training capabilities designed for Windows and Linux environments
