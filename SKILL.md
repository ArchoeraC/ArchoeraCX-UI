---
name: archoera-ui
description: Build or restyle application UI in the ArchoeraCX design language — flat beveled surfaces, hairline separators instead of cards, token-only light/dark themes, a full-bleed desktop shell with a bottom command bar, and restrained 120–240 ms motion. Use when designing or implementing pages, components, or a theme that should feel clean, spacious, hierarchical and calm; when reviewing an interface against these rules; or when porting this visual system to another stack.
---

# Archoera UI

A framework-agnostic UI design system distilled from ArchoeraCX. It describes a
look and a set of rules, not an app: use it for any desktop-first product that
should read as clean, spacious, layered, and calm.

> Core idea: **hierarchy comes from hairlines, spacing and background steps — not
> from rounded cards, heavy shadows or decoration.** A distinctive background is
> allowed, but content always wins.

## When to use

- Building a new page, window shell, settings screen, list, detail view or dialog.
- Defining color/type/spacing/motion tokens for a product.
- Reviewing UI for consistency, contrast, density, motion or accessibility.
- Porting this visual language to Flutter, web, Qt/QML, Compose, SwiftUI, etc.

Do **not** use it to copy a third-party product's layout, assets or branding, or
to add business/architecture rules to a UI task.

## Design stance

1. **Token-driven.** Colors, type, radii, spacing and durations live in one token
   layer. Pages never hard-code visual values.
2. **Window-level hierarchy.** Title bar, content, command bar and overlays each
   have one job. At most three visual layers.
3. **Flat, angular structure.** Page grouping uses straight edges, cut corners
   (bevels) and 1 px rules. Rounded cards are not the default unit.
4. **Restrained motion.** 120–240 ms, ease-out, short distances. Motion explains
   state change; it never performs.
5. **Degradable.** Background effects, blur and shaders are optional. Every
   effect has a static fallback; content stays readable without it.

## Non-negotiables

- Only tokens or theme roles for color, type, spacing, radius and duration.
- Page-level sections: transparent, square, 1 px top/bottom rule. No filled
  rounded card, no shadow.
- Cut corners (bevels) are the signature: `8 / 14 / 22` on panels, dialogs,
  menus, toasts and the sliding nav plate. Buttons keep only `2–4` px radius.
- One accent color per product. Default brand accent `#4F7CFF` light /
  `#7A9BFF` dark. Never adopt another product's identity color.
- Body text contrast ≥ 4.5:1, large text ≥ 3:1, in both themes and over any
  background image.
- Motion is opacity/transform only. No bounce, flash, scale-up, or layout change
  inside an animation.
- Icon buttons always have tooltips/labels. Every icon has one meaning.
- A disabled control stays readable and explains itself; do not just gray it out.

## Fast token reference

Colors (full tables in `references/tokens.md`):

| Role | Light | Dark |
|---|---|---|
| accent | `#4F7CFF` | `#7A9BFF` |
| bg | `#F7F8FB` | `#10141B` |
| surface1 | `#FFFFFF` | `#171D26` |
| surface2 | `#F1F3F8` | `#1E2632` |
| surface3 | `#E8EBF3` | `#27313F` |
| textPrimary | `#111827` | `#F2F5FA` |
| textSecondary | `#4B5563` | `#AAB5C6` |
| textTertiary | `#8A94A6` | `#738094` |
| border | `#E3E7EF` | `#2A3442` |
| danger / success / warning | `#D64545` / `#2F9E6B` / `#B7791F` | `#FF7A7A` / `#54D39A` / `#F0B45A` |

Type: display `32/40`, title1 `24/32`, title2 `18/26`, body `14/22`,
bodyStrong `14/22 · 600`, caption `12/18`, overline `11/16 · 600 · 0.6`.
Default weight **500**; build hierarchy with size, color and space, not bold.
CJK and Latin share one family (Noto Sans SC Medium). Numbers, paths, hashes and
logs use a mono face (JetBrains Mono) with tabular figures where compared.

Space: `4 / 8 / 12 / 16 / 24 / 32 / 48 / 64`.
Radii: `2 / 4 / 4 / 6`; bevels: `8 / 14 / 22`; full radius only for avatars and
meters.
Motion: fast `120`, normal `180`, slow `240` ms, `easeOutCubic` (exit
`easeInCubic`).
Desktop shell: top bar `48`, command bar `60` (`46` on the home canvas), content
gutter `32` (`16–24` narrow), full-bleed width with a `2880` cap.

