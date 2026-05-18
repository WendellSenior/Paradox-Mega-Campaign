# Admin

Lightweight tooling and templates for running the campaign.

- `templates/` — AAR template, session prep checklist, regent standing-orders form, etc.
- `scripts/` — any automation (credit recalculation, roster sync, etc.)

Live operational data (credits, attendance, roster) lives in `docs/_data/` so the website renders directly from the source of truth — no shadow copies in this folder.

## Adding a map

Drop the image file into `docs/assets/maps/` using this filename convention:

`<Game>_Session<N>_<Year>.<ext>`

- `<Game>` — one of: `Imperator`, `CK3`, `EU4`, `Vic3`, `HoI4` (case-sensitive; must match a key in `docs/_data/games.yml`)
- `<N>` — session number, no leading zeros (e.g. `Session1`, `Session10`)
- `<Year>` — in-game year. Imperator uses AUC (Ab Urbe Condita; AUC 1 = 753 BCE); all other games use CE
- `<ext>` — `png`, `jpg`, `jpeg`, or `webp`

Examples:

- `Imperator_Session1_635.png` (AUC 635 ≈ 119 BCE)
- `CK3_Session5_1134.png`
- `EU4_Session12_1612.jpg`

Push the file. Within a minute the map appears in the [single-map viewer](https://wendellsenior.github.io/Paradox-Mega-Campaign/maps/) and on the [all-maps page](https://wendellsenior.github.io/Paradox-Mega-Campaign/maps/all/). Game ordering follows the chain order in `docs/_data/games.yml`; sessions within a game are sorted by number.

Maps render in a fixed-aspect-ratio frame (16:9 by default) so all maps are visually the same size regardless of source resolution. To change the frame ratio, edit `aspect-ratio` in `docs/assets/css/maps.css`.
