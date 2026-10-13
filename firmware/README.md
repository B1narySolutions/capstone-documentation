# Firmware

Repo: [realtime-nam-seed3](https://github.com/B1narySolutions/realtime-nam-seed3). Its README has setup,
build and flashing steps.

- Runs on the Daisy Seed (STM32H7, 480 MHz) with libDaisy.
- Three neural amp models (A2-Lite): Fender '65 Twin Reverb, Vox AC30, Marshall JCM800.
- 48 kHz audio, 48-sample blocks, so each audio callback has a 1 ms budget.
- Left input goes to both outputs. A fixed cabinet EQ follows the model (toggle with `C`).
- Today, models are picked over USB serial: `0` bypass, `1` Fender, `2` Vox, `3` Marshall, `B` reboot to DFU.

## Gotchas
- The model weights are T3K-licensed. Never commit `models/local/` or firmware binaries that contain them.
- `make test` downloads the models from api.tone3000.com, so it needs that site reachable.
- Speed notes and rejected experiments are in `docs/performance.md` in the firmware repo.
