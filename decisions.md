# Decisions

Newest first. One entry per decision: what we decided, why, and the date.

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
