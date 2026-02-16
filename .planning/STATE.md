# Project State

## Project Reference

See: .planning/PROJECT.md (updated 2026-02-16)

**Core value:** Clearly communicate that Open Buro is the missing orchestration layer turning isolated open source apps into a real platform, and make it effortless for visitors to join the Alliance.
**Current focus:** Phase 4 - Timeline & Ecosystem

## Current Position

Phase: 4 of 7 (Timeline & Ecosystem)
Plan: 2 of 2 in current phase - PHASE COMPLETE
Status: Phase 4 complete, ready for Phase 5
Last activity: 2026-02-17 — Completed Phase 04 Plan 02 (Ecosystem section with member cards and gradient CTA)

Progress: [███████░░░] 70%

## Performance Metrics

**Velocity:**
- Total plans completed: 7
- Average duration: 127 seconds (2.1 minutes)
- Total execution time: 0.25 hours

**By Phase:**

| Phase | Plans | Total | Avg/Plan |
|-------|-------|-------|----------|
| 01 | 2/2 ✓ | 311s | 156s |
| 02 | 1/1 ✓ | 116s | 116s |
| 03 | 2/2 ✓ | 264s | 132s |
| 04 | 2/2 ✓ | 146s | 73s |

**Recent Trend:**
- Last 5 plans: 02-01 (116s), 03-01 (146s), 03-02 (118s), 04-01 (~73s), 04-02 (~73s)
- Trend: Accelerating velocity, Phase 4 complete

## Accumulated Context

### Decisions

Decisions are logged in PROJECT.md Key Decisions table.
Recent decisions affecting current work:

- Single HTML file (no build step): Maximum deployability, simplest hosting
- Canvas animation for hero (not CSS SVG): More dynamic/impressive network effect, acceptable JS weight
- Tailwind via CDN (not local): No build step, rapid prototyping
- English content (overriding PRD's French default): Broader European audience reach
- Text placeholders for partner logos: No logo files available yet, swap later
- [Phase 01]: English content with French badges for brand identity (broader European reach)
- [Phase 01]: Inline SVG favicon for zero external asset dependencies
- [Phase 01]: Layered CSS token system (primitive → semantic → component) for maintainable theming
- [Phase 01]: Inline blocking script for FOUC prevention (only acceptable blocking script)
- [Phase 01]: Transform-based animations to avoid layout thrashing
- [Phase 02]: Canvas API for hero animation (not CSS/SVG) for dynamic constellation effect
- [Phase 02]: IntersectionObserver for automatic animation pause when offscreen (battery savings)
- [Phase 02]: Mobile particle reduction (15 vs 25) for smooth 30+ FPS on European 4G mobile
- [Phase 02]: CSS + JS animation combo (GPU-accelerated CSS with dynamic JS delays)
- [Phase 02]: YOLO mode checkpoint auto-approval for faster execution
- [Phase 03-01]: SVG inline (not external file) for single-file constraint
- [Phase 03-01]: stroke-dasharray draw-in animation for smooth SVG path reveals
- [Phase 03-01]: Intersection Observer threshold 0.3 triggers animation at 30% visibility
- [Phase 03-02]: Single-expand accordion behavior for focused user attention
- [Phase 03-02]: Smooth scroll to expanded accordion (scrollIntoView) keeps content visible
- [Phase 03-02]: Alliance blocks reuse constat card styling for design consistency
- [Phase 03-02]: max-height: 500px for accordion animation (covers all content without truncation)
- [Phase 04-01]: Mobile-first vertical timeline switching to horizontal at 768px breakpoint
- [Phase 04-01]: CSS state classes (milestone-completed/current/future) for semantic timeline styling
- [Phase 04-01]: prefers-reduced-motion: static box-shadow replaces pulse animation
- [Phase 04-02]: CSS mask-composite gradient border technique (no extra wrapper elements)
- [Phase 04-02]: Separate btn-eco-ghost class to preserve existing btn-ghost behavior
- [Phase 04-02]: auto-fit grid with 250px minimum for natural responsive column adaptation

### Pending Todos

None yet.

### Blockers/Concerns

None yet.

## Session Continuity

Last session: 2026-02-17 (Phase 4 execution)
Stopped at: Completed Phase 04 Plan 02 (Ecosystem section with member cards and gradient CTA)
Resume file: None

---

**Next step:** Phase 4 complete. Ready to begin Phase 5 when requested.
