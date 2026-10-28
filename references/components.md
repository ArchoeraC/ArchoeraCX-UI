# Components

Sizes are logical pixels. States are mandatory, not optional polish.

## 0. State matrix

| State | Treatment |
|---|---|
| Default | token surface + 1 px `border` |
| Hover | background `surface2` (or `accentHover` on accent fill); border may go `borderStrong` |
| Pressed | `accentPressed` for accent fills; `surface3` for neutral; no movement |
| Selected | `accentSoft` + 2 px `accent` edge and/or accent text |
| Focused | 2 px accent ring (or 2 px accent border) at 2 px offset; visible in both themes |
| Disabled | `surface3` + `textTertiary`; keep the label readable; explain why nearby |
| Loading | inline skeleton or a 16 px spinner; never blank the whole page |
| Error | `danger` icon + cause + retry + details link |

No state may be signaled by color alone.

## 1. Sections, panels, rows

### Section (page-level grouping)

- Transparent background, square corners, **no shadow**.
- 1 px `border` top and/or bottom. Default: bottom only; first/last groups may
  show both.
- Padding 24 vertical (12 for tight groups), child content supplies horizontal.
- Optional subtle `surface1 @ 42%` fill only for a rare highlighted block; never
  combine it with a border+shadow card look.
- Section title 18/26, 8–12 px above content.

### Panel (discrete interactive block)

- `surface1`, 1 px `border`, two cut corners: top-right `22`, bottom-left `8`.
- Padding 16 (12 for compact tiles, 24–32 for hero).
- Active/selected panel: border `accent @ 55%` and optional accent edge.
- Interactive panels get a pointer-following accent sheen capped at 10% hover /
  16% press, radius ~220, clipped by the same bevel shape. Fallback: a plain
  `surface2` hover. Static panels get no extra paint or pointer listeners.
- Panels are for hero blocks, metric groups, shortcut strips, cards. A page is
  not a stack of panels.

### Row (list / settings / result)

- Height 56–72; compact settings rows may be 44–48.
- Layout: leading 24–40 px (icon/avatar/mark) · 8–12 gap · title 14–16 + meta
  12 `textTertiary` · actions right.
- Selected: `accentSoft` background + 2 px accent left edge.
- Hover: `surface2`; no translate, no scale, no shadow.
- Secondary click opens the same overflow menu at the pointer.
- Overflow menu button is ghost: transparent, visible on hover/focus, always
  keyboard reachable.
- Trailing value: max ~380 px, single line, ellipsis, 12 `textTertiary`.

### Settings group

```
GROUP LABEL                          ← 11 / 600 / letter-spacing 1.2, secondary
────────────────────────────────────
[34 badge] Title                     ← 14 / 500
           Subtitle                  ← 12 / tertiary, max 2 lines
                          [control]  ← segmented / switch / value / chevron
────────────────────────────────────
note                                 ← 12 / tertiary, 8 px below
```

- Group = top + bottom rules, rows separated by 1 px `border` indented 12 px.
- Icon badge: 34 × 34, `accent @ 10%`, radius 4, icon 18 accent. Not a filled
  accent square.
- Control aligns right, vertical center; row padding 12.
- Whole row is clickable only when it has one obvious action; otherwise the
  control itself is the hit target.

## 2. Buttons

| Type | Height | Shape | Use |
|---|---|---|---|
| Primary | 40 (44 hero) | radius 4, accent fill, `onAccent` | launch/save/confirm, one per region |
| Secondary | 40 (44 hero) | radius 4, `surface2` or transparent + border | cancel, alternative |
| Ghost / icon | 36–40 | radius 4, transparent until hover (`surface2` / `accentSoft`) | toolbar, row actions, close |
| Text | 36 | radius 2, accent label | inline "view all", retry |
| Danger | 40 | `danger` fill or danger text | delete, stop, clear |

- Horizontal padding 18 (12 for text), icon 18–20 + 8 gap.
- Disabled: `surface3` + `textTertiary`; do not remove the button from layout.
- One primary action per region. A destructive primary is filled `danger`, not
  accent.
- Icon-only buttons always have a tooltip; 38 × 38 minimum hit area.
- No pill buttons, no gradients, no elevation on buttons.

