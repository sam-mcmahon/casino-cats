# Casino Cats — Design Reskin Handoff

**Version this doc reflects**: v1.9.1  
**Live URL**: https://sam-mcmahon.github.io/casino-cats  
**Entire app lives in one file**: `index.html` (3,135 lines)

---

## What This App Is

Lucky Paws Casino is a browser-based slot machine game with an Asian-fusion casino theme — cats, dragons, fish, red envelopes, fortune wheels. Everything (HTML, CSS, JavaScript) is in a single file with no build step and no external dependencies beyond a Google Fonts CDN import.

The reskin goal is a fresh visual identity: new palette, typography, and polish. JavaScript game logic must not change.

---

## File Map

```
index.html
├── Lines   1–10    <head> — meta, font import, title
├── Lines  11–1139  <style> — all CSS (1,129 lines)
│   ├── Lines 14–24     :root CSS variables
│   ├── Lines 26–46     Body / global reset
│   ├── Lines 54–81     Header
│   ├── Lines 83–104    Coins / last-win display
│   ├── Lines 106–140   Jackpot bar
│   ├── Lines 142–223   Dragon Vault progress bar + tile grid
│   ├── Lines 226–362   Machine frame + reel grid + cells
│   ├── Lines 364–399   Win display
│   ├── Lines 420–529   Controls (bet buttons, spin button, icon buttons)
│   ├── Lines 531–618   Info cards + achievements panel
│   ├── Lines 620–727   Mini-game overlay + buttons
│   ├── Lines 729–862   Individual game UIs (wheel, fish pond, paw race)
│   └── Lines 864–1138  Toast, particles, history, footer, mobile breakpoints
├── Lines 1141–1307  HTML body (structure only — no inline styles)
└── Lines 1307–3135  <script> — all game logic
```

---

## Current Design Identity

**Theme**: High-contrast dark casino with gold highlights and red accents. Traditional Chinese/Asian gaming motifs — dragons, luck symbols, jade, red envelopes. Cinematic glow effects and shimmer animations throughout.

**Background**: Near-black dark burgundy (`#0d0202`) with a repeating `+` texture at 8% opacity. Radial gradients add a red glow in the upper-left and warm orange-brown in the lower-right.

**Machine frame**: 3D beveled border (gold outer edge, dark red inner). Inset shadow creates depth.

**Controls**: Large circular SPIN button (red radial gradient, gold border). Surrounding bet buttons with dark backgrounds and gold text. Right-side icon buttons (auto-spin, max bet, sound toggle, reload).

**Information**: Card-based panels with dark semi-transparent backgrounds and red borders.

---

## CSS Variables (`:root`, lines 14–24)

These 9 variables are the fastest lever for repainting most of the UI.

| Variable | Current Value | Used For |
|----------|--------------|----------|
| `--gold` | `#FFD700` | Primary accent, borders, highlights |
| `--gold2` | `#FFA500` | Secondary accent (orange-gold) |
| `--red` | `#CC0000` | Dark red elements |
| `--red2` | `#FF3333` | Bright red elements |
| `--jade` | `#00A86B` | Green (declared, underused in base UI) |
| `--dark` | `#1a0505` | Dark brown-red surfaces |
| `--darker` | `#0d0202` | Near-black page background |
| `--panel` | `rgba(30,5,5,0.92)` | Translucent panel backgrounds |
| `--text` | `#FFE0A0` | Primary text color (warm beige) |

### Hardcoded Colors Outside Variables

These are scattered through the CSS and will need manual find/replace if changing the palette:

| Hue Family | Values |
|-----------|--------|
| Golds / oranges | `#FF8C00`, `#FFF176`, `#FFD060`, `#FFB84D`, `#FF9944` |
| Dark reds / maroons | `#2a0000`, `#200000`, `#440000`, `#330000`, `#660000`, `#880000`, `#AA0000`, `#CC2200`, `#FF4400`, `#FF4444` |
| Free spins green | `#00FF88`, `#00CC66`, `#00AA55`, `#007733`, `#004422`, `#2ECC71` |
| Dragon Vault purple | `#8E44AD`, `#C44CFF`, `#9B59B6`, `#1a0030`, `#2a0050`, `#5a2a7a`, `#D7A6F0` |
| Ocean Frenzy blue | `#001a3a`, `#003366`, `#0055AA`, `#3498DB`, `#00BFFF`, `#7FFFD4`, `#2C3E50` |
| Accent pinks / yellows | `#FF69B4`, `#F1C40F` |

