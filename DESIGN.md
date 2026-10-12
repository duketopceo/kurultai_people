# Kurultai Memory design direction: Council Ring (plugin panel)

Status: proposal, 2026-10-11. The owner signs off logos. This plugin inherits the parent repo's direction ("Council Ring", `kurultai/DESIGN.md`) so the two read as one product; it does not invent a second identity.

## Surfaces
| Surface | File | Notes |
|---|---|---|
| Settings panel | `webui/config.html` | hero, connection pill, form cards |
| Search modal | `webui/search-modal.html`, `webui/ui.js` | query field, hit cards with citations |
| Sidebar quick action | `extensions/webui/sidebar-quick-actions-main-end/kurultai-search.html` | 22 px logo button |
| Shared styles | `webui/ui-kit.css` | `--ui-*` tokens layered on Agent Zero theme variables |
| Agent-facing text | `prompts/*.md`, `helpers/normalize.py` | excerpts and citations: the real "UI" for the agent |
| README | `README.md` | |

## Direction
A council ring seen at plugin scale: the parent's ring, with three excerpt lines inside (what the plugin returns: short excerpts, not files) and one ember dot as the citation marker. Near-black canvas, warm ink, hairline borders, one ember accent for focus, primary action and live state only.

Research: Refero MCP was not available on this machine; the fallback is the parent repo's reference lock (Linear-style single-accent dark, rejecting purple/indigo category default, glassmorphism and gradient heroes) and the category peers listed in `docs/research/`. Decision ledger: ember accent (`#e8a33d`) replaces the old blue fallback `#6cc4ff`; Agent Zero's `--color-primary` still wins when the host sets it; no new fonts.

## Tokens (same values as the parent)
| Token | Dark | Light |
|---|---|---|
| bg | `#0c0d11` | `#f6f4ee` |
| panel | `#15171d` | `#ffffff` |
| ink | `#ece8df` | `#16171c` |
| ink-muted | `#9a968c` | `#5c594f` |
| accent | `#e8a33d` | `#9a5b00` |
| on-accent | `#0c0d11` | `#ffffff` |

Contrast (WCAG, computed): ink on bg 15.88:1; muted on panel 6.07:1; accent on bg 9.01:1; light accent on `#f6f4ee` 4.93:1; white on light accent 5.43:1; light muted on white 7.01:1. All pass AA.

Type: Agent Zero's own UI font; mono (`ui-monospace`) for source ids, scores and citations.

## Glyphs and motion
Stroked, 2.4 unit weight at 32, round caps. Motion: only the skeleton shimmer (1.4 s) and hover colour changes; both stop under `prefers-reduced-motion`.

## Assets
`assets/brand/`: `logo.svg`, `logo-dark.svg`, `logo-adaptive.svg` (follows the OS colour scheme; used inside the plugin UI), wordmarks, previews.
