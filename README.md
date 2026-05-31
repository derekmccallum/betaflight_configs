# Betaflight Configs

Personal Betaflight CLI backups for each quadcopter build.

## Contents

| Quad | Directory | Board | BF Version | Notes |
|---|---|---|---|---|
| FoldApe4 | `darwinfpv_foldape4/` | DARWINF4SX1280HD | 4.4.2 | VTx config JSON, STL camera mounts |
| Mobula8 | `happymodel_mobula8/` | CRAZYBEEF405 | 2025.12.2 | VTx table Lua script |
| Chimera 7 | `iflight_chimera_7/` | SPEEDYBEEF7V3 | 4.5.2 | GPS-equipped long-range build |

Config files are raw Betaflight CLI output (`dump all` or `diff all`). Accompanying files (VTx tables, STLs) live alongside.

---

## OSD Setup

Common elements active across both whoop and 7" builds:

- **Battery voltage** — top-right area
- **Throttle position** — top-right area
- **Flight mode** — top area
- **Power (watts)** — upper area
- **Core temperature** — upper area
- **Crosshairs** — center
- **Artificial horizon sidebar**
- **Up/down reference**
- **DISARMED** — lower area
- **Warnings** — lower center
- **Ready mode** indicator
- **Camera frame** (24x11)

Chimera 7 additionally shows **GPS sats**, **home direction/distance**, **flight distance**, **ESC temperature**, and **altitude** — GPS-dependent elements appropriate for a long-range build.

Units: METRIC. Warnings: standard arming/ in-flight alerts enabled. Logo on arming: OFF. Canvas: 53×20 (Chimera 7) / default (Mobula8). Frame rate: 12 Hz. Background: transparent.

---

## Modes Setup

Consistent across all builds:

| Switch | Channel | Mode | Range |
|---|---|---|---|
| AUX1 | CH5 | ARM | 900–1200 |
| AUX2 (mid) | CH6 | ANGLE | 1300–1700 |
| AUX2 (high) | CH6 | HORIZON | 1700–2100 |
| AUX3 | CH7 | BEEPER | 1300–2100 |

Mobula8 also assigns **OSD disable switch** on AUX4 (CH8, 1300–2100) for clean DVR footage.
