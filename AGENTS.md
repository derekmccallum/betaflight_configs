# Betaflight Configs

Personal Betaflight CLI backups for each quadcopter build.

## Structure

- Each quad has its own top-level directory.
- Config files are raw Betaflight CLI outputs (`dump all` or `diff all`) saved from the Configurator CLI tab.
- Accompanying files (VTx tables, 3D-printed part STLs) live in the same directory.

## Naming

Config dumps follow: `BTFL_cli_[QUAD_NAME]_[YYYYMMDD]_[HHMMSS]_[BOARD_NAME].txt`

Example: `BTFL_cli_MOBULA8_20260531_115351_CRAZYBEEF405.txt`

## Adding a new quad

1. Create a `quad_name/` directory.
2. Run `dump all` or `diff all` in Betaflight Configurator CLI, save as `BTFL_cli_<name>_<date>_<time>_<board>.txt`.
3. Add any VTx table exports, Lua scripts, or STL files alongside.
4. No build/CI — pure asset storage. Commit directly.

## Contents

| Directory | Quad | Board | BF Version |
|---|---|---|---|
| `darwinfpv_foldape4/` | FoldApe4 | DARWINF4SX1280HD | 4.4.2 |
| `happymodel_mobula8/` | Mobula8 | CRAZYBEEF405 | 2025.12.2 |
| `iflight_chimera_7/` | Chimera 7 | SPEEDYBEEF7V3 | 4.5.2 |