## 3. Inputs and selectors

### Text field / search

- Fill `surface2`, 1 px `border`, radius 4, padding 14 × 12.
- Hint `textTertiary`; focus border `accent` (1 px, plus a 2 px ring if the
  platform needs it); error border `danger`.
- Label sits above the field (12 secondary) or inside the decoration; do not
  keep the default underline of the UI framework.
- Search: 16–18 px leading icon, clear button only when non-empty.

### Dropdown

- Trigger: `surface2`, 1 px `border`, radius 4, 10/6/5/5 padding, 16 px chevron
  `textSecondary`, body text 14. Expanded variant fills the column.
- Popup: `surface1`, 1 px border, bevel (top-right 22 / bottom-left 8), shadow
  `0 8 24`. Options 36–40 high; selected option shows a 16 px accent check in a
  fixed 16 px gutter so labels do not shift.
- Clip the popup with anti-aliasing so hover/selection highlight cannot spill
  past the beveled edge.
- Same popup primitive for settings dropdowns, filter dropdowns and form fields;
  never mix a default framework menu with the custom one.
- Keyboard: arrows move, type-ahead filters, Enter selects, Esc closes and
  returns focus to the trigger.

### Menu (overflow / context)

- Reuse the dropdown popup surface and bevel.
- Entry: 20 px icon + 8 gap + label; danger entries use `danger` for icon and
  text; disabled entries remain readable and do not close the menu.
- Toolbar menu, row overflow and right-click menu share one entry model.
- Group related actions; put destructive actions last, separated by a rule.

### Segmented control

- Track `surface2`, 1 px `border`, radius 4, 3 px inner padding.
- Selected segment `accent @ 14%`, radius 2, label accent 600; others
  `textSecondary` 400. Transition 120 ms.
- 2–4 options. More than four → dropdown.

### Switch

- On: `accent` track, `onAccent` thumb. Off: `surface3` track, `textTertiary`
  thumb. 44 × 24 target minimum. Label is the row title, not a separate sentence.

### Number stepper

- `surface2` + 1 px border, radius 4, 2 px padding; 28 × 28 −/+ buttons with
  16 px icons; value 32 px wide, centered, tabular figures.
- Disable − at min and + at max instead of accepting invalid input.

### Tooltip

- `textPrimary` background, `bg` text, 12 px, radius 2, 500 ms delay.
- Every icon-only control and every truncated metric needs one. Tooltips never
  carry essential-only information.

### Scrollbar

- 6 px thick, fully rounded thumb `borderStrong`, transparent track.
- Visible on desktop; do not auto-hide on hover-only.

## 4. Dialogs, confirmations, notices

### Dialog

- `surface1`, 1 px border, bevel top-left 22 / bottom-right 8, shadow; scrim
  `overlay` (40% light / 65% dark). Max width 520; padding 24.
- Title 18–20 primary; body 14 secondary; actions right-aligned, 8 gap.
- Esc and scrim click dismiss (unless the action is in-flight).
- Keep the trigger visible behind the scrim; do not stack dialogs.

### Destructive confirmation

```
[! danger]  Delete this item?
            What will be removed and what cannot be undone.
            [ ] also delete local files        ← only if relevant
            [Cancel]                [Delete]    ← filled danger
```

- Explain impact before the buttons; never a bare "Are you sure?".
- Both actions are easy to hit; cancel is not a tiny link.
- For irreversible, high-impact operations add a **second** confirmation that
  lists exactly what will be deleted (paths, counts) and state that it is
  permanent.
- A checkbox is never pre-checked for data loss; the default is the safe option.

### Toast / inline notice

- Position: top center, 12 px below the title bar, max width 460, max 5 items.
- Panel: `surface1`, bevel, 1 px border tinted 60% with the type color, shadow.
- Icon 18 + 8 gap + message 12, max 3 lines. 180 ms enter with 10 px upward
  motion; leave reverses.
- Duration 3 s (configurable 2–8). **Errors persist until dismissed.**
- Toasts never block the page, steal focus or replace page-level error states.

### Empty / error state

```
        [30px accent icon]
        Title (one line, 20–24)
        Why + what to do next (max 520 wide, 14 secondary)
        [Primary] [Secondary]
```

