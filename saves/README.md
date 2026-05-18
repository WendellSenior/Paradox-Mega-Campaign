# Saves

Save archives from the dry run and the live campaign.

Save binaries (`.rome`, `.ck3`, `.eu4`, `.v3`, `.hoi4`, plus `.zip`/`.7z` bundles) are gitignored by default — they get large fast and git handles binaries poorly. Folder structure is tracked so the layout stays consistent.

If you want to version-control saves long-term (recommended for the dry run and milestone saves), set up [Git LFS](https://git-lfs.github.com/) and add the relevant extensions to LFS tracking.

## Layout

- `dry-run/` — saves from the singleplayer chain validation
- `campaign/` — saves from the live multiplayer campaign
