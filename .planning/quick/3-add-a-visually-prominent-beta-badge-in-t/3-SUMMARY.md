---
phase: quick-3
plan: 01
subsystem: ui
tags: [css, animation, hero, badge, accessibility]

# Dependency graph
requires: []
provides:
  - BETA badge inline in hero h1 with amber-to-red gradient, glow, and pulse animation
affects: [index.html, hero section]

# Tech tracking
tech-stack:
  added: []
  patterns:
    - "betaPulse @keyframes for glow-based pulse animation (opacity-independent)"
    - "aria-label on inline badge span for screen reader clarity"
    - "prefers-reduced-motion override in hero reduced-motion block"

key-files:
  created: []
  modified:
    - index.html

key-decisions:
  - "BETA badge inline inside h1 (not above it) so it flows naturally with the heading text"
  - "Amber-to-red gradient hardcoded (not CSS custom properties) — badge only appears on dark hero, consistent across light/dark modes"
  - "vertical-align: middle on .beta-badge ensures alignment with uppercase h1 text"
  - "betaPulse animates box-shadow only (no transform/opacity) for GPU-safe glow pulse"

patterns-established:
  - "Badge pulse: box-shadow keyframes only, no layout-affecting properties"

requirements-completed: [QUICK-3]

# Metrics
duration: 2min
completed: 2026-02-20
---

# Quick Task 3: Add BETA Badge Summary

**Amber-to-red gradient BETA badge with glow pulse animation inline in hero h1, accessible via aria-label, responsive on mobile, and animation-safe under prefers-reduced-motion**

## Performance

- **Duration:** ~2 min
- **Started:** 2026-02-20T10:56:00Z
- **Completed:** 2026-02-20T10:58:19Z
- **Tasks:** 1
- **Files modified:** 1

## Accomplishments

- Added `.beta-badge` CSS class with bold Syne font, amber-to-red gradient background, and glowing box-shadow
- Added `@keyframes betaPulse` for a 2-second infinite glow pulse (box-shadow only, no layout effects)
- Added `prefers-reduced-motion` override to disable the pulse for users who prefer reduced motion
- Added responsive tweak inside `@media (max-width: 400px)` to scale down badge on very small screens
- Inserted `<span class="beta-badge" aria-label="Beta version">BETA</span>` inline inside the hero h1

## Task Commits

1. **Task 1: Add BETA badge CSS and HTML to hero section** - `99c15a1` (feat)

## Files Created/Modified

- `/home/ben/Dev-local/openburo/index.html` - Added .beta-badge CSS, @keyframes betaPulse, reduced-motion override, responsive tweak, and HTML element in hero h1

## Decisions Made

- Badge placed inline inside h1 (appended after the title text) rather than above or below it, so it reads as part of the heading and is impossible to miss
- Gradient colors hardcoded (`#f59e0b` to `#ef4444`) rather than using CSS custom properties — the badge only appears on the dark hero canvas background and must look consistent regardless of light/dark mode
- `vertical-align: middle` added to align the badge visually with the h1 uppercase text
- `betaPulse` animates only `box-shadow` (not opacity or transform), keeping it GPU-safe and non-distracting

## Deviations from Plan

None - plan executed exactly as written.

## Issues Encountered

None.

## User Setup Required

None - no external service configuration required.

## Next Phase Readiness

Quick task complete. The BETA badge is live in the hero section. No further action required.

---
*Phase: quick-3*
*Completed: 2026-02-20*
