# zmk-redox

ZMK config for a wireless Redox split keyboard (nice!nano v2) with a PandaKB nRF52840 dongle as central, a 1.3" OLED status display and a Danish layout.

The keyboard is based on an earlier version of the falbatech PCBs (with the contoller centered) where i substituted the pro micro for nice nanos with a battery.

The dongle in question is a prebuilt PandaKB ZMK Dongle, which utilizes a nRF52840 and a 1.3" SH1106 OLED, SDA P0.17 / SCL P0.20

## Hardware

| Role | Board | Notes |
|---|---|---|
| Dongle (central) | PandaKB nRF52840 (built as `nice_nano_v2`) | 1.3" SH1106 OLED, SDA P0.17 / SCL P0.20 ✓ |
| Left half | nice!nano v2 | peripheral |
| Right half | nice!nano v2 | peripheral |

Layout: Danish (`keys_da.h`), 4 layers.

## Build artifacts

Trigger **Actions → Build → Run workflow** to produce:

- `redox_dongle` — flash to PandaKB dongle
- `redox_left` — flash to left half
- `redox_right` — flash to right half
- `settings_reset` — wipe BLE pairings (flash to each controller when needed)

## Flashing order

> Always flash `settings_reset` to all three controllers before first use or when re-pairing.

1. Flash `settings_reset` to **dongle**, left, and right (in any order). Wait for each to reboot.
2. Flash `redox_left` to left half.
3. Flash `redox_right` to right half.
4. Flash `redox_dongle` to dongle **last**, with the left half plugged in nearby (powered on).

To enter flash mode: double-press the reset button. The controller appears as a USB drive.

## Notes

- `build.yml` uses `zmkfirmware/zmk@v0.3`. GitHub removes Node.js 20 from runners **Sept 16 2026** (forced from June 2 2026). Check for a new ZMK release and bump the tag before then — see comment in `.github/workflows/build.yml`.
- Display uses [englmaxi/zmk-dongle-display](https://github.com/englmaxi/zmk-dongle-display) (`dongle_display` shield) for the status screen (bongo cat, layer, peripheral battery, modifiers).
- OLED pin mapping confirmed: SDA P0.17, SCL P0.20 via `pro_micro_i2c`.

## License

MIT — see [LICENSE](LICENSE). `config/keys_da.h` is generated from Unicode CLDR data and carries its own Unicode license notice.
