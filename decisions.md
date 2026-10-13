# Decisions

Newest first. One entry per decision: what we decided, why, and the date.

## 2026-10-13: All audio processing stays on the Seed
- **Decision:** every effect runs on the Seed. The touchscreen only sends settings and shows readings.
- **Why:** the guitar signal only passes through the Seed. The touchscreen board has no audio input or output
  and one spare pin, the link is far too slow to carry audio both ways, and its CPU is busy driving the display.

## 2026-10-13: No fake controls on the touchscreen
- **Decision:** a control stays in the UI only if the Seed acts on it. Until an effect exists on the Seed, the
  "Seed controls only" mode hides it; effects the Seed won't run get removed from the UI.
- **Why:** the sponsor wants real readings only, and a knob that does nothing misleads anyone testing or
  watching a demo. See [touchscreen/README.md](touchscreen/README.md).

## 2026-10-01: Touchscreen talks to the Seed over UART
- **Decision:** 3-wire UART at 1,000,000 baud with checksummed packets, on the carrier's J4 "DISPLAY_UART" header.
- **Why:** the carrier already has the header; the touchscreen board has no free pins for anything else; 1 Mbaud
  is plenty for settings and uploads a 7.5 KB amp profile in under 0.1 s. Details:
  [touchscreen/seed-link.md](touchscreen/seed-link.md).

## 2026-10-13: Knowledge base in this repo, as plain Markdown
- **Decision:** keep shared team knowledge in `capstone-documentation` as Markdown files, not the GitHub Wiki.
- **Why:** changes can be reviewed in pull requests, files like PDFs and images can live next to the pages,
  and anyone can edit in the browser.

## 2026-10-13: Rev 2 is a new KiCad project beside rev 1
- **Decision:** in `seed3-carrier-board`, rev 1 moves to `rev1/` and rev 2 is a new project in `rev2/`.
  Footprint and symbol libraries stay at the repo root, shared by both.
- **Why:** rev 1 stays intact for reference and reorders, and both boards use the same parts libraries.
- Change list: [hardware/rev2.md](hardware/rev2.md).

## Cabinet is an EQ, not an impulse response
- **Decision:** the firmware runs a fixed six-biquad speaker EQ after the amp model instead of a cabinet IR.
- **Why:** the models capture the amp alone and sound harsh without a speaker; a fixed EQ is cheap enough for
  the 1 ms audio budget. See the firmware README.