## Apply workflow

1. **Classify the surface.** Window background (L0), page (L1), section (L2),
   interactive panel/row (L3), overlay (L4). Do not add a layer you cannot name.
2. **Set tokens first.** Pick light/dark neutrals, one accent, type scale and
   motion. Derive hover/pressed/soft from the accent (see `references/tokens.md`).
3. **Build the shell.** Top bar (brand + global task entry), scrolling content
   column, bottom command bar (navigation left, account + primary action right).
4. **Compose pages from archetypes.** Dashboard, collection, detail, settings,
   feed, account — see `references/patterns.md`. Use sections and rules; only use
   panels for discrete interactive blocks (hero, metric group, card).
5. **Define every state** before styling extras: default, hover, pressed,
   selected, focused, disabled, loading, empty, error, success.
6. **Verify.** Run the checklist below, in both themes, at narrow and ultrawide
   widths, at 150%/200% zoom, with reduced motion on.

## Verify checklist

- [ ] No hard-coded color, font size, radius or duration in a page.
- [ ] Selected states use `accentSoft` + a 2 px accent edge or accent text.
- [ ] Hover changes background/border only — no translation, no scaling.
- [ ] Page sections have no fill/shadow; only true overlays float.
- [ ] Body contrast ≥ 4.5:1; text over imagery has a scrim.
- [ ] Every icon-only control has a tooltip and accessible name.
- [ ] Every list has skeleton (after ~300 ms), empty state and error state.
- [ ] Overlays close on Esc and restore focus; Tab order is logical.
- [ ] Reduced motion removes sweeps, sheens and transitions (keeps state).
- [ ] Narrow layout collapses to icons/tabs; ultrawide uses the real width.
- [ ] No third-party layout, icon, illustration, font or copy is reproduced.

## Porting to a stack

The rules are the same; only the primitives change.

| Stack | Cut-corner shape | Token carrier |
|---|---|---|
| Web | `clip-path: polygon(...)` | CSS custom properties (`assets/tokens.css`) |
| Flutter | `BeveledRectangleBorder` + `ShapeBorderClipper` | `ThemeExtension` + `BuildContext` getter |
| Qt / QML | `Shape` path or a custom painter | theme singleton / `QQmlPropertyMap` |
| Compose | `GenericShape` path | `CompositionLocal` + `MaterialTheme` |
| SwiftUI | custom `Shape` with a `Path` | `EnvironmentValues` |

A bevel is a straight diagonal cut, not a large border radius. Buttons stay at
2–4 px radius; only panels, dialogs, menus, toasts and the nav plate are beveled.
Numbers, paths and logs use the mono family with tabular figures.

## Reference map

Load only what the task needs:

| File | Contents |
|---|---|
| `references/tokens.md` | Full light/dark palette, accent derivation, type, spacing, radii, borders, elevation, contrast rules |
| `references/layout.md` | Window anatomy, dimensions, breakpoints, grids, page layouts, scrolling rules |
| `references/components.md` | Buttons, sections, panels, rows, settings, menus, dialogs, toasts, states, progress, task drawer, logs |
| `references/motion.md` | Durations, curves, page transitions, nav plate, glow, skeletons, reduced motion, performance |
| `references/patterns.md` | Page archetypes, state patterns, writing, accessibility, i18n, review rubric |
| `assets/tokens.json` | Machine-readable tokens (light/dark, type, space, motion, layout, breakpoints) |
| `assets/tokens.css` | CSS custom properties + small base recipes |
| `assets/preview.html` | Dependency-free visual reference page (light/dark toggle) |

## Anti-patterns

- A grid of identical rounded cards for page-level content.
- Drop shadows or blur used to fake hierarchy.
- A brand accent borrowed from another product, or multi-color/gradient icons.
- Nav items each drawing their own underline/pill; use one moving plate.
- Animated width/height/margin, per-frame shader creation, or `setState` loops.
- Full-screen scrims for ordinary messages; use inline notices or toasts.
- Pixel-copying another launcher/store/product; borrow only generic structure.

---

Origin: re-authored from the ArchoeraCX UI design system (`docs/11-ui-design-system.md`,
AGPL-3.0). This folder is an independent specification, not a source-code copy,
and contains no third-party assets. Keep the attribution if you redistribute it.