- Center with 32 padding, max width 520, actions wrap.
- Error adds: cause, retry, and a details/log entry.
- Page-level failure uses this block; a toast alone is not enough.
- Empty state is specific ("No projects yet — import one or create one"), not
  "Nothing here".

## 5. Loading

### Skeleton

- `surface3`, radius 2, height matching the final line (12–16), shapes matching
  the final layout.
- Pulse opacity 0.45 ↔ 0.9 over 1100 ms, low contrast.
- Appear only after ~300 ms to avoid flashing on fast loads.
- Use for lists, search results and queues; not for a single button action
  (use an inline spinner there).

### Progress bar

- 6 px high track `surface2`, 1 px `border`, chamfered top-right and bottom-left
  (cut = min(height × 0.7, 10)).
- Fill `accent`, leading edge slanted by ~60% of the height.
- Determinate: width = value, clamped 0–1. Indeterminate: 32% segment sweeping
  over 1400 ms; under reduced motion it sits statically centered.
- Always expose value semantics (e.g. `aria-valuenow` / semantic label).
- 4 px variant for the pinned queue strip.

## 6. Composite patterns

### Shortcut strip

One panel, cells separated by 1 px rules (vertical when horizontal, horizontal
when stacked). Each cell: 19 px accent icon, label 12 tertiary, value 14 primary,
18 px chevron tertiary; 12 × 16 padding; whole cell clickable.

### Metric tile

```
LABEL                  ← 11 tertiary, letter-spacing 1
42%                    ← 22 title, tabular figures
8 cores · load 0.4     ← 12 tertiary
[▬▬▬▬▬▬▬──────]         ← 3 px meter
```

Meter colors: accent <75%, warning ≥75%, danger ≥92%. Absolute values (MB, uptime)
hide the meter. Unknown values show `—`, never `0%`.

### Queue strip

Pinned to the bottom of the page region: 1 px top rule, 8 px vertical padding.
Running icon 16 accent · "N active" 12 primary + current item 11 tertiary ·
4 px progress · bytes and speed (≥860) · "Open" text button. Collapse the whole
strip when empty (animate size 180 ms).

### Task drawer

- Right-side overlay, 360–420 wide, full height below the title bar, `surface1`,
  left border, shadow. Background scrim optional (light) — Esc and a close button
  always work.
- Task row: 16–20 icon/type, title "category · target", stage 12 secondary,
  progress bar, speed/bytes 12 tertiary, cancel ghost action.
- Failed: `danger` stage + error code + Retry + View log.
- Keep the last N finished tasks; completion keeps the last byte count. Long
  tasks only — do not list every short request.

### Log viewer

- Toolbar: file dropdown (name + size, active file marked), follow toggle
  (running only), clear old, copy all, open folder.
- Body: mono 12, line height ~1.5, tail only (e.g. last 400 lines / 256 KiB),
  show "only the last N lines" when truncated.
- `ERROR/FATAL/SEVERE/Exception` danger, `WARN` warning, stack lines tertiary.
- Auto-follow until the user scrolls up; then stop and offer "jump to end".
- Never render unbounded logs or read files from the UI layer; stream.

### Chips and badges

- Overline 11/600/0.4–1.0, 4 × 6 padding, radius 2, `accentSoft` + accent text.
- Neutral chips use `surface2` + `textSecondary`.
- Unhealthy source: danger text + "Unavailable", tooltip with the error code.
- A selected filter chip uses accent border + accentSoft; do not use a filled
  accent pill.

## 7. Iconography

- One family only: outline, 24 px grid, 2 px stroke, round caps (Tabler-style).
- Sizes 16 / 18 / 20 / 24; never stretch or rotate for decoration.
- Color comes from `IconTheme` or a token: accent, secondary, tertiary, status.
  No two-tone, gradient or colored-in icons.
- Reference icons by **semantic name** in one registry; one meaning per name.
  Pages never call raw icon constants.
- Brand/loader/product marks are images, not substitutes for UI icons.
- Icon-only actions have a tooltip and an accessible label.

## 8. Density

Offer a compact density only if needed: reduce row padding 12 → 8 and control
heights 40 → 34, never font sizes. Do not create a second visual language for
"compact"; the same tokens and shapes apply.
