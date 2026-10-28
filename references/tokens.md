# Tokens

Every visual value in a page must come from this layer. If a value is missing,
add a token instead of a literal.

## 1. Color roles

| Role | Purpose |
|---|---|
| `accent` | Primary action, selection, links, active icon |
| `accentHover` / `accentPressed` | Pointer/press feedback for accent surfaces |
| `accentSoft` | Selected background, soft tint, 34 px icon badge |
| `onAccent` | Text/icon on accent |
| `bg` | Window canvas (L0), top bar and home footer share it |
| `surface1` | Panels, inputs, menus, dialogs, non-home footer |
| `surface2` | Hover, secondary surface, dropdown/stepper track |
| `surface3` | Disabled fill, deeper inset, meter track, skeleton |
| `textPrimary` | Titles and body |
| `textSecondary` | Supporting copy, idle nav, secondary icons |
| `textTertiary` | Captions, metadata, placeholders, disabled text |
| `danger` / `success` / `warning` | Destructive, positive, caution |
| `border` | Hairline rules, control outlines |
| `borderStrong` | Hover/focus edge, scrollbar thumb |
| `overlay` | Modal scrim (black with alpha) |

Rules:

- Never write a hex value in a page. Read `tokens.*` or `ColorScheme` roles.
- Status color is never the only signal: pair it with an icon or a word.
- Accent alpha stays low for fills (10–18%). Accent text uses the pure accent.

## 2. Palette

### Light

| Token | Value | Notes |
|---|---|---|
| `accent` | `#4F7CFF` | Archoera Blue (default brand) |
| `accentHover` | `#6B90FF` | |
| `accentPressed` | `#3D66E0` | |
| `accentSoft` | `#4F7CFF1F` | 12% accent |
| `onAccent` | `#FFFFFF` | |
| `bg` | `#F7F8FB` | window canvas |
| `surface1` | `#FFFFFF` | |
| `surface2` | `#F1F3F8` | |
| `surface3` | `#E8EBF3` | |
| `textPrimary` | `#111827` | |
| `textSecondary` | `#4B5563` | |
| `textTertiary` | `#8A94A6` | |
| `danger` | `#D64545` | |
| `success` | `#2F9E6B` | |
| `warning` | `#B7791F` | |
| `border` | `#E3E7EF` | |
| `borderStrong` | `#CFD6E4` | |
| `overlay` | `#00000066` | 40% |

### Dark

| Token | Value | Notes |
|---|---|---|
| `accent` | `#7A9BFF` | lightened for dark text contrast |
| `accentHover` | `#94AEFF` | |
| `accentPressed` | `#5F82E6` | |
| `accentSoft` | `#7A9BFF26` | 15% accent |
| `onAccent` | `#0B1020` | dark ink on light accent |
| `bg` | `#10141B` | |
| `surface1` | `#171D26` | |
| `surface2` | `#1E2632` | |
| `surface3` | `#27313F` | |
| `textPrimary` | `#F2F5FA` | |
| `textSecondary` | `#AAB5C6` | |
| `textTertiary` | `#738094` | |
| `danger` | `#FF7A7A` | |
| `success` | `#54D39A` | |
| `warning` | `#F0B45A` | |
| `border` | `#2A3442` | |
| `borderStrong` | `#3A4759` | |
| `overlay` | `#000000A6` | 65% |

Pairing matters: the light theme is a near-white canvas with true-white surfaces;
the dark theme is a blue-black canvas with slightly lifted surfaces. Do not use
pure black (`#000`) or pure white text (`#FFF`) on a large area.

## 3. Deriving a theme from one accent

A product supplies one seed color; everything else is derived. This keeps custom
or OS accent colors consistent.

```
onAccent   = luminance(seed) > 0.55 ? #0B1020 : #FFFFFF
accentHover   = mix(seed, white, 0.12)
accentPressed = mix(seed, black, 0.16)
accentSoft    = seed @ 12% (light) / 15–18% (dark; the palette uses 15%)
```

Optional "global tint" (pull neutrals slightly toward the accent, never text):

| Surface | Light blend | Dark blend |
|---|---|---|
| `bg` | 3.5% | 5% |
| `surface1` | 2.5% | 4% |
| `surface2` | 3.5% | 4.5% |
| `surface3` | 4% | 5% |
| `border` | 6% | 6% |
| `borderStrong` | 8% | 8% |

Tint is a preference, not the default. Text colors never get tinted.

Do not derive a new accent for status colors; danger/success/warning are fixed
roles with their own light/dark values.

## 4. Typography

Canonical scale (size / line-height):

| Token | Spec | Use |
|---|---|---|
| `display` | 32 / 40 | Home hero, empty-state title |
| `title1` | 24 / 32 | Page title |
| `title2` | 18 / 26 | Section title |
| `body` | 14 / 22 | Body, labels, buttons |
| `bodyStrong` | 14 / 22 · 600 | Emphasis, selected label |
| `caption` | 12 / 18 | Helper text, metadata |
| `overline` | 11 / 16 · 600 · 0.6 | Group labels, chips, metric labels |

