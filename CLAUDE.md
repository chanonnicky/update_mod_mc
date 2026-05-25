# CLAUDE.md — update_mod_mc

## Project Overview

This repository distributes versioned Minecraft modpack profiles for a private server ("MC Server 29-05-2024 Mod"). It is not a code project — there are no build scripts, tests, or CI pipelines. The primary workflow is adding new modpack versions and keeping the version registry up to date.

The current modpack theme is **Create Essentials** (major version 6.x): a Minecraft Fabric/Forge profile focused on the Create mod and its addons.

---

## Repository Structure

```
update_mod_mc/
├── .gitignore                          # Ignores .DS_Store and *.csv
├── README.md
└── MC Server 29-05-2024 Mod/
    ├── meta.json                       # Version registry (source of truth)
    └── versions/
        ├── 1.0.0/
        │   ├── changelog.txt
        │   └── profile.zip
        ├── 1.1.7/
        │   ├── changelog.txt
        │   └── profile.zip
        └── ... (one folder per version)
```

Every version folder contains exactly two files:
- **`changelog.txt`** — human-readable notes on what changed in this version
- **`profile.zip`** — the Minecraft launcher profile archive players import

---

## Key Files

### `MC Server 29-05-2024 Mod/meta.json`

The canonical version registry. Schema version is `1`. All released versions must be listed here in order.

```json
{
    "schemaVersion": 1,
    "hotfixesFormat": null,
    "versions": [
        {
            "id": "6.1.2",
            "releasedAt": "04-02-2026"
        }
    ]
}
```

- **`id`** — version string in `X.Y.Z` format (see versioning conventions below)
- **`releasedAt`** — release date in `DD-MM-YYYY` format
- Versions are listed in ascending order (oldest first)

---

## Versioning Conventions

The version scheme is `MAJOR.MINOR.PATCH`:

| Segment | Meaning |
|---------|---------|
| MAJOR   | Modpack era / major theme change (e.g. switching to Create 6.0 moved to 6.x) |
| MINOR   | Significant mod additions or server configuration overhaul |
| PATCH   | Small mod additions, removals, or fixes within the same minor series |

Current active series: **6.1.x** (Create Essentials — Create 6.0 + addons)

Historical series for reference:
- `1.x.x` — initial server launch (Jun 2024)
- `2.x.x` — second era (Sep 2024)
- `3.x.x` — third era (Sep–Oct 2024)
- `4.x.x` — fourth era (Nov 2024)
- `5.x.x` — fifth era (Apr–Jun 2025)
- `6.x.x` — Create Essentials era (Feb 2026–present)

---

## Changelog Format

Every `changelog.txt` follows this plain-text format:

```
[Modpack Theme / Series Name]

Mods:
- Mod added or removed (brief description if needed)
- Another mod
```

Example from `6.1.2/changelog.txt`:
```
[Create Essentials - Create 6.0  Better Experience with Create and Create Addons]

Mods:
- Epic Fight
- Not Enough Animation
```

Keep entries short. List mods added or removed in that specific version — not the full mod list.

---

## Adding a New Version (Standard Workflow)

1. **Prepare the profile** — build/export the Minecraft launcher profile as `profile.zip`.

2. **Create the version folder:**
   ```
   MC Server 29-05-2024 Mod/versions/<new-version>/
   ```

3. **Add the two required files:**
   - `changelog.txt` — follow the format above
   - `profile.zip` — the exported profile archive

4. **Update `meta.json`** — append the new version entry to the `versions` array:
   ```json
   {
       "id": "<new-version>",
       "releasedAt": "<DD-MM-YYYY>"
   }
   ```
   The new entry goes at the **end** of the array.

5. **Commit and push:**
   ```
   git add "MC Server 29-05-2024 Mod/versions/<new-version>/" "MC Server 29-05-2024 Mod/meta.json"
   git commit -m "add <new-version>"
   git push -u origin main
   ```

---

## Branch Strategy

- **`main`** — the single production branch; all releases land here directly.
- No feature branches or pull-request workflow is used for normal version updates.
- AI/automation work is done on `claude/claude-md-docs-qLO3M` and merged to `main`.

---

## .gitignore Rules

```
.DS_Store     # macOS metadata — never commit
*.csv         # Data exports — never commit
```

The `versions/.DS_Store` file currently tracked in the repo is a legacy artifact; new `.DS_Store` files are ignored.

---

## AI Assistant Guidelines

- **Do not modify `profile.zip` files** — these are binary game archives; treat them as immutable artifacts.
- **Always update `meta.json`** when adding a version folder — the two must stay in sync.
- **Date format is `DD-MM-YYYY`** — not ISO 8601.
- **Append versions to the end** of the `meta.json` array — do not reorder existing entries.
- **Commit messages** are short and descriptive: `add 6.1.2`, `change neta .json`, `optimise mod 1`.
- **No build/test commands exist** — there is nothing to run or validate programmatically.
- The repository owner is **Chanon Boonkangwan** (`chanonnicky`).
