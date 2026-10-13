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
| 2026-10-13 | Output polarity, guitar in to output | Inverted | Scope on input and output with a single guitar note; output had to be inverted to line up | Rocco |
| 2026-10-13 | Delay, guitar in to output | ~4.4 ms | Same scope setup, matching peaks after inverting one trace. Amp model state not recorded; bypass retest pending (see below) | Rocco |

## Audio phase and latency

**Polarity.** The board inverts the audio once. The TPA6111A2 headphone amp is wired as an inverting
amplifier with a gain of -1 (R9/R11 = 20k/20k), and the line out is fed from its left output through the
headphone jack's switch (J5 TN to R3 to J2). The measured inversion matches this, which suggests the Seed
module itself does not invert.

**Analog phase shift** from the coupling caps and filters, worked out from the KiCad netlist (not measured).
It is small across the audio band:

| Stage | Parts | 100 Hz | 1 kHz | 10 kHz |
| --- | --- | --- | --- | --- |
| Input buffer | C3 1 uF into RV1 1M, TLV9062 follower | +0.1° | 0° | 0° |
| Output low-pass (48 kHz) | R2 3.3k, C2 1 nF | -0.1° | -1.2° | -11.7° |
| Output buffer | C5 1 uF, R6 1M, follower | +0.1° | 0° | 0° |
| Headphone amp input | C12 4.7 uF, R9 20k | +1.0° | +0.1° | 0° |
| Output cap, 32 ohm headphones | C14 470 uF, R13 100, R15 4.7 | +7.2° | +0.7° | +0.1° |
| Output cap, line out (10k+ load) | Same | +2.0° | +0.2° | 0° |
| **Total, headphones** | Plus the 180° inversion | 180° + 8° | 180° - 0.4° | 180° - 12° |
| **Total, line out** | Plus the 180° inversion | 180° + 3° | 180° - 0.9° | 180° - 12° |

**Where the delay comes from.** The analog parts add microseconds at most. The estimate is about 3 ms, from:
- 1 ms collecting a 48-sample input block before the audio callback runs (libDaisy double-buffered DMA).
- 1 ms because the processed block plays out during the next DMA half.
- Roughly 0.5-1 ms of ADC and DAC digital filter delay in the Seed's codec (not yet checked against the datasheet).

The callback's compute time (~550 us) does not add to the delay as long as it finishes inside the 1 ms block.
Latency is about 2 x block size + codec delay, so 24-sample blocks would save about 1 ms.

**Why 4.4 ms and not 3 ms is still open.** Two measurement effects could explain the difference:
- Matching peaks on a plucked note can slip by a whole cycle, because the note builds up over its first few
  cycles (an open high E is about 3 ms per cycle).
- With an amp model on, distortion and the cab EQ move where the peaks land.

**Pending:** retest in bypass (`0`), comparing onsets (where each trace first leaves zero) instead of peaks, with a
sharp edge such as muted strings or a tapped cable tip. If bypass still reads about 4.4 ms, check the codec
delays for our Seed revision.

## Known issues
- No AUX input yet (required by the spec).
- 5 V header is unkeyed; it can be plugged in backwards (the P-FET protects the board).
- Display header carries 5 V and is unkeyed.
- No mounting holes.
- A mono plug in the headphone jack shorts the right channel through 4.7 ohms.
- Seed3 pinout vs the Rev4 footprint (VIN pin 39, audio pins 16-19) still needs confirming.

These are all on the [rev 2 list](rev2.md).
