# Touchscreen to Seed link test plan (draft)

**Status:** draft. Run once the Seed firmware has the link ([touchscreen/seed-link.md](../touchscreen/seed-link.md)).
Pass numbers marked TBD need agreeing as a team. Setup for every test: touchscreen and Seed wired with three
wires, SW1 on UART2, both USB cables in one computer running `tools/lab_day.py`.

## Requirements ("working" means)
1. The touchscreen controls master volume, mute and the amp model on the Seed.
2. Every number the touchscreen shows about the Seed is a real reading.
3. The link never disturbs the audio.
4. The link recovers by itself from a Seed reboot or a pulled cable.

## Tests

### L1. Link up
- **Risk:** crossed wires, wrong switch, wrong baud. **Checks:** req 1.
- **Measure:** `lab_day.py` checklist; Test Mode shows Connected with the Seed's firmware version.
- **Pass:** connected within 1 s of both running; link errors at most 2 (at connect) over 10 min.
- **If it fails:** swap the two signal wires, check SW1, check both sides at 1,000,000 baud.

### L2. Controls reach the Seed
- **Checks:** req 1.
- **Measure:** tap each amp card (Seed console prints `>>> ... loaded`); tap MUTE while playing; drag MASTER.
- **Pass:** amp switches every time; mute silences within TBD ms; no zipper noise on MASTER.

### L3. Readings are real
- **Checks:** req 2.
- **Measure:** compare the touchscreen's DSP load and model with the Seed's own console line (`lab_day.py` does
  this); play the guitar and watch the IN/OUT meters.
- **Pass:** DSP load within 3 percentage points of the Seed's console; meters move with playing and read "-"
  when the cable is pulled.

### L4. Audio unaffected
- **Checks:** req 3.
- **Measure:** the Seed's console DSP load and overruns for 2 min with the cable connected and the touchscreen
  being used, and for 2 min without the cable.
- **Pass:** average DSP load differs by at most TBD %; zero new overruns.

### L5. Recovery
- **Checks:** req 4.
- **Measure:** reset the Seed; pull the cable for 5 s and plug it back.
- **Pass:** after a Seed reset the touchscreen resends everything (and re-uploads a selected SD profile) within
  1 s; after a cable pull it shows "link lost" then "link back", and the Seed keeps sounding the same throughout.

## Results

| Date | Test | Result | Notes | Who |
| --- | --- | --- | --- | --- |
| 2026-10-13 | L1 | Pass | Link up in about 1 s; 0 link errors over about 2 min. Bare Seed, test build (c3abe82 + patch) | Jacob |
| 2026-10-13 | L2 | Partial | Amp switched on 5 of 5 taps; mute took the output meter to silence. Not listened to: no headphones or guitar on the bare Seed | Jacob |
| 2026-10-13 | L3 | Pass (DSP, model) | DSP 57.2 % vs 57 % on the Seed console; meters show the real noise floor. Not yet with a guitar playing | Jacob |
| 2026-10-13 | L4 | Not run | DSP 57 % with the link code vs 56 % without it; with vs without the cable not compared yet | Jacob |
| 2026-10-13 | L5 | Pass | Seed reset: full resend at once. Cable pulls: link lost, then back, on both boards | Jacob |
