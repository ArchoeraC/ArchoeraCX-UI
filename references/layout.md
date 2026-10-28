# Layout

The shell is desktop-first and full-bleed. It should look intentional from
1280 px to 4K, and collapse gracefully below 900 px.

## 1. Window anatomy

```
┌──────────────────────────────────────────────────────────────────────┐
│ TopBar · brand                                    global task entry   │ 48
├──────────────────────────────────────────────────────────────────────┤
│ Page content                                                          │
│   page header (title + actions)                                       │
│   flat sections / rows / panels / grid                                │
│   max width = window, gutter 32                                       │
│                                                                       │
├──────────────────────────────────────────────────────────────────────┤
│ CommandBar · navigation (left)     account │ primary action (right)   │ 60
└──────────────────────────────────────────────────────────────────────┘
```

- **TopBar 48 px.** Same background as the page, **no bottom rule** — brand and
  global controls appear to float on the canvas. Left: 30 px mark + 8 px gap +
  product name (`titleSmall`, 600, accent color). Right: one global entry
  (task/progress). No placeholder bell, avatar or debug icons.
- **Content.** One scrollable column, centered, full width with 32 px side
  gutters (16–24 narrow), 24 px above, 8 px below. Internal limits belong to the
  page (e.g. settings max 1500), not to the shell.
- **CommandBar 60 px** (`46` on the home canvas). Background `bg` at home,
  `surface1 @ 86%` + 1 px top rule elsewhere. Left: nav; right: account and the
  primary action, separated by a 1 × 30 px rule. Never a floating dock.
- **Page title lives in content**, not in the TopBar. The TopBar is chrome only.

On wide windows use the whole width. A centered 900 px strip on a 2560 px screen
is a bug, not restraint. Cap only absurd widths (2880 px).

## 2. Command bar

| Part | Spec |
|---|---|
| Height | 60 normal / 46 home |
| Horizontal padding | 32 (matches content gutter) |
| Nav item | height 40; padding 12–16; icon 18 + 8 gap + label |
| Nav gap | 4 |
| Selected plate | `accentSoft` fill, 1 px accent border @ 32%, bevelS corners |
| Plate motion | slides/resizes 180 ms easeOutCubic; snaps on relayout |
| Home item | navigates but never highlights, no hover block |
| Account | 30 px avatar + name (≤150 px) + type; 8 gap |
| Primary action | filled, 38 high; icon-only ≤ 680 |
| Divider | 1 × 30, `border`, 16 px each side |

Responsive:

- `< 820` nav drops labels (icons only, tooltips).
- `< 900` account drops name, keeps avatar.
- `< 680` primary action becomes a 38 px icon button.
- On the home page hide the primary action and animate it in/out with
  `SizeTransition` + fade over 180 ms; never pop it.

Navigation items are measured after layout so the plate matches the real hit
target (labels change with language and density). One plate is shared by all
items; do not draw an underline or pill per item.

## 3. Content and page header

`AppPageHeader`: title `title1` (24), optional subtitle 14 `textSecondary` with
8 px gap, actions right-aligned and wrap.

- `< 600` with actions: stack title above actions, 16 px gap.
- Leading back button: 12 px before the title block, top-aligned.
- Page header is part of the scrolling content (it is not sticky). The TopBar is
  the fixed upper boundary.
- Vertical rhythm: header → 16 → first block; blocks separated by 16–24.

## 4. Breakpoints

Use content width, not the window width, for in-page decisions.

| Width | Behavior |
|---|---|
| 380 | metric tiles 1 column |
| 420 | compact search controls wrap |
| 460 | version rows drop secondary columns |
| 560 | account rows show/hide "active" label |
| 600 | page header actions stack under title |
| 620 | recent grid 1→2 columns |
| 680 | command bar primary action becomes icon |
| 720 | hero art appears; shortcut strip becomes row; list row shows date; metrics 4 columns; instance row wide mode |
| 760 | hero art column; empty account panel splits |
| 820 | command bar nav icons only; settings switches from sidebar to tabs |
| 860 | download queue shows bytes/speed |
| 900 | account button drops name |
| 1020 | instance list becomes 2-column card grid |
| 1040 | recent grid 3 columns; Java grid 2 columns; detail page 2 columns |
| 1200 | feed 2 columns |
| 1400 | accounts page splits current account / list |
| 1500 | instance grid 3 columns; recent grid 4 columns; settings content caps |
| 1850 | instance grid 4 columns |
| 2100 | feed 3 columns |
| 2200 | instance grid 5 columns |
| 2880 | shell max width cap |

Breakpoints are thresholds, not device classes. Keep them in one place and test
both sides of each one.

## 5. Grids

