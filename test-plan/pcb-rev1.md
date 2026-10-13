# Rev 1 carrier board test plan (draft)

**Status:** draft. Pass numbers marked TBD need agreeing as a team, then a review with the TA.
Record results in the table at the bottom and copy measurements into [hardware/rev1.md](../hardware/rev1.md).

## Board requirements ("working" means)
1. Powers up from 5 V without damage, and survives reversed polarity.
2. The Seed boots and runs the firmware from the board's 5 V.
3. Guitar in reaches the Seed's audio input at a usable level.
4. Processed audio comes out of the headphone jack and the 1/4" line out.
5. Plugging in headphones mutes the line out.
6. Pots are read by the firmware (needs firmware support; not in the firmware yet).
7. Noise is low enough to practice with.

## Tests

### T1. Power and current draw
- **Risk:** shorts, wrong parts, regulator problems. **Checks:** req 1, 2.
- **Setup:** bench supply at 5 V with current limit at TBD mA, Seed fitted, firmware running.
- **Measure:** supply current; VREF and AMP_5V test points with a multimeter.
- **Pass:** current within TBD of the known ~200 mA; rails within TBD % of nominal.
- **Reverse polarity:** apply -5 V briefly with the current limit low; board must draw ~0 mA and work afterwards.
- **If it fails:** check the P-FET and look for hot parts with a finger or thermal camera.

### T2. Audio path, guitar to headphones and line out
- **Risk:** wrong gain staging, clipping, dead channel. **Checks:** req 3, 4.
- **Setup:** function generator (or audio interface) into the guitar input, 1 kHz sine at guitar level (TBD mV).
  Firmware in bypass (`0`) first, then each amp model.
- **Measure:** scope on headphone L/R into a 32 ohm load and on the line out. Sweep 20 Hz to 20 kHz for frequency response.
- **Pass:** no clipping at TBD input level; response flat within TBD dB from TBD Hz to TBD kHz in bypass.
- **Expected plot:** flat line in bypass; with the cab EQ on, a bump at 110 Hz and 2.5 kHz and a roll-off above 5 kHz.

### T3. Headphone mute
- **Checks:** req 5.
- **Measure:** line out level with and without a plug in the headphone jack.
- **Pass:** line out drops by at least TBD dB with headphones in.

### T4. Controls
- **Checks:** req 6.
- **Measure:** turn each pot end to end and log the ADC value over serial (needs a small firmware addition), or check the wiper voltage with a multimeter.
- **Pass:** each pot covers TBD % of the ADC range, smoothly, with no jumps.

### T5. Noise floor
- **Checks:** req 7.
- **Setup:** input shorted (or guitar cable with volume at 0).
- **Measure:** output noise with an audio interface or scope, in bypass and with each amp model.
- **Pass:** noise below TBD mV RMS (or TBD dB below full scale).

### T6. Audio callback timing
- **Checks:** req 2 (firmware runs in real time on this board).
- **Measure:** the firmware's serial status line (per-block time and overrun count) for each model.
- **Pass:** zero overruns over 5 minutes of playing.

### T7. Polarity and latency
- **Checks:** req 4 (audio feels real time to play through).
- **Setup:** firmware in bypass (`0`) first, then each amp model. Scope channel 1 on the guitar input, channel 2 on
  the headphone or line out.
- **Input:** a short impulse or a sharp edge, not a sustained note.
- **Measure:** time from the impulse to the centre of the output's main peak, and whether that peak is inverted.
- **Expected:** output inverted; about 3.3 ms delay. See [hardware/rev1.md](../hardware/rev1.md#audio-phase-and-latency).
- **Pass:** delay below TBD ms.

## How this differs from the EXPO build
- Bench supply instead of the battery and power board.
- No touchscreen yet; models are switched over USB serial.
- No AUX input and no speaker amp on rev 1.

## Results

| Test | Date | Result | Notes |
| --- | --- | --- | --- |
| T1 current draw | | ~200 mA at 5 V | Measured on bench supply, limits still TBD |
| T2 | | | |
| T3 | | | |
| T4 | | | |
| T5 | | | |
| T6 | | | |
| T7 | 2026-10-13 | ~3.3 ms, inverted | Impulse on a Keysight DSO-X 3024G; amp model state not recorded. [Capture](../hardware/images/rev1-impulse-latency.png) |
