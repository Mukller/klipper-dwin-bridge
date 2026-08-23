<div align="center">

[English](README_EN.md) • **Русский**

</div>

# klipper-dwin-bridge

<p align="center">
  <a href="https://github.com/Mukller">
    <img src="https://img.shields.io/badge/Anton%20Petnitsky-Developer-0d1117?style=for-the-badge&logo=github&logoColor=white&labelColor=0d1117&color=58a6ff" alt="Anton Petnitsky" />
  </a>
</p>

Klipper ↔ Creality DWIN touchscreen bridge for the **Ender 5 S1** running on a **Raspberry Pi Zero 2 W**.

Keeps the stock DWIN screen fully working with Klipper/Moonraker and adds **Marlin-style live tuning screens** (Motion: steps/mm, acceleration, speed, jerk; PID; Bed Mesh visualizer; filament profiles) — "control the printer from the panel".

![status](https://img.shields.io/badge/status-alpha-orange) ![platform](https://img.shields.io/badge/hardware-Pi%20Zero%202W%20%2B%20DWIN%20T5UID1-blue)

## How it works

- A Python daemon (`klipper_dwin_bridge.py`) talks to the stock DWIN screen over GPIO UART (`/dev/serial0`, physical pins 8/10 = GPIO14/15) and pulls state / pushes variables through the local Moonraker API.
- The screen keeps its stock Creality firmware pages (Home/Print/Prepare/Settings) — no DWIN_SET reflash needed.
- Marlin-style live tuning is implemented as extra VP-variable screens; values map to Klipper equivalents (`rotation_distance` instead of M92, `max_velocity`/`max_accel`, `square_corner_velocity` instead of Jerk, etc.).
- Runs as a systemd unit `klipper-dwin.service` (`python3 -u ...` so logs stream to `journalctl -u klipper-dwin.service -f`).

## Wiring (Pi Zero 2 W header)

| Screen | Pi pin | Note |
|---|---|---|
| VCC | pin 2 (5V) | |
| GND | pin 6 | |
| Screen TX | pin 10 (GPIO15, RXD) | |
| Screen RX | pin 8 (GPIO14, TXD) | |

Bluetooth must be disabled on the Pi Zero 2 W — BT occupies the primary UART; `/dev/serial0` has to resolve to it.

## Status

- ✅ Screen boots and stays connected across reboots (`NRestarts=0`), verified in production on 2026-08-21.
- ✅ Live tuning screens (Motion/PID/Bed Mesh/filament profiles) implemented and confirmed working.
- ⚠️ This repo currently ships documentation only. The patched `klipper_dwin_bridge.py` runs on the printer's offline Pi; a code snapshot will be pushed once the printer's network segment is reachable again.
- Based on the alpha [FoxCraft67/Klipper-Creality-DWIN-Touchscreen-Bridge](https://github.com/FoxCraft67/Klipper-Creality-DWIN-Touchscreen-Bridge) concept (upstream archived/unmaintained), extended heavily for Ender 5 S1 + live tuning.

## Author

**Anton Petnitsky** — [github.com/Mukller](https://github.com/Mukller) · [antonpetnitsky.com](https://antonpetnitsky.com)

## License

MIT
