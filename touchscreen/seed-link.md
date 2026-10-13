# Touchscreen to Seed link

How the touchscreen controls the Seed. Full byte layouts: `docs/SEED_LINK_PROTOCOL.md` in
[esp32-touchscreen-ui](https://github.com/B1narySolutions/esp32-touchscreen-ui).

## Status (2026-10-13)
- **Touchscreen side:** done, tested against two PC stand-ins for the Seed.
- **Seed side:** not in [realtime-nam-seed3](https://github.com/B1narySolutions/realtime-nam-seed3) yet. Until it
  is, the Seed doesn't read its UART pins and the touchscreen says "Not connected".
- **Request for Rocco:** `docs/SEED_FIRMWARE_REQUEST.md` with a ready patch, `docs/seed3-link.patch`. The patch
  compiles and links against the Seed repo (230eb74, libDaisy cc146d5): flash use goes from 78.9 % to 85.2 %.
  It hasn't run on a Seed yet.

## Wiring
3.3 V logic both sides, no level shifting. **Three wires only, boards powered off while wiring.** Set SW1 on the
touchscreen to UART2.

| Touchscreen header H3 "UART2" | Seed carrier J4 "DISPLAY_UART" | Bare Seed |
| --- | --- | --- |
| pin 2 GND | pin 1 GND | DGND (pin 40) |
| pin 3 RX (GPIO44) | pin 2 Seed TX | D13 (pin 14), USART1 TX |
| pin 4 TX (GPIO43) | pin 3 Seed RX | D14 (pin 15), USART1 RX |
| pin 1 3V3: **leave empty** | pin 4 +5V: **leave empty** | |

> **Never a straight 4-wire cable:** it joins the carrier's +5 V to the touchscreen's 3.3 V rail.
> Check the bare-Seed pin numbers against the Seed's pinout card before wiring.

## How it works
- UART, 1,000,000 baud, 8N1, no flow control.
- Small binary packets. Each has a header (type, sequence number, length), the data, and a CRC-16 checksum, and
  packets are separated by a `00` byte (COBS encoding). A damaged packet is dropped, never trusted.
- **Touchscreen to Seed:** HELLO on start-up, then the full setup at once (a snapshot: chain, amp, every knob,
  master, mute), then only changes. Slider drags are limited to 100 updates per second; mute goes at once. The
  full setup is resent every 5 s, after a preset load and after a Seed reboot, so a lost packet fixes itself.
- **Seed to touchscreen:** HELLO (firmware version, built-in amp names), STATUS twice a second (DSP load,
  overruns, active model), METERS 30 times a second (IN/OUT peaks).
- **Both ways:** heartbeat every second; 3 s of silence shows "link lost". PING/PONG measures the round trip.
- **SD amp profiles:** the touchscreen converts TONE3000 A2 `.nam` files and uploads the weights (7484 bytes) in
  acknowledged chunks; the Seed checks a CRC-32 before using them.
- On the Seed, bytes arrive by DMA and are decoded in the main loop, never in the audio callback.

## Tested (2026-10-13, Jacob, touchscreen + Mac)
| What | Result | How |
| --- | --- | --- |
| Protocol library | 51,022 checks, 0 failures | Host tests with sanitizers on macOS (`tools/run_host_tests.sh`) |
| Seed's own receiver code vs the real touchscreen | 21 min, 252 of 252 snapshots confirmed, no link drops, errors only at connect | `seed_sim` on the Mac through the USB TO UART port (SW1 on UART1) |
| SD profile upload | Marshall JCM800 G5 A2 uploaded, CRC verified by the receiver | Same setup |
| Touchscreen with no Seed | "Not connected", 0 bytes received, 0 errors | Boot log and `DIAG` line |
| Seed firmware + patch | Compiles and links, +8.2 KB flash | Arm GNU 15.3.rel2 on a copy of the Seed repo |

Not tested yet: anything on the real Seed (wiring, sound of mute and volume, meters, DSP load with the link).

## Bring-up day
1. Rocco flashes the Seed build with the link.
2. Wire it (table above), SW1 on UART2, power on.
3. Both USB cables into one computer, then `python tools/lab_day.py` from the touchscreen repo. It reads both
   consoles, sends nothing, and prints a live checklist with the next thing to check.
4. Follow `docs/LAB_DAY_CHECKLIST.md`: mute, master, amp cards, meters, SD profile, Seed reboot, cable pull.

Don't type into the Seed's USB console: `0`-`3` switch amps, `C` toggles the cab, `B` reboots it into DFU.