### Particle & Win-Line Colors (in JavaScript)

These are hardcoded arrays in the `<script>` block — search for these identifiers if you want to update them:

- `JACKPOT_COLORS` — colors for jackpot particle burst
- `WIN_COLORS` — colors for regular win particle burst
- `FREE_COLORS` — colors for free spins particle burst
- `BONUS_COLORS` — colors for bonus game particle burst
- Win-line colors are in `drawWinLines()` — an array of color strings, one per payline

---

## Typography

- **Font**: Noto Sans SC — loaded from Google Fonts (weights 400, 700, 900)
- **`@import` location**: Line 12 of the `<style>` block
- **Sizes**: Fluid with `clamp()` throughout (e.g., `clamp(1rem, 4vw, 1.2rem)`)
- **Primary text color**: `--text` = `#FFE0A0`
- **Accent text color**: `#FF9944` (orange)

To swap fonts: update the Google Fonts URL on line 12 and find/replace `Noto Sans SC` in the CSS. Keep a CJK fallback if preserving the Chinese subtitle text (`幸运爪子`).

---

## UI Component Inventory

### Always-Visible Components

| Component | Selector | Description |
|-----------|----------|-------------|
| Header | `#header`, `#logo`, `#subtitle` | Logo `🐱 LUCKY PAWS CASINO 🎰` + subtitle with Chinese text |
| Coins display | `#coins-display`, `#last-win-display` | Player balance + persistent last-win badge |
| Jackpot bar | `#jackpot-bar`, `#jackpot-amount` | Progressive jackpot with animated pulse border |
| Dragon Vault progress | `#vault-bar`, `#vault-fill`, `#vault-keys-text` | Key collection progress (0–5); glows gold when ready |
| Machine frame | `#machine`, `#machine-frame` | Outer slot machine container with 3D border effect |
| Reel grid | `#reels-container`, `.reel`, `.reel-cell` | 5 columns × 4 rows of emoji symbol cells |
| Win display | `#win-display`, `#win-message`, `#win-amount` | Animated payout message + amount |
| Controls | `#controls`, `.bet-btn`, `#spin-btn`, `.icon-btn` | Bet buttons, spin button, auto-spin, sound, reload |
| Info cards | `#info-panel`, `.info-card`, `.paytable-row` | Paytable and special features reference |
| Achievements | `#achievements-panel`, `.achievement` | 16 unlockable achievement badges |
| Spin history | `#history-panel`, `.history-item` | Last ~15 spins log |
| Free spins bar | `#free-spins-bar`, `#free-spins-count` | Green banner, visible only during free spins |
| Multiplier display | `#multiplier-display` | "HOT STREAK ×N" — visible during win streaks |
| Version footer | `#version-footer`, `#changelog` | Collapsible version/changelog at bottom |

### Spin Button States

| State | Color | Trigger |
|-------|-------|---------|
| Normal | Red radial gradient (`#FF6600 → #CC2200 → #990000`) | Default |
| Free spins | Green radial gradient (`#00FF88 → #00AA55 → #007733`) | During free spins |
| Dragon Stampede | Purple radial gradient | During stampede bonus |

Size: 120px circle, `clamp`-sized label, gold 4px border, glow `box-shadow`.

### Overlay / Modal Components

