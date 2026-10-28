# Motion

Motion is feedback, not performance. If an animation can be removed without
losing information, remove it.

## 1. Scale

| Token | Value | Use |
|---|---|---|
| `fast` | 120 ms | color/opacity on hover, nav text, segmented selection |
| `normal` | 180 ms | page content enter, nav plate slide, bar height, toast enter |
| `slow` | 240 ms | larger layout/elevation changes |
| `easeOut` | `cubic-bezier(0.215, 0.61, 0.355, 1)` | enter, expand, move |
| `easeIn` | `cubic-bezier(0.55, 0.055, 0.675, 0.19)` | exit, collapse |

Rules:

- Never exceed 320 ms for a UI transition; never go below 100 ms for a visible
  fade.
- Animate `opacity` and `transform` only. Do not animate width, height, margin,
  top/left or layout-affecting properties.
- No bounce, spring overshoot, flash, or scale-up on hover/press.
- One motion per event: do not slide + scale + shadow + color at once.
- Movement distance is small: 8–10 px for content, full-height only for a shell
  page transition.

## 2. Interaction timing

| Event | Motion |
|---|---|
| Hover background/border | 120 ms ease-out |
| Nav selection plate | 180 ms ease-out position + width; 120 ms text/icon color |
| Button press | color change only (no scale) |
| Page content enter | 180 ms: opacity 0→1 + translateY 8→0 |
| Page → page (shell) | 320 ms: new page lifts 2% + old snapshot dissolves |
| Home → page | 320 ms: new page covers from bottom over old snapshot |
| Page → home | 320 ms: old page retracts downward, revealing home |
| Toast enter/leave | 180 ms opacity + 10 px translate |
| Dialog | 120 ms fade + slight upward move; scrim fades with it |
| Task drawer | 180–240 ms slide from right; scrim fades |
| Queue strip show/hide | 180 ms size + opacity |
| Indeterminate progress | 1400 ms repeating sweep |
| Skeleton pulse | 1100 ms opacity 0.45 ↔ 0.9 |
| Splash (optional) | ~1.2 s line-drawn frame, then fade out |

## 3. Shell page transitions

The shell keeps the visual boundary fixed: top bar stays, only the page area
moves.

- **Cover** (home → another page): render a snapshot of the outgoing page, slide
  the new page up from the bottom over it with ease-out. The TopBar is the upper
  clip boundary.
- **Lift** (page → page): snapshot the old page, lift the new page 2% into
  place, and dissolve the old snapshot away (alpha-only mask). No full-screen
  fade of both pages — that turns into a gray wash.
- **Retract** (any page → home): slide the old page down, revealing the home
  canvas already painted underneath.
- Snapshot with the platform's raster-thread image capture if available
  (e.g. `toImageSync`); never do a GPU→CPU readback on the UI thread.
- Draw snapshots at equal width, top-aligned; when the footer height changes,
  scale width only — never stretch the snapshot vertically.
- If capture fails, skip the snapshot (instant swap) rather than flashing.
- In-page route pushes (detail pages) use the simple 180 ms fade + 8 px rise.

## 4. Navigation plate

- One plate is shared by all nav items and moves between measured item rects.
- Selection change: 180 ms ease-out position and width.
- Relayout (window resize, language change, density change): snap to position —
  no slide, so the plate never lags a resize.
- Width/height changes come from real item measurements, not hard-coded widths.
- The home item has no plate and no hover block: the home page is the canvas.

## 5. Hover sheen (optional)

A pointer-following accent glow on interactive panels.

- Implementation: analytic radial falloff in a fragment shader; alpha capped at
  **0.10 hover** and **0.16 press** (combined never exceeds 0.16).
- Entry 160 ms, exit 220 ms (slower out avoids strobing), press pulse 120 ms up
  + 120 ms down.
- Radius ~220 px. Clipped to the panel's bevel shape; paints above content but
  ignores pointers.
- Pointer movement repaints one quad only — it must not rebuild the subtree.
- Fallback: plain `surface2` hover. No shader → no extra listener, no stack.

## 6. Reduced motion

Honor the OS/browser preference and an in-app toggle.

- Set all durations to **0** (state still changes instantly).
- Remove: dissolve masks, sweeping fills, skeleton pulsing, sheen, splash.
- Keep: opacity/state changes, progress value updates, focus rings.
- Indeterminate progress becomes a static centered segment; skeletons become a
  flat block.
- Test with reduced motion on; never leave an infinite ticker running.

## 7. Performance constraints

- Do not create a shader/program per frame or per widget. Compile once at app
  scope, share the program, and create one shader instance per program.
- Do not `setState` per frame; drive painters with an animation/listenable.
- Do not blur per frame; cache background blur and recompute only on background
  or size change.
- Do not animate the background layer; the canvas is a solid color by default.
- Effects are independent: if one program fails to load, disable only that
  effect and keep layout identical.
- Keep page transitions single-layer where possible; a snapshot + one moving
  layer is cheaper than two live subtrees.
- Verify on a 60 Hz software-rendered machine, not only on a fast GPU.

## 8. Optional shader effects and fallbacks

| Effect | Use | Cost | Fallback |
|---|---|---|---|
| Dissolve mask | page-to-page reveal | alpha-only, one layer | plain fade |
| Pointer glow | interactive panel sheen | one radial falloff per panel | hover background |

Anything else (grain, animated gradients, glass) is off by default. It must
degrade to the static design without changing spacing or contrast.

## 9. Anti-patterns

- Animating width/height/margin or re-laying out a page during a transition.
- Per-frame `setState`, per-frame shader creation, or a `Timer` driving motion.
- Fade-through-gray page swaps, cross-fades of two live pages.
- Hover scale/lift, press bounce, or shadow bloom on rows.
- Spinners on top of content that already has a skeleton.
- Infinite ambient animation on the canvas or chrome.
- Motion that cannot be turned off.
