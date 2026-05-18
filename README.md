# Paradox Mega-Campaign

Multi-year multiplayer chain through five Paradox grand strategy games: Imperator: Rome → CK3 → EU4 → Victoria 3 → HoI4. Roughly 30 players, ~2-3 year real-time horizon.

This repo doubles as project workspace and source for the campaign website hosted via GitHub Pages.

## Layout

- `docs/` — published site (GitHub Pages serves from here)
- `planning/` — organizer working docs: design drafts, dry-run plan, playbook, decision log
- `admin/` — templates and scripts for running the campaign
- `converters/` — version-pinned converter configs (binaries gitignored)
- `saves/` — save archives (binaries gitignored; folder structure tracked)

See `CLAUDE.md` for full project context, locked design decisions, and open questions.

## Publishing

GitHub Pages → Settings → Pages → Source: `main` branch, `/docs` folder. Push, wait ~1 minute, site is live.