Implementation aliases seen in production:

- `20 / 27` for hero block titles and empty-state titles.
- `16 / 22` for compact row titles; `15 / 22.5` for long-form body.
- `display` uses `-0.8` letter spacing; `title1` uses `-0.4`; overline uses
  `+0.4–1.2` for small caps-like group labels.

Weight:

- Default **500** for titles, body and buttons.
- **600** only for overline/group labels and a selected segment.
- Hierarchy comes from size, color and spacing first; bold is a last resort.
- No faux-bold, no letter-spacing on CJK body text.

Fonts:

- CJK + Latin share one family so baselines and fallbacks match. Recommended:
  **Noto Sans SC** (OFL). Bundle the Medium weight; do not rely on a system CJK
  fallback for one language and not the other.
- Numbers, paths, hashes and logs: mono (**JetBrains Mono**, OFL) with tabular
  figures when values are compared or updated live.
- Never ship an unlicensed commercial font.

## 5. Spacing

Scale: `4 / 8 / 12 / 16 / 24 / 32 / 48 / 64` (`xxs` … `xxxl`).

| Situation | Value |
|---|---|
| Icon ↔ label | 8 |
| Inside a section | 16–24 vertical, 12–24 horizontal |
| Row padding | 8–12 |
| Page gutter | 32 desktop, 16–24 narrow |
| Gap between sections/groups | 16–24 |
| Button horizontal padding | 18 (primary/secondary), 12 (text) |

Do not invent 10, 14, 18, 20 px gaps. Rhythm is more important than a perfect
optical nudge.

## 6. Radius, bevel and shape

| Token | Value | Use |
|---|---|---|
| `radiusS` | 2 | chips, pills, text buttons, small badges |
| `radiusM` | 4 | buttons, inputs, rows, dropdowns |
| `radiusL` | 4 | panels (when not beveled) |
| `radiusXL` | 6 | dialogs |
| `bevelS` | 8 | panel corner cut, nav plate, toast |
| `bevelM` | 14 | medium panels |
| `bevelL` | 22 | hero panels, dialogs, sheets |
| `radiusFull` | 999 | avatar, status dot, progress caps only |

Shape language:

- **Panels:** straight rectangle with two opposite corners cut — normally
  top-right `bevelL` + bottom-left `bevelS`.
- **Dialogs/sheets:** mirrored cut — top-left `bevelL` + bottom-right `bevelS`;
  bottom sheets cut both top corners.
- **Nav plate / chips / toasts:** top-right + bottom-left `bevelS`.
- **Buttons/inputs:** 2–4 px radius only. No pill buttons.
- Page sections are square with no radius at all.

A cut corner is a clipped straight edge, not a curve. Most frameworks can do it
with a beveled rectangle primitive or `clip-path: polygon(...)`.

## 7. Borders and elevation

- 1 px `border` is the default separator. 1 px `borderStrong` on hover/focus.
- A selected row/panel adds a 2 px accent edge (left side) and `accentSoft`.
- Shadows are for overlays only:
  - light `0 8 24 rgba(17,24,39,0.08)`
  - dark `0 8 24 rgba(0,0,0,0.35)`
- Menus, dialogs, toasts and the task drawer may use that shadow plus a 1 px
  border. Page sections and panels never do.
- Prefer a background step over a shadow. If you need a blur to see the edge,
  the hierarchy is wrong.

Layer stack:

```
L0  window canvas         bg
L1  page background       transparent (inherits bg)
L2  section / flat group  transparent + 1 px rules
L3  panel / row           surface1 or accentSoft / surface2 hover
L4  overlay               surface1 + border + shadow + scrim
```

## 8. Contrast and backgrounds

- Body text ≥ 4.5:1; large text (≥ 24 px) and icons ≥ 3:1.
- Light theme over an image: add a scrim or raise the content surface opacity
  until body text still passes. Dark theme: same rule, opposite direction.
- Never place secondary/tertiary text directly on a busy image.
- Focus indicators must be visible in both themes: 2 px accent ring or a 2 px
  accent edge, never a color-only change.

## 9. CSS naming used in `assets/tokens.css`

```
--accent --accent-hover --accent-pressed --accent-soft --on-accent
--bg --surface-1 --surface-2 --surface-3
--text-primary --text-secondary --text-tertiary
--danger --success --warning --border --border-strong --overlay
--radius-s --radius-m --radius-l --radius-xl
--bevel-s --bevel-m --bevel-l
--space-xxs … --space-xxxl
--font-sans --font-mono
--dur-fast --dur-normal --dur-slow --ease-out --ease-in
--titlebar-h --commandbar-h --homebar-h --content-gutter
```

Use the same names in other stacks (camelCase or platform constants) so specs
map one-to-one.
