# Capstone documentation

Team knowledge base for the portable touchscreen practice guitar amp. If you learn something the rest of the
team should know (a measurement, a decision, a gotcha), add it here.

## Pages

| Page | What's in it |
| --- | --- |
| [project/requirements.md](project/requirements.md) | What the sponsor asked for, and course deadlines |
| [decisions.md](decisions.md) | Running log of decisions and why we made them |
| [hardware/rev1.md](hardware/rev1.md) | Rev 1 carrier board: what's on it and what we've measured |
| [hardware/rev2.md](hardware/rev2.md) | Rev 2 change list |
| [firmware/README.md](firmware/README.md) | Firmware overview and where to find it |
| [touchscreen/README.md](touchscreen/README.md) | Touchscreen board: what it does, Seed controls only mode, gotchas |
| [touchscreen/seed-link.md](touchscreen/seed-link.md) | Touchscreen to Seed link: wiring, how it works, status, bring-up |
| [test-plan/README.md](test-plan/README.md) | What the course needs in a test plan |
| [test-plan/pcb-rev1.md](test-plan/pcb-rev1.md) | Draft test plan for the rev 1 board |
| [test-plan/seed-link.md](test-plan/seed-link.md) | Draft test plan for the touchscreen to Seed link |

## Repositories

- [realtime-nam-seed3](https://github.com/B1narySolutions/realtime-nam-seed3): Daisy Seed firmware (neural amp models)
- [seed3-carrier-board](https://github.com/B1narySolutions/seed3-carrier-board): KiCad carrier board (rev1/ and rev2/)
- [esp32-touchscreen-ui](https://github.com/B1narySolutions/esp32-touchscreen-ui): touchscreen firmware (ESP32-S3, UI and the link to the Seed)
- [capstone-documentation](https://github.com/B1narySolutions/capstone-documentation): this knowledge base

## How to add something

- Small fix or new fact: edit the page on GitHub (pencil icon) and commit.
- Bigger change: make a branch and open a pull request so someone can look at it.
- Write down **how you know** a number (what you measured, with what, on which board) and the date.
- Put PDFs and images next to the page that uses them, for example `hardware/images/`.
