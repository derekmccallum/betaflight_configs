# iFlight Chimera 7 — Betaflight config

Long-range 7" quad, converted from DJI O4 Pro digital back to analog video (Oct 2026).

> Items marked **TODO** are unconfirmed and need filling in or checking on the hardware.

## Firmware

| Item | Value |
|---|---|
| Firmware | Betaflight **2026.6.2** (built 4 Oct 2026, MSP API 1.48)<br>ExpressLRS v3.6.4 |
| Build key | `7ddc3fb74d75cce483e672c6a7e4bf41` |
| Target | `SPEEDYBEEF7V3` (manufacturer `SPBE`) |
| MCU | STM32F722, 216 MHz (no overclock) |
| Config file | `betaflight/diff-all.txt` (diff against 2026.6.2 defaults) |

The config was rebuilt from defaults on 2026.6.2 (the diff starts with `defaults nosave`).
Anything not in the diff is the 2026.6.2 default, not the old 4.5.2 value.

### Cloud build options

The cloud builder only includes what is ticked. Restoring a diff onto a build that is
missing an option fails **silently**, so re-flash with all of these:

- [x] **OSD (SD)** — analog OSD chip. **Not** OSD (HD), which is for DJI/digital only
- [x] **CRSF** serial receiver protocol
- [x] **GPS** (+ GPS Rescue)
- [x] **Barometer** (BMP280)
- [x] **Blackbox** to SD card
- [x] **VTX SmartAudio**
- [ ] TODO: any others ticked in the builder (check the Setup tab build info)

## Hardware

| Component | Detail |
|---|---|
| Frame | iFlight Chimera 7 |
| FC | SpeedyBee F7 V3 (stack) |
| ESC | SpeedyBee F7 V3 (stack) |
| Motors | iFlight Xing 2809, 14 poles — TODO: KV (800 / 1250) |
| Gyro / acc | BMI270 (gyro position 2) — runs at 3.2 kHz max |
| Baro | BMP280 (I2C1) |
| GPS | u-blox M10, UART6 @ 57600 |
| OSD | Onboard analog OSD chip, 30 × 16 grid, PAL |
| Blackbox | SD card (~480 MB) |
| Receiver | CRSF protocol, 250 Hz packet rate — RadioMaster RP4TD True Diversity 2.4GHz RX (ESP32 target) |
| VTX | RushFPV Tank Ultimate Mini 48ch v1 — 25 / 200 / 500 / 800 mW, SmartAudio |
| Camera | Run Cam Phoenix (J Bardwell edition) |
| Battery | S6 (`bat_capacity` is unset) |
| Goggles | Skyzone Cobra X V2 (analog, SteadyView). DJI Goggles 3 for the old O4 setup |

## UART map

| UART | Function | Notes |
|---|---|---|
| USB VCP | MSP | Configurator |
| UART1 | VTX SmartAudio (`2048`) | Was MSP + VTX (MSP) for the DJI O4 |
| UART2 | Serial RX (CRSF, `64`) | Receiver TX → RX2, receiver RX → TX2 |
| UART3 | Free | Full TX/RX |
| UART4 | Free | **RX only** (no TX pin), so can't be used for VTX control |
| UART6 | GPS (`2`) | 57600 |

## Video wiring

Analog OSD only works if video passes **through** the FC:

```
Camera video → FC camera video pad → FC (OSD overlay) → FC VTX video pad → VTX
```

A camera wired straight to the VTX gives a clean picture with **no OSD**.

## VTX

| Setting | Value |
|---|---|
| VTX table | 6 bands (A, B, E, F, R, L) × 8 channels; power 25 / 200 / 500 / 800 mW (14 / 23 / 27 / 29 dBm) |
| Current channel | Raceband R7, **5880 MHz** — see open items |
| Default power | 25 mW (`vtx_power = 1`) |
| Low power when disarmed | On (`vtx_low_power_disarm = ON`) — keeps the VTX cool on the bench |
| Power switch | AUX5: low = 25 mW, mid = 200 mW, high = 500 mW (band/channel unchanged) |

UK licence-free 5.8 GHz: **25 mW, 5725–5875 MHz** only. Higher power needs an amateur licence.

## Receiver / ExpressLRS

