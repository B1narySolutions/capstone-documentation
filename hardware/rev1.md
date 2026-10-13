# Carrier board rev 1

KiCad project: [seed3-carrier-board](https://github.com/B1narySolutions/seed3-carrier-board) (`rev1/`).
Fabrication files: `fabrication/2026-09-15-e7ddc60/` in that repo.

## What's on the board
- Guitar input (1/4"), buffered, with an input-level pot
- Daisy Seed sockets (Seed Rev4 footprint)
- Headphone amp (TPA6111A2) on a 1/8" jack
- 1/4" line out, muted in hardware by the headphone jack's switch
- Volume and tone pots
- 4-pin display UART header
- 2-pin 5 V power header with P-FET reverse-polarity protection
- Test points: VREF, AMP_5V
- Size 95.8 x 66.2 mm, 2 layers

## Measurements

Add a row whenever you measure something. Say how you measured it.

| Date | What | Value | How | Who |
| --- | --- | --- | --- | --- |
| 2026 (fall) | Current draw at 5 V | ~200 mA | Benchtop supply set to 5 V, supply's current readout | Rocco |

## Known issues
- No AUX input yet (required by the spec).
- 5 V header is unkeyed; it can be plugged in backwards (the P-FET protects the board).
- Display header carries 5 V and is unkeyed.
- No mounting holes.
- A mono plug in the headphone jack shorts the right channel through 4.7 ohms.
- Seed3 pinout vs the Rev4 footprint (VIN pin 39, audio pins 16-19) still needs confirming.

These are all on the [rev 2 list](rev2.md).
