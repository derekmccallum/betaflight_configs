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

## Mobula8 — Components

This is the **HD** version (CrazyF405HD board with no analog OSD). The FC runs Betaflight 2025.12.2 on target `CRAZYBEEF405`.

| Component | Detail |
|---|---|
| **Frame** | Mobula8 85mm whoop frame (1.5mm CF / plastic ducts) |
| **FC** | CrazyF405HD ELRS AIO — STM32F405RGT6, BMI270 (SPI), BMP280 baro, 8MB blackbox |
| **ESC** | 4-in-1 12A (15A peak) BLHeli\_S / Bluejay, DSHOT600 |
| **RX** | Built-in UART ExpressLRS 2.4GHz (ELRS V3.0, CRSF) |
| **Motors** | Happymodel EX1103 11000KV (9N12P, 1.5mm shaft, 3.8g) |
| **Props** | Gemfan Hurricane 2023 tri-blade (2.3" pitch, PC) |
| **Battery** | 2S LiPo/LiHV 450-530mAh (XT30) |
| **Camera** | DJI O4 Lite |
| **Mount** | 25.5x25.5mm (whoop pattern) |

---

## OSD & Modes — CLI Setup

Paste these commands into the Betaflight Configurator CLI tab.

### Common (all builds)

```
# Modes
aux 0 0 0 900 1200 0 0    # ARM on AUX1 (CH5, low)
aux 1 1 1 1300 1700 0 0   # ANGLE on AUX2 (CH6, mid)
aux 2 2 1 1700 2100 0 0   # HORIZON on AUX2 (CH6, high)
aux 3 13 2 1300 2100 0 0  # BEEPER on AUX3 (CH7, mid+)
aux 4 19 3 1300 2100 0 0  # OSD DISABLE SWITCH on AUX4 (CH8, mid+)

# OSD — general
set osd_units = METRIC
set osd_warn_bitmask = 286719
set osd_rssi_alarm = 20
set osd_link_quality_alarm = 80
set osd_rssi_dbm_alarm = -60
set osd_rsnr_alarm = 4
set osd_cap_alarm = 2200
set osd_alt_alarm = 100
set osd_core_temp_alarm = 70
set osd_ah_max_pit = 20
set osd_ah_max_rol = 40
set osd_ah_invert = OFF
set osd_logo_on_arming = OFF
set osd_framerate_hz = 12
set osd_menu_background = TRANSPARENT

# OSD — element positions (whoop layout)
set osd_vbat_pos = 2344
set osd_throttle_pos = 2472
set osd_flymode_pos = 3368
set osd_crosshairs_pos = 2361
set osd_ah_sbar_pos = 313
set osd_disarmed_pos = 2614
set osd_ready_mode_pos = 1347
set osd_warnings_pos = 14932
set osd_core_temp_pos = 2376
set osd_camera_frame_pos = 142
set osd_up_down_reference_pos = 312
```

### Chimera 7 additions (GPS long-range)

```
set osd_gps_sats_pos = 3144
set osd_home_dir_pos = 2586
set osd_home_dist_pos = 2132
set osd_flight_dist_pos = 2141
set osd_esc_tmp_pos = 2408
set osd_altitude_pos = 18548
set osd_power_pos = 2344
set osd_canvas_width = 53
set osd_canvas_height = 20
```

Position values are Betaflight's coordinate encoding (`row * 100 + col`). 341 = hidden (default). Adjust positions in the OSD tab to your preference — these are starting layouts, not a pixel-perfect overlay.
