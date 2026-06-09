# Lucky Paws Casino — Claude Instructions

## MANDATORY: Version & Changelog Update on Every PR

**Before creating any PR, you MUST:**

1. Increment the version number in `index.html`
2. Add a new entry to the changelog in `index.html`

### Version numbering
- Minor fixes/tweaks → bump patch: `v1.5` → `v1.5.1`
- New features or meaningful gameplay changes → bump minor: `v1.5` → `v1.6`
- Major overhauls → bump major: `v1.x` → `v2.0`

### Where to update (two places, keep in sync)

**1. The `<summary>` tag** (shows version at a glance):
```html
<summary>v1.5 · Lucky Paws Casino</summary>
```

**2. The `#changelog` div** (prepend a new line at the top):
```html
<b>v1.6</b> — Short description of what changed in this PR<br>
<b>v1.5</b> — Previous entry...
```

Both are inside `<!-- VERSION FOOTER -->` near the bottom of `index.html`.

## Current version: v1.5

## Development branch
Always develop on `claude/sleepy-wozniak-3f4rxt` and PR into `main`.

## Architecture
- Single file: `index.html` — all CSS, JS, and HTML inline, no build step
- No external dependencies except Google Fonts
- Deployed via GitHub Pages from `main` branch
- Live at: https://sam-mcmahon.github.io/casino-cats
- Save key: `luckypaws_v2` in localStorage

## Key constants
- `NUM_ROWS = 4` (5 reels × 4 rows)
- `KEYS_NEEDED = 5` (Dragon Vault)
- `REEL_WEIGHTS` — parallel array to `SYMBOLS`, higher = more frequent
- `PAYLINES` — 12 lines on the 5×4 grid

## Game features
- 6 mini-games: Cat Shrine, Dragon Egg, Red Envelope Rain, Ocean Frenzy, Fortune Wheel, Crab Race
- Dragon Vault grand feature (collect 5 keys → pick-game)
- Free spins (manual press, green SPIN button)
- Hot streak multiplier
- Single player, localStorage persistence
