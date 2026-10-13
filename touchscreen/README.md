# Touchscreen

Repo: [esp32-touchscreen-ui](https://github.com/B1narySolutions/esp32-touchscreen-ui). Its `docs/HANDOFF.md`
has the full picture; `docs/` also has the link protocol, the Seed integration guide and the lab checklist.

- Board: Waveshare ESP32-S3-Touch-LCD-7B (7", 1024 x 600, GT911 touch, 8 MB PSRAM, 16 MB flash).
- Firmware: ESP-IDF v5.5.5 and LVGL 9. Builds on Windows and macOS (`idf.py build`, flash over the "USB" port).
- What it does: the whole UI (effects chain, amp picker, knobs, presets, master and mute), a 10k pot as master
  volume, loads TONE3000 A2 amp profiles from its microSD card, and sends all of it to the Seed over a 3-wire
  UART link ([seed-link.md](seed-link.md)). It shows the Seed's DSP load, active model and IN/OUT meters.
- **It never touches the audio.** The guitar signal only goes through the Seed; the touchscreen sends settings.
- **No fake data.** Every value on screen is a real reading or "-" / "Not connected". Anything from a PC
  simulator is labelled MOCK.

## Seed controls only mode
Settings > "Seed controls only" swaps the effects rail and panel for one page with only what the Seed acts on
today: amp model (3 built-ins and SD profiles), amp on/bypass, master and mute, plus the link, DSP load and
meters. The other effects keep their settings and come back when the mode is off. Use it for demos until the
Seed runs more effects.

## What the Seed acts on (2026-10-13)
With the proposed Seed integration ([seed-link.md](seed-link.md)): master volume, mute, amp model, amp bypass.
Everything else (gate, comp, EQ, drive, amp tone knobs, octave, chorus, phaser, tremolo, delay, reverb, cab
cut/level) is sent but ignored by the Seed until someone writes those effects. The cabinet EQ is always on.

## Gotchas
- **Startup hang:** opening the "USB" port within ~1.5 s of a reset can stop the board at "Enter psram timing
  tuning". Press RESET. `idf.py flash monitor` can trigger it; the repo's tools wait for you.
- **Switch SW1:** UART2 sends the link to header H3 (the real Seed); UART1 sends it to the "USB TO UART" port
  (PC simulator).
- **Internal RAM is the tight resource** (about 49 KB free at runtime). Check `heap_min` in the `DIAG` line after
  any UI change.
- On a Mac, opening the "USB" port resets the board.
- The microSD card must be FAT32 on an MBR partition table.
- `.nam` files and anything with model weights are T3K-licensed: never commit them.

## Open items
- Decide which effects the Seed will run. The sponsor's list (reverb, delay, distortion, compression, chorus,
  noise gate, see [requirements](../project/requirements.md)) uses different controls than the UI has today
  (for example compression: level, attack, sustain). Effects that won't be built get removed from the UI.
- The Wi-Fi and Bluetooth switches in Settings don't control any radio yet. Wire them up or remove them.
- STEMMA QT encoder driver is written but untested.
