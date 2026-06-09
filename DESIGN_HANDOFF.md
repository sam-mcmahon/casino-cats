# Casino Cats — Design Reskin Handoff

## Project Overview

**Lucky Paws Casino** is a single-file HTML/CSS/JavaScript slot machine game with an Asian-fusion casino theme (cats, dragons, fish, red envelopes). Everything — HTML markup, CSS, and JavaScript — lives in one file: `index.html` (2,757 lines). There is no build system, no framework, and no external dependencies beyond a Google Fonts CDN import.

The game is fully playable in a browser, mobile-responsive, and PWA-capable.

---

## Goal

Reskin the visual design while preserving all game logic and functionality. The target is a fresh coat of paint: new color palette, typography treatment, layout polish, and visual identity. Do not alter JavaScript game logic.

---

## File Structure

```
casino-cats/
└── index.html          # Entire app — HTML + <style> + <script>
    ├── Lines 1–10      # <head> (meta, fonts, title)
    ├── Lines 11–1015   # <style> block (all CSS)
    └── Lines 1016–2757 # HTML body + <script> block
```

There are no images, icon fonts, or asset files. All visuals use:
- **Emoji** (game symbols and UI icons)
- **SVG** (fortune wheel, procedurally generated)
- **HTML5 Canvas** (particles, win-line overlays)
- **Pure CSS** (gradients, shadows, keyframe animations)

---

## Current Design Identity

### Theme
Chinese/Asian casino aesthetic. Dark, moody, red-and-gold with neon accents. Heavy use of glows and shimmer animations.

### Color Palette (CSS Variables, lines 14–24)

| Variable | Value | Usage |
|----------|-------|-------|
| `--gold` | `#FFD700` | Primary accent, highlights |
| `--gold2` | `#FFA500` | Secondary accent (orange-gold) |
| `--red` | `#CC0000` | Dark red |
| `--red2` | `#FF3333` | Bright red |
| `--jade` | `#00A86B` | Green (declared but underused) |
| `--dark` | `#1a0505` | Dark brown-red |
| `--darker` | `#0d0202` | Near-black background |
| `--panel` | `rgba(30,5,5,0.92)` | Translucent panel backgrounds |
| `--text` | `#FFE0A0` | Primary text (warm beige) |

Additional hardcoded colors throughout (not variables):
- Free spins: `#00FF88`, `#00CC66`, `#007733`
- Dragon Vault: `#8E44AD`, `#C44CFF`, `#D7A6F0`
- Fish pond: `#001a3a`, `#003366`, `#0055AA`

### Typography
- **Font**: Noto Sans SC (loaded from Google Fonts CDN, weights 400/700/900)
- **Sizes**: Responsive with `clamp()` (e.g., `clamp(1rem, 4vw, 1.2rem)`)
- **Text colors**: `#FFE0A0` (primary), `#FF9944` (orange accents)

### Logo / Identity
```
🐱 LUCKY PAWS CASINO 🎰
✨ 幸运爪子 · FORTUNE · FISH · FELINES ✨
```
- Logo uses a gold shimmer gradient animation (`logoGlow` keyframe, `shimmer` keyframe)
- Subtitle is smaller with reduced opacity

---

## UI Component Inventory

### Always-Visible Elements

| Component | Selector | Description |
|-----------|----------|-------------|
| Header | `#header` | Logo + subtitle |
| Jackpot bar | `#jackpot-bar` | Progressive jackpot amount, animated pulse border |
| Dragon Vault progress | `#vault-bar` | Key collection progress (0–5 keys), glows when ready |
| Slot machine frame | `#machine` | Outer container, dark red gradient background |
| Reel grid | `#reels` | 5×4 grid of symbol cells |
| Free spins bar | `#free-spins-bar` | Green bar, visible only during free spins |
| Win display | `#win-display` | Shows win message + amount |
| Coins display | `#coins-display` | Player balance |
| Controls panel | `#controls` | Bet buttons, spin button, auto-spin, toggles |
| Paytable | Lower section | Symbol payout reference |
| Special features | Lower section | Feature descriptions |
| Achievements grid | Lower section | 17 unlockable achievements |
| Spin history | Lower section | Last 15 spin results |

### Overlay / Modal Elements

| Component | Trigger | Description |
|-----------|---------|-------------|
| Dragon Vault | 5 keys collected | 4×3 tile-pick grid, purple theme |
| Cat Shrine | 3+ cats scattered | 5 cat buttons, pick-one for multiplier |
| Ocean Frenzy | 5+ clownfish | Tap fish in a pond, 8-second timer |
| Fortune Wheel | 3+ orbs | 12-segment spinning SVG wheel |
| Dragon Egg | 3+ dragons | 3 colored egg choices, hatching animation |
| Red Envelope Rain | 4+ envelopes | Tap falling envelopes, 6-second timer |
| Crab Race | 5+ crabs | 3-lane race, 1-in-3 win chance |
| Big Win overlay | `#bigwin-overlay` | Full-screen flash, triggered on large wins |
| Notification toast | `#notification` | Slide-in toast, 3.5s timeout |
| Particles canvas | `#particles-canvas` | Full-viewport floating particles on wins |
| Win-line overlay | `#win-line-overlay` | Canvas drawn over reels showing paylines |

### Spin Button States
- **Normal**: Red radial gradient (`#FF6600 → #CC2200 → #990000`)
- **Free spins active**: Green radial gradient (`#00FF88 → #00AA55 → #007733`)
- Size: 120px circle, large font, glow box-shadow