| Component | Selector | Trigger | Description |
|-----------|----------|---------|-------------|
| Mini-game overlay | `#minigame-overlay`, `#minigame-box` | Various | Wraps all bonus games |
| Cat Shrine | — (inside minigame-box) | 3+ cats scattered | 5 cat buttons, pick one for multiplier |
| Dragon Egg | — | 3+ 😸 Happy Neko on a line | 3 glowing egg choices, hatching animation |
| Red Envelope Rain | — | 4+ 🧧 on a line | Tap falling envelopes, 6-second timer |
| Ocean Frenzy | `#fish-pond`, `.pond-fish` | 3+ 🐠 on a line | Tap fish before they escape, 8-second timer |
| Fortune Wheel | `#fortune-wheel`, `#wheel-pointer` | 3+ 🔮 on a line | 12-segment SVG wheel, 4-second spin |
| Paw Race | `#crab-race-wrap`, `.crab-lane`, `.crab-racer` | 3+ 🐾 on a line | 3-lane cat race, 1-in-3 win, ×10 payout |
| Dragon Vault | `.vault-tile` (in `#vault-bar`) | 5 keys collected | 4×3 tile-pick, purple theme |
| Big Win overlay | `#bigwin-overlay`, `#bigwin-text` | Large win amount | Full-screen flash for big/mega/jackpot wins |
| Notification toast | `#notification` | Various events | Slides in top-right (mobile: bottom-center), 3.5s |
| Particles canvas | `#particles-canvas` | Wins / jackpots | Full-viewport Canvas confetti burst |
| Win-line overlay | `#win-line-overlay` | After each spin | Canvas lines drawn over reels showing paylines |

---

## Game Symbols

These are slot machine assets tied to game logic. **Do not change the emoji.** The symbol list changed significantly in v1.8 — it now has 14 symbols with a stronger cat/Asian theme:

| Emoji | ID | Name | Role | Rarity |
|-------|----|----|------|--------|
| 🐱 | cat | Lucky Neko | Scatter (triggers Cat Shrine) | Common |
| 🌙 | moon | Lucky Moon | Triggers Dragon Stampede (3+ anywhere) | Uncommon |
| 💎 | diamond | Fortune Diamond | Jackpot trigger (5 on center line) | Uncommon |
| 🐙 | octopus | Octopus Wild | Wild — stacks to fill entire reel | Uncommon |
| 🐉 | dragon | Lucky Dragon | Premium symbol | Uncommon |
| 🔮 | fortune | Fortune Orb | Triggers Fortune Wheel | Medium |
| 🧧 | hongbao | Red Envelope | Triggers Envelope Rain | Medium |
| 😸 | neko | Happy Neko | Triggers Dragon Egg bonus | Medium |
| 🐾 | paw | Lucky Paw | Triggers Paw Race | Medium |
| 🏮 | lantern | Red Lantern | Filler | Common |
| 🐠 | clownfish | Clownfish | Triggers Ocean Frenzy | Common |
| 🌸 | blossom | Cherry Blossom | Scatter 2 (4+ = free spins) | Rare |
| 🔔 | bell | Lucky Bell | Low-value filler | Common |
| 🪙 | coin | Fortune Coin | Lowest-value filler | Very common |

---

## Animations Reference

| Keyframe | Duration | Animates | On |
|----------|----------|----------|----|
| `logoGlow` | 3s infinite alternate | `drop-shadow` | Header logo |
| `shimmer` | 2s linear infinite | `background-position` (gradient shift) | Jackpot/win amounts |
| `jackpotPulse` | 4s infinite | `box-shadow` | Jackpot bar border |
| `vaultReady` | 0.7s infinite alternate | `box-shadow` + `border-color` | Vault bar when 5 keys ready |
| `tileFlip` | 0.4s ease | `rotateY` + `scale` | Dragon Vault tile reveal |
| `cellWin` | 0.4s infinite alternate | `scale` + `brightness` | Winning reel cells |
| `wildStack` | 0.5s infinite alternate | inset `box-shadow` | Wild-stacked reel columns |
| `freeSpinBtn` | 0.7s infinite alternate | `transform` + `box-shadow` | Spin button during free spins |
| `mgBtnPulse` | 2s infinite | `transform` + `border-color` + `box-shadow` | Mini-game pick buttons |
| `fishSwim` | 3s linear | `translateX` | Fish moving through pond |
| `fishCaught` | 0.5s ease | `scale` + `translateY` + `opacity` | Fish on catch |
| `achieveUnlock` | 0.5s ease | `scale` + `opacity` | Achievement badge pop-in |
| `freeSpinBar` | 0.8s infinite alternate | `box-shadow` | Free spins bar glow |
| `bigWinPop` | 0.5s cubic-bezier | `scale` + `rotate` + `opacity` | Big win text entrance |
| `boxIn` | 0.4s cubic-bezier | `scale` + `rotate` + `opacity` | Mini-game modal entrance |
| `envFloat` | 2.5–4.5s (dynamic) | `translateY` + `rotate` | Falling red envelopes |

