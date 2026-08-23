<div align="center">

**English** • [Русский](README.md)

</div>

# klipper-dwin-bridge

<p align="center">
  <a href="https://github.com/Mukller">
    <img src="https://img.shields.io/badge/Anton%20Petnitsky-Developer-0d1117?style=for-the-badge&logo=github&logoColor=white&labelColor=0d1117&color=58a6ff" alt="Anton Petnitsky" />
  </a>
</p>

Мост между Klipper и тачскрином Creality DWIN для **Ender 5 S1** на **Raspberry Pi Zero 2 W**.

Штатный экран DWIN полностью продолжает работать с Klipper/Moonraker, а вдобавок появляются
**экраны живой настройки в стиле Marlin** (Motion: steps/mm, ускорения, скорость, jerk; PID;
визуализатор сетки стола; профили филамента) — «управляй принтером с панели».

![status](https://img.shields.io/badge/status-alpha-orange) ![platform](https://img.shields.io/badge/hardware-Pi%20Zero%202W%20%2B%20DWIN%20T5UID1-blue)

## Как это работает

- Python-демон (`klipper_dwin_bridge.py`) общается со штатным экраном DWIN через GPIO UART
  (`/dev/serial0`, физические пины 8/10 = GPIO14/15) и забирает состояние / пушит переменные
  через локальный Moonraker API.
- Экран сохраняет штатные страницы прошивки Creality (Home/Print/Prepare/Settings) — перепрошивка
  DWIN_SET не нужна.
- Живая настройка в стиле Marlin реализована как дополнительные экраны VP-переменных; значения
  отображаются на эквиваленты Klipper (`rotation_distance` вместо M92, `max_velocity`/`max_accel`,
  `square_corner_velocity` вместо Jerk и т.д.).
- Запускается как systemd-юнит `klipper-dwin.service` (`python3 -u ...`, чтобы логи шли в
  `journalctl -u klipper-dwin.service -f`).

## Подключение (распиновка Pi Zero 2 W)

| Экран | Пин Pi | Примечание |
|---|---|---|
| VCC | pin 2 (5V) | |
| GND | pin 6 | |
| Screen TX | pin 10 (GPIO15, RXD) | |
| Screen RX | pin 8 (GPIO14, TXD) | |

Bluetooth на Pi Zero 2 W нужно отключить — BT занимает основной UART; `/dev/serial0` должен
резолвиться именно в него.

## Статус

- ✅ Экран грузится и остаётся подключённым между перезагрузками (`NRestarts=0`), проверено в бою 21.08.2026.
- ✅ Экраны живой настройки (Motion/PID/Bed Mesh/профили филамента) реализованы и подтверждены.
- ⚠️ Сейчас в репозитории только документация: патченный `klipper_dwin_bridge.py` работает на офлайн-Pi
  принтера; снапшот кода будет запушен, когда сетевой сегмент принтера снова станет доступен.
- Основано на альфа-концепте [FoxCraft67/Klipper-Creality-DWIN-Touchscreen-Bridge](https://github.com/FoxCraft67/Klipper-Creality-DWIN-Touchscreen-Bridge)
  (апстрим заархивирован/не поддерживается), сильно расширено под Ender 5 S1 + живую настройку.

## Автор

**Anton Petnitsky** — [github.com/Mukller](https://github.com/Mukller) · [antonpetnitsky.com](https://antonpetnitsky.com)

## Лицензия

MIT