---

## Animations Reference

| Name | Duration | Used On |
|------|----------|---------|
| `logoGlow` | 3s infinite | Header logo |
| `shimmer` | 2s infinite | Logo text gradient |
| `jackpotPulse` | 4s infinite | Jackpot bar border |
| `tileFlip` | 0.4s | Dragon vault tile reveal |
| `vaultReady` | 0.7s infinite | Vault bar when 5 keys collected |
| `freeSpinBtn` | 0.7s infinite | Spin button during free spins |
| `cellWin` | 0.4s infinite | Winning reel cells |
| `bigWinPop` | 0.5s | Big win overlay appear |
| `fishSwim` | 3s linear | Fish movement in pond |
| `fishCaught` | 0.5s | Fish catch animation |
| `mgBtnPulse` | 2s infinite | Mini-game buttons |
| `achieveUnlock` | 0.5s | Achievement unlock pop |
| `freeSpinBar` | 0.8s infinite | Free spins bar glow |
| `envFloat` | 2.5–4.5s | Falling envelopes |

---

## Responsive Breakpoints

- Primary breakpoint: `max-width: 480px` (mobile styles)
- Uses `clamp()` for fluid sizing throughout
- iOS safe area support: `env(safe-area-inset-*)` in padding
- Touch gesture: swipe down 60px to spin

---

## Design Constraints

1. **Single file** — all CSS stays inside the `<style>` block in `index.html`. No external stylesheets.
2. **No image files** — all visuals are emoji, CSS, SVG, or Canvas. New design should follow the same constraint unless adding a single background image.
3. **Emoji are game assets** — do not replace game symbols (🐱💎🐙🐉🔮🧧🐠🦀🐡🌸🪙). UI emoji can be changed.
4. **Canvas elements are drawn by JS** — the win-line overlay and particles canvas are painted programmatically. Colors for these are hardcoded in the JS (`drawWinLines`, `spawnParticles` functions). If you want to change win-line or particle colors, those constants need updating in the script too.
5. **Particle colors** are defined in JS arrays near the top of the script block — search for `JACKPOT_COLORS`, `WIN_COLORS`, `FREE_COLORS`, `BONUS_COLORS`.
6. **Font swap** — if replacing Noto Sans SC, update the Google Fonts `<link>` tag in `<head>` and the `font-family` declarations in CSS. Keep a CJK fallback if keeping the Chinese subtitle text.

---

## Game Symbol Reference

These emoji are game symbols and carry specific game logic. **Do not change them.**

| Emoji | Name | Role |
|-------|------|------|
| 🐱 | Lucky Neko | Scatter (triggers Cat Shrine) |
| 💎 | Fortune Diamond | Premium symbol (jackpot at 5×) |
| 🐙 | Octopus | Wild (substitutes all) |
| 🐉 | Lucky Dragon | Triggers Dragon Egg bonus |
| 🔮 | Fortune Orb | Triggers Fortune Wheel |
| 🧧 | Red Envelope | Triggers Envelope Rain |
| 🐠 | Clownfish | Triggers Ocean Frenzy |
| 🦀 | Lucky Crab | Triggers Crab Race |
| 🐡 | Pufferfish | Low-value filler |
| 🌸 | Cherry Blossom | Scatter 2 (triggers Free Spins) |
| 🪙 | Fortune Coin | Lowest value filler |

---

## Suggested Reskin Directions (Designer's Choice)

The following are starting-point ideas — not requirements. Pick one or invent something better.

### Option A: Neon Night Market
- Replace dark red tones with deep navy/midnight blue (`#060618`, `#0a0a2e`)
- Neon cyan and magenta accents instead of gold/red
- Electric glow effects instead of warm shimmer
- Font: something more geometric (e.g., Rajdhani, Exo 2, or Orbitron)

### Option B: Cozy Cottage Casino
- Warm cream and forest green palette
- Soft, rounded UI elements with less glow
- Whimsical rather than dramatic
- Font: Quicksand or Nunito

### Option C: Luxury High-Roller
- Deep black with platinum/silver and emerald accents
- More geometric/grid-based layout
- Serif or display font (Playfair Display, Cormorant)
- Subtle texture patterns instead of solid fills

### Option D: Retro Arcade
- Pixel-art inspired borders and scanlines
- CRT-glow green or amber monochrome accents
- Font: Press Start 2P or VT323
- Dotted/dithered backgrounds

---

## Where to Focus Changes

1. **`:root` variables** (lines 14–24) — change 8 variables to immediately repaint most of the UI
2. **Body/background** — `body` and `#machine` backgrounds set the overall mood
3. **Spin button** — most visually prominent interactive element, high impact
4. **Header logo** — `#header h1` controls the brand mark, including gradient and animation
5. **Panel backgrounds** — `--panel` variable + individual panel `background` declarations
6. **Jackpot bar** — `#jackpot-bar` has its own gradient and border animation
7. **Overlay modals** — each mini-game has its own color treatment; the Dragon Vault purple is the most distinctive

---

## What to Leave Alone

- All `<script>` content except particle/win-line color arrays (if changing those colors)
- Element IDs and class names (JS selects by these)
- The `display: none / .active` pattern on overlays (JS toggles these)
- `localStorage` key `luckypaws_v2` and data shape
- Viewport/meta tags and PWA manifest attributes
- The Google Fonts `<link>` tag (can swap font URL but keep the tag format)