---

## Responsive Breakpoints

- **`max-width: 480px`** — mobile layout: buttons expand, info panels condense
- **`max-width: 480px` + `max-height: 700px`** — compact layout for iPhone SE–sized screens (smaller reel cells, subtitle hidden)

Other mobile features:
- iOS safe area insets (`env(safe-area-inset-*)`)
- `user-scalable=no` prevents accidental zoom in Safari
- Swipe down ≥ 60px triggers spin
- Vibration API for haptic feedback
- Notification toast repositions to bottom-center on mobile

---

## Design Constraints

1. **Single file** — all CSS stays in the `<style>` block inside `index.html`. No separate stylesheet.
2. **No image files** — all visuals are emoji, CSS, SVG, or Canvas. Keep it that way, or add at most one background texture image.
3. **Emoji are game assets** — the 14 slot symbols (listed above) are referenced by ID in JavaScript. Don't change them.
4. **Canvas is code-drawn** — win-line overlay and particles are drawn by the JavaScript engine. To change those colors, update the color arrays in the `<script>` block (search `JACKPOT_COLORS`, `WIN_COLORS`, `FREE_COLORS`, `BONUS_COLORS`, `drawWinLines`).
5. **Don't touch element IDs or class names** — JavaScript selects elements by these to control game state.
6. **Don't touch the `display: none` / `.active` toggle pattern** — JavaScript uses this to show/hide overlays.
7. **Don't touch `localStorage` key `luckypaws_v2`** — changing this wipes player save data.

---

## Where to Focus for Maximum Impact

| Priority | Target | Why |
|----------|--------|-----|
| 1 | `:root` variables (lines 14–24) | 9 values repaint most of the UI instantly |
| 2 | `body` + `#machine` backgrounds | Sets the overall mood |
| 3 | `#spin-btn` | The most visually prominent interactive element |
| 4 | `#header h1` (`#logo`) | Brand mark — gradient, animation, glow |
| 5 | `#jackpot-bar` | Animated border, gradient background |
| 6 | `--panel` + panel `background` declarations | All card backgrounds |
| 7 | Mini-game overlays | Dragon Vault purple is the most distinctive sub-theme |
| 8 | Font swap | Single `@import` on line 12 + find/replace `Noto Sans SC` |

---

## Suggested Reskin Directions

Pick one or invent something better — these are starting points only.

### A — Neon Night Market
Replace dark reds with deep navy/midnight blue (`#050518`, `#0a0a2e`). Swap gold/red for electric cyan (`#00F5FF`) and hot magenta (`#FF007F`). Geometric glow effects instead of warm shimmer. Font: Rajdhani, Exo 2, or Orbitron.

### B — Cozy Lantern Festival
Warm cream and terracotta palette. Soft, rounded UI with subtle paper-texture background. Less glow, more warmth. Font: Quicksand or Nunito.

### C — Luxury High-Roller
Deep black with platinum/silver and emerald green accents. Geometric/grid-based layout. More space, less clutter. Serif or display font (Playfair Display, Cormorant Garamond). Subtle noise texture instead of solid fills.

### D — Retro Arcade
Pixel-art inspired borders and scanline overlay effect. CRT-glow green or amber monochrome accents. Dotted/dithered backgrounds. Font: Press Start 2P or VT323.

---

## What Not to Touch

- All `<script>` content — except the particle/win-line color arrays if changing those colors
- Element IDs and class names (JS depends on these)
- The `.active` toggle pattern on overlays
- `localStorage` key `luckypaws_v2` and the data schema
- Viewport/meta tags in `<head>`
- The 14 game symbol emoji
