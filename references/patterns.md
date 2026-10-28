# Patterns

Use these as recipes. Pick the archetype closest to the task, then adapt with
tokens — do not invent a new page grammar per screen.

## 1. Choose the archetype

| The screen is… | Archetype | Key block |
|---|---|---|
| A landing/home surface | Dashboard | Hero + shortcut strip + recent grid |
| A browsable set of things | Collection | Header + search/filter + list or grid |
| One thing with sub-content | Detail | Two columns ≥1040, single below |
| Configuration | Settings | Fixed 208 px nav + flat groups |
| A stream or system readout | Feed / metrics | Metric panel + chronological rows |
| Identity/profile | Account | Hero + list |
| Background work | Overlay | Task drawer + pinned queue strip |

If a screen needs two archetypes, split it into two regions with one rule — not
two competing card systems.

## 2. Compose a screen

1. **Header.** Title (24), optional one-line subtitle, actions right. Title says
   what the screen is, not what the product is.
2. **Primary content.** One dominant block (hero, list, grid, or detail body).
   Everything else supports it.
3. **Secondary content.** Recent items, shortcuts, metrics, notes. Lower
   contrast, smaller type, never competing for the first glance.
4. **States.** Loading skeleton, empty, error, no-results, offline, permission
   denied, partial data.
5. **Overlays.** Row actions, dialogs, toasts, task drawer. Only for short-lived
   or blocking choices.
6. **Responsive pass.** 600 / 820 / 1020 / 1400 / 2100. Check each side.
7. **Theme pass.** Light and dark, custom accent, tinted neutrals, reduced motion.

## 3. Page patterns

### Dashboard

- Hero panel is the only big filled block; accent border when there is an active
  object. Contains eyebrow, large title, meta, primary + secondary action.
- Shortcut strip is one panel with 1 px dividers (row ≥720, stacked below).
- Recent grid shows at most 4 items, tiles fill the row, whole tile selects.
- No debug/system cards on the landing page. Put them in settings/diagnostics.
- Empty: the hero becomes the create/import action; no separate empty panel.

### Collection

- Search + filters + sort + view toggle on one line; wrap below 600.
- Show a count or active-filter summary above the list.
- Rows are default; cards appear only when width ≥1020 and metadata still fits.
- Row: icon/mark, name + meta, last-used/date (wide only), one action, overflow.
- Selection is exclusive and visible; hover has no movement.
- Preserve query, filters, scroll and selection when returning from detail.

### Detail

- Back + title + primary action on the header line.
- Left/main: overview, media, description; right/side: versions/options/actions.
- ≥1040: side-by-side. Below: single column in reading order; the action comes
  after the content, not in a floating modal.
- Selection lists use `accentSoft` + 2 px left edge + check icon; the commit
  action is a single primary button at the bottom of that column.
- Show why an action is blocked inline, and keep the rest of the page usable.

### Settings

- One flat column of groups; no nested tabs.
- Group = label + rules + rows; note below the group, not inside a tooltip.
- Controls: switch for booleans, segmented for 2–4 choices, dropdown for more,
  stepper for small ranges, text field for free values.
- Defaults come from the product; empty means "inherit/auto", and the UI says so.
- Dangerous operations live in their own group at the bottom with confirmation.
- About/updates are not mixed with behavior settings.

### Feed / metrics

- Metric panel: 4 metrics ≥720, 2 ≥380, else 1. Label 11, value 22 tabular,
  meta 12, 3 px meter. No meter for absolute values.
- Feed: source health chips under the title; rows in reverse-chronological order;
  relative time (just now / minutes / hours / days / date).
- One refresh action updates both; no timer polling unless the product requires
  it (then show "updates every N s").
- Failure is per-source: mark the source unavailable, keep the rest.

### Account

- Hero: avatar, name, type, status, expiry. Never show tokens or secrets.
- List: switch, refresh, sign out, remove. Selected account has accent edge.
- Add flow: system sign-in / offline / third-party, each with a one-line
  explanation. Expired state is visible, not silent.
- Empty: the add panel is the empty state.

## 4. State design