- Gap: 12 for card grids, 24 for metric groups.
- Column count depends on available width; **if there are fewer items than
  columns, use the item count** so the last row fills the width instead of
  leaving a ragged right edge.
- Card grids switch from list rows only when there is room: rows carry more
  metadata and are the default.

Recipes:

```
instances   2 / 3 / 4 / 5 cols at 1020 / 1500 / 1850 / 2200
recent      1 / 2 / 3 / 4     at 620 / 1040 / 1500
metrics     1 / 2 / 4         at 380 / 720
feed        1 / 2 / 3         at 1200 / 2100
java        1 / 2             at 1040
```

## 6. Page layouts

### Dashboard (landing / home)

```
[ eyebrow + product/workspace name          ]  ← hero panel (beveled, accent edge)
[ 44 px title                              ]     right art column ≥760
[ meta · meta · meta                       ]
[ primary 44 ] [ secondary 44 ]

[ shortcut | shortcut | shortcut ]            ← one panel, 1 px dividers

Recent                                     View all
[ tile ] [ tile ] [ tile ] [ tile ]            ← max 4, fills row
```

- Hero is the only large filled block; give it the accent border when active.
- Shortcut strip: 1 row ≥ 720, stacked below; each cell 12/16 padding, icon 19
  accent, label 12 tertiary, value 14 primary, chevron right.
- No debug cards (runtime, core, Java) on the landing page.
- Empty: hero turns into "create / import" with one primary action.

### Collection (list of items)

```
Page header: title + subtitle                 [refresh] [primary]
[ search ................ ] [filter] [sort] [view]
[ count / active filter summary ]
──────────────────────────────────────────────
[ icon | title + meta | date | action | ⋯ ]    row 56–72
...
```

- Default is a list; switch to cards only ≥1020.
- Selected row: `accentSoft`, 2 px accent left edge, accent check at the end.
- Hover: `surface2`. No movement.
- Row actions: one text action + overflow menu; secondary click opens the same
  menu at the pointer.
- Wide cards keep the same actions (primary + overflow) as rows.

### Detail (one object, e.g. project/version)

```
[←] Title                                      [primary]
subtitle / meta chips
──────────────────────────────────────────────
[ overview / media / description ]   [ versions ]   ≥1040 two columns
                                     [ ...      ]   <1040 single column
```

- Back returns to the **same** search results, scroll position and query.
- Two columns at ≥1040; single column otherwise, preserving reading order.
- Groups are flat sections + rules, not nested cards.
- The primary action sits at the bottom of the selection column, not in a modal.

### Settings

```
Page header
[ section nav 208 ] │ [ content ≤1500, single column ]
  icon + label      │   GROUP LABEL
  selected: 2 px    │   ────────────────────────────
  accent bar,       │   34px icon | title/subtitle | control
  accentSoft        │   ────────────────────────────
                    │   note
```

- ≥820: fixed 208 px left nav, only the right column scrolls; nav items 40–44
  high, 2 px accent left edge + `accentSoft` when selected.
- <820: horizontal tab row (42 px, scrollable, auto-scroll the active tab into
  view), then content.
- Content max 1500; groups single column even on ultrawide (readability).
- No nested page tabs inside a section; flatten the hierarchy.

### Feed / metrics

- Metrics: label 11 tertiary → value 22 tabular → meta 12 → 3 px meter. Meter
  colors: <75% accent, ≥75% warning, ≥92% danger. Absolute counts get no meter.
- Feed: single column below 1200; 2–3 balanced columns above. Each row: meta line
  (source · type · relative time), title ≤2 lines, summary ≤2 lines, tags. Whole
  row clickable; 15 px external-link icon top-right.
- Empty/error use the standard empty state, not a toast.
- Relative time: "just now" <1 min, minutes <1 h, hours <24 h, days <7 d, then
  `YYYY-MM-DD`.

### Accounts / profile

- Current account hero + full-width list. ≥1400 puts them side by side.
- Never show tokens; show type, status and expiry only.
- Empty state is the add-account panel itself, not an empty list plus a button.

## 7. Scrolling

- One scroll owner per region. Do not nest vertical scrolls.
- Settings: nav fixed, content scrolls. Download: page content scrolls and the
  queue strip stays pinned to the bottom of the page region.
- Task center is an overlay drawer, not a route; Esc/scrim closes it.
- Preserve scroll position when navigating to detail and back.
- Show a 6 px rounded scrollbar (`borderStrong` thumb); do not hide it on
  desktop.

## 8. Window behavior (optional platform layer)

System decorations are acceptable. If you go borderless later, provide drag,
double-click maximize, resize edges, window controls and keyboard shortcuts; add
Mica/Acrylic/background blur only when the platform supports it and always fall
back to the solid `bg`. The layout must be identical with or without effects.
