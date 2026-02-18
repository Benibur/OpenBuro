---
phase: quick-2
plan: 01
subsystem: infra
tags: [fonts, tailwind, phosphor-icons, cdn, self-hosted, woff2, performance, privacy]

# Dependency graph
requires: []
provides:
  - Local copies of all external CSS/JS resources under assets/
  - assets/js/tailwind-browser.js (Tailwind CSS browser runtime, 257KB)
  - assets/css/fonts.css (18 @font-face declarations, latin + latin-ext subsets)
  - assets/css/phosphor-duotone.css (Phosphor Duotone icon stylesheet, local font path)
  - assets/fonts/ (18 Google Font woff2 files for IBM Plex Mono, IBM Plex Sans, Plus Jakarta Sans)
  - assets/fonts/phosphor/Phosphor-Duotone.woff2 (Phosphor icon font)
  - index.html updated to load zero external resources
affects: [performance, privacy, offline-capability, deployment]

# Tech tracking
tech-stack:
  added: []
  patterns: [self-hosted fonts via @font-face with local relative paths, latin+latin-ext subset strategy for European audience]

key-files:
  created:
    - assets/js/tailwind-browser.js
    - assets/css/fonts.css
    - assets/css/phosphor-duotone.css
    - assets/fonts/ibm-plex-mono-400-latin.woff2
    - assets/fonts/ibm-plex-mono-400-latin-ext.woff2
    - assets/fonts/ibm-plex-mono-500-latin.woff2
    - assets/fonts/ibm-plex-mono-500-latin-ext.woff2
    - assets/fonts/ibm-plex-sans-400-latin.woff2
    - assets/fonts/ibm-plex-sans-400-latin-ext.woff2
    - assets/fonts/ibm-plex-sans-500-latin.woff2
    - assets/fonts/ibm-plex-sans-500-latin-ext.woff2
    - assets/fonts/ibm-plex-sans-600-latin.woff2
    - assets/fonts/ibm-plex-sans-600-latin-ext.woff2
    - assets/fonts/plus-jakarta-sans-500-latin.woff2
    - assets/fonts/plus-jakarta-sans-500-latin-ext.woff2
    - assets/fonts/plus-jakarta-sans-600-latin.woff2
    - assets/fonts/plus-jakarta-sans-600-latin-ext.woff2
    - assets/fonts/plus-jakarta-sans-700-latin.woff2
    - assets/fonts/plus-jakarta-sans-700-latin-ext.woff2
    - assets/fonts/plus-jakarta-sans-800-latin.woff2
    - assets/fonts/plus-jakarta-sans-800-latin-ext.woff2
    - assets/fonts/phosphor/Phosphor-Duotone.woff2
  modified:
    - index.html

key-decisions:
  - "Latin + latin-ext subsets only (skip cyrillic, vietnamese, greek) - European site, covers all EU languages"
  - "woff2-only in @font-face src (drop woff/ttf/svg) - universal browser support, no extra files needed"
  - "Separate files per font weight even when underlying bytes are identical (variable font) - browser needs distinct @font-face declarations per weight"

patterns-established:
  - "Local font @font-face pattern: relative path from assets/css/ to assets/fonts/ via ../fonts/"
  - "Phosphor icon CSS path: url('../fonts/phosphor/Phosphor-Duotone.woff2') from assets/css/"

requirements-completed: []

# Metrics
duration: 8min
completed: 2026-02-18
---

# Quick Task 2: Move All External Resources Locally - Summary

**All CDN dependencies eliminated: Tailwind CSS (257KB), Phosphor Icons (161KB), and 18 Google Font woff2 files served locally with zero external HTTP requests on page load.**

## Performance

- **Duration:** ~8 min
- **Started:** 2026-02-18T11:22:00Z
- **Completed:** 2026-02-18T11:30:00Z
- **Tasks:** 2
- **Files modified:** 23 (1 modified, 22 created)

## Accomplishments
- Downloaded Tailwind CSS browser runtime (@tailwindcss/browser@4) locally as assets/js/tailwind-browser.js
- Downloaded Phosphor Duotone CSS and woff2 font; fixed font path from CDN-relative to local relative path; dropped woff/ttf/svg formats keeping only woff2
- Downloaded 18 Google Font woff2 files (IBM Plex Mono 400/500, IBM Plex Sans 400/500/600, Plus Jakarta Sans 500/600/700/800 - latin + latin-ext subsets only)
- Created assets/css/fonts.css with 18 @font-face declarations preserving exact unicode-range values from Google Fonts
- Updated index.html: replaced 5 CDN lines (tailwind script, 2 preconnect links, google fonts link, phosphor link) with 3 local references

## Task Commits

Each task was committed atomically:

1. **Task 1: Download all external resources locally** - `70af64c` (feat)
2. **Task 2: Update index.html to reference local resources** - `6651850` (feat)

## Files Created/Modified
- `assets/js/tailwind-browser.js` - Tailwind CSS browser runtime (257KB)
- `assets/css/phosphor-duotone.css` - Phosphor Duotone icon stylesheet with local font path
- `assets/css/fonts.css` - 18 @font-face declarations for all 3 font families
- `assets/fonts/phosphor/Phosphor-Duotone.woff2` - Phosphor icon font (161KB)
- `assets/fonts/ibm-plex-mono-*.woff2` - 4 files (400/500 weights, latin/latin-ext)
- `assets/fonts/ibm-plex-sans-*.woff2` - 6 files (400/500/600 weights, latin/latin-ext)
- `assets/fonts/plus-jakarta-sans-*.woff2` - 8 files (500/600/700/800 weights, latin/latin-ext)
- `index.html` - Replaced 5 CDN lines with 3 local resource links; removed preconnect hints

## Decisions Made
- Latin + latin-ext subsets only: skipped cyrillic, vietnamese, greek - this is a European site and latin + latin-ext covers all EU languages including Eastern European characters
- woff2-only in @font-face src: dropped woff, ttf, svg format fallbacks - woff2 has universal browser support in all modern browsers, no fallbacks needed
- Separate named files per font weight: even though some Google Fonts weight variants share the same underlying variable font bytes, distinct local file names enable proper @font-face weight declarations

## Deviations from Plan

None - plan executed exactly as written.

## Issues Encountered
- Initial parallel font downloads using `cd` in bash had a race condition where the `cd` only applied to the first background job, causing some files to be downloaded to the repo root instead of the fonts directory. Cleaned up the stray files and re-downloaded with absolute paths. No impact on final state.

## Next Phase Readiness
- Page loads with zero external domain requests
- All fonts, icons, and Tailwind CSS served locally
- Ready for offline use or deployment to any static host without CDN dependency

---
*Phase: quick-2*
*Completed: 2026-02-18*
