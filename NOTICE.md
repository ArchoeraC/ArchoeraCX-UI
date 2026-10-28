# Attribution and asset boundaries

This repository is an independent, re-authored UI specification derived from the
visual system of **ArchoeraCX**, whose upstream design documentation is licensed
under **AGPL-3.0**.

The text, token names, measurements, preview markup and CSS here are re-authored
for a portable Agent Skill. They are not a copy of the ArchoeraCX application
source and contain no business logic, backend contracts or product data.

No application assets are bundled:

- **Fonts** — the reference stack names Noto Sans SC / JetBrains Mono, but no font
  files are included. Use licensed copies or your platform's fallbacks.
- **Icons** — the preview uses small, hand-authored SVG glyphs. The ArchoeraCX
  icon font is not included.
- **Illustrations and screenshots** — none are included. `assets/preview-dark.png`
  is a PNG rendering of this repository's own `assets/preview.html`.
- **Brand assets** — the preview's simple diamond mark is a generic placeholder,
  not the ArchoeraCX application logo.

If you reuse ArchoeraCX source code itself, the upstream AGPL-3.0 obligations
apply to that code and to modified versions distributed to users. This notice
does not change those obligations.

The preview has no external dependencies and makes no network requests.