| Setting | Value |
|---|---|
| Flashing | ExpressLRS Configurator |
| Target Category |RadioMaster 2.4 GHz |
| Device Target | RadioMaster RP4TD True Diversity 2.4GHz RX (ESP32 target) |
| Version | 3.6.4 |
| Binding Phrase | elrs1234 |
| Flashing Method | Betaflight Passthrough |
| Connection Protocol | Serial UART (CRSF) |
| Comment | Can't go to v4 yet owing to lack of support in another drone (Dawin FoldApe 4") |

## Key settings

| Area | Setting |
|---|---|
| OSD | Analog OSD chip, `vcd_video_system = PAL` |
| Airmode | **Off** (`feature -AIRMODE`) |
| Motors | DShot600 — TODO: check `get dshot_bidir` (RPM filter needs it on) |
| Rates | Actual: 70°/s centre, 670°/s max, no expo (2026.6.2 defaults) |
| Throttle curve | `thr_mid = 25`, `thr_expo = 70` |
| PIDs / filters | 2026.6.2 defaults (no custom tune) |
| Failsafe | Default — TODO: check `get failsafe_procedure` (was `DROP` on 4.5.2) |
| GPS Rescue | Defaults; not assigned to a switch |
| Core temp alarm | 85 °C |

### Modes

| Mode | Channel | Range |
|---|---|---|
| ARM | AUX1 | 900–1200 (**switch low = armed**) |
| ANGLE | AUX2 | 1300–1700 (mid) |
| HORIZON | AUX2 | 1700–2100 (high); AUX2 low = acro |
| BEEPER | AUX3 | 1300–2100 |
| OSD DISABLE | AUX4 | 1300–2100 — **hides the OSD** |
| VTX power | AUX5 | See VTX section |

### OSD layout (PAL, 30 × 16)

| Element | Col, row |
|---|---|
| VTX channel | 1, 1 |
| Link quality | 1, 2 |
| Battery voltage | 1, 7 |
| Core temp | 1, 8 |
| Throttle | 1, 11 |
| Home distance | 12, 1 |
| Home direction | 14, 2 |
| GPS sats | 23, 1 |
| Fly mode | 22, 6 |
| Flight distance | 22, 7 |
| Altitude | 22, 8 |
| Crosshairs | 13, 7 |
| Disarmed | 10, 11 |
| Warnings | Default position |

## Bench `status` baseline (2026-10-05, USB only)

```
GYRO rate 3177 Hz, cycle time 315 µs, CPU 16%, RX rate 250 Hz
Core temp 50 °C, Vref 3.27 V
GPS: M10 connected, no fix (indoors)
Arming disable flags: CLI MSP   ← expected while connected over USB
```

Arming is blocked while USB is connected (`MSP` flag). Test arming on battery only, with props off.

## Restoring

1. Flash **2026.6.2** with the build options above.
2. Paste `betaflight/diff-all.txt` into the CLI (it runs `defaults nosave` first, then `save`).
3. Check the Ports, Receiver, OSD and Video Transmitter tabs. Across firmware versions some
   settings are renamed or dropped, and the CLI only warns about rejected lines.
4. Props off: check the receiver bars, arm switch position, motor order and direction.

## Open items

- [ ] **Move VTX off R7 (5880 MHz)**: it's just outside the UK licence-free 5725–5875 MHz band.
      R3–R6 (5732–5843 MHz) are inside it
- [ ] AUX5 mid/high selects 200/500 mW: above the 25 mW licence-free limit
- [ ] Confirm OSD DISABLE on AUX4 is intended
- [ ] Set `motor_kv` to the Xing 2809 KV (still the 1960 default)
- [ ] Check `failsafe_procedure` and `dshot_bidir` defaults on 2026.6.2
- [ ] Enable bidirectional DShot (needs ESC support) so the RPM filter works
- [ ] Decide on failsafe: `DROP` vs `GPS_RESCUE` (test rescue in a safe area first)
- [ ] Decide whether airmode should stay off
- [ ] Set `bat_capacity` and check cell-voltage warnings for the pack in use
- [ ] Fill in the remaining TODOs

## History

| Tag | Description |
|---|---|
| `v1-dji-o4` | DJI O4 Air Unit Pro, HD OSD over MSP on UART1, Betaflight 4.5.2 |
| `v2-analog` | Rush Tank Ultimate Mini VTX (SmartAudio) + analog OSD (PAL), Betaflight 2026.6.2, config rebuilt from defaults |