| State | What to render |
|---|---|
| Loading <300 ms | nothing (avoid flash) |
| Loading >300 ms | skeleton matching the final shape |
| Empty | specific reason + one primary next step + optional secondary |
| No results | keep search/filters visible, say what to change, offer reset |
| Error | cause + retry + details/log link; keep the shell usable |
| Partial | show what loaded, mark the failed region inline |
| Offline | banner/notice with retry; do not block cached content |
| Success | toast (transient) or inline confirmation; no modal |
| Long task | task drawer + queue strip; page stays interactive |

Never: blank page on error, toast-only page errors, infinite spinner, disabled
button with no explanation, error color without text.

## 5. Forms

- Labels above fields; required marked once, not with asterisks everywhere.
- Group related fields with 16–24 px gaps; max width ~520 for text forms.
- Validate on blur/submit; keep the value and focus on the first invalid field.
- Explain consequences next to the control, not after submission.
- Inline help is 12 `textTertiary`; errors are 12 `danger` with an icon.
- Primary action at the end of the form; cancel is secondary. Long forms may use
  a sticky footer only if the content scrolls under it.
- Multi-step flows show step count and allow going back without losing input.

## 6. Destructive flows

1. Label the object by name ("Delete project Atlas?").
2. State impact: what is removed, what is kept, what cannot be undone.
3. Default to the safe choice; never pre-check data loss.
4. Use a filled `danger` confirm, not accent.
5. For irreversible bulk actions, require a second confirmation listing exact
   paths/counts.
6. Report the outcome (count, bytes, skipped items) and leave no half-finished
   state.

## 7. Writing

- Short, concrete, sentence case. No exclamation marks, no hype.
- Buttons are verbs ("Create", "Retry", "Open folder"), not "OK"/"Yes".
- Empty/error copy gives the reason and the next action.
- Numbers use one format everywhere; bytes use binary units with a space;
  percentages are integers unless precision matters.
- Truncate with ellipsis and a tooltip for full value; never wrap a control label.
- All strings go through the i18n layer; no concatenated sentences.
- Do not put brand names or product marketing in component copy.

## 8. Accessibility

- Contrast: body ≥4.5:1, large text/icons ≥3:1, focus ring ≥3:1 against adjacent
  colors.
- Keyboard: logical Tab order, visible focus, Enter/Space activate, Esc closes
  overlays, arrows move within menus/segmented controls.
- Screen readers: labels on icon buttons, `selected`/`checked`/`expanded` states,
  live regions for toasts, `aria-valuenow` for progress.
- 150%/200% zoom: no clipped text, no horizontal scroll for reading content,
  controls stay ≥32 px high.
- Long CJK and RTL text wrap; do not fix heights on text containers.
- Reduced motion and high contrast modes are supported; color is never the only
  status signal.
- Drag/right-click have keyboard-accessible equivalents.

## 9. Responsive behavior

- Collapse labels before shrinking hit targets; icons get tooltips.
- Settings sidebar → horizontal scrollable tabs below 820.
- Command bar: nav labels 820, account name 900, primary label 680.
- Cards → list when metadata no longer fits; grids add columns only when the
  minimum readable card width (~260–300) is preserved.
- Ultrawide: use the width for more columns or a side panel, not wider paragraphs.
  Cap reading text at ~72 characters.
- Never let the shell center a narrow column with empty sides.

## 10. Review rubric

Score each 0–2; anything below 2 is a fix.

| Area | 0 | 1 | 2 |
|---|---|---|---|
| Tokens | literals in pages | some tokens | all visual values tokenized |
| Hierarchy | cards/shadows everywhere | mixed | hairline + space + surface steps |
| Shape | random radii/pills | mostly consistent | square/bevel language held |
| States | default only | some hover/empty | all states incl. error/disabled |
| Motion | decorative/long | present but inconsistent | 120–240 ms, transform/opacity, off switch |
| Responsive | one width | partial | defined at all listed thresholds |
| Contrast | fails | borderline | ≥4.5:1 both themes |
| A11y | mouse only | partial | keyboard, labels, live regions, zoom |
| Content | vague | okay | specific, actionable, consistent format |
| Originality | copied third-party UI/assets | derivative | own assets, own layout, generic structure only |

## 11. Boundaries

- Do not reproduce another product's layout files, icons, illustrations, copy,
  fonts or pixel-level pages. Borrow only generic principles (spacing, hierarchy,
  restraint).
- Do not adopt another product's brand color as the accent.
- Do not add unlicensed fonts or assets; record source and license for every
  asset.
- Do not let a background image (or blur/glass effect) reduce text contrast.
