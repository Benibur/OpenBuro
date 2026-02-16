# Project State

## Project Reference

See: .planning/PROJECT.md (updated 2026-02-16)

**Core value:** Clearly communicate that Open Buro is the missing orchestration layer turning isolated open source apps into a real platform, and make it effortless for visitors to join the Alliance.
**Current focus:** Phase 6 - Animations & Interactivity (COMPLETE)

## Current Position

Phase: 6 of 7 (Animations & Interactivity)
Plan: 2 of 2 in current phase - PHASE COMPLETE
Status: Phase 6 complete, ready for Phase 7
Last activity: 2026-02-17 — Completed Phase 06 Plans 01-02 (Scroll animations, navbar transitions, mobile menu, hover polish)

Progress: [█████████░] 90%

## Performance Metrics

**Velocity:**
- Total plans completed: 10
- Average duration: 132 seconds (2.2 minutes)
- Total execution time: 0.37 hours

**By Phase:**

| Phase | Plans | Total | Avg/Plan |
|-------|-------|-------|----------|
| 01 | 2/2 ✓ | 311s | 156s |
| 02 | 1/1 ✓ | 116s | 116s |
| 03 | 2/2 ✓ | 264s | 132s |
| 04 | 2/2 ✓ | 146s | 73s |
| 05 | 1/1 ✓ | 125s | 125s |
| 06 | 2/2 ✓ | 345s | 173s |

**Recent Trend:**
- Last 5 plans: 04-01 (~73s), 04-02 (~73s), 05-01 (125s), 06-01 (~170s), 06-02 (~175s)
- Trend: Phase 6 slightly longer due to animation complexity and mobile drawer

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
- [Phase 01]: Layered CSS token system (primitive -> semantic -> component) for maintainable theming
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
- [Phase 05-01]: Flex horizontal scroll on mobile, CSS grid at 768px for news cards
- [Phase 05-01]: Date tag overlay with backdrop-filter blur on gradient images
- [Phase 05-01]: Inline SVG mail icon (Lucide-style) for newsletter section
- [Phase 05-01]: sr-only accessible label pattern for form inputs
- [Phase 05-01]: 600px narrow container for focused newsletter form UX
- [Phase 05-01]: Subtle gradient background for newsletter section visual separation
- [Phase 06-01]: Single shared IntersectionObserver with threshold 0.15 for all scroll animations
- [Phase 06-01]: requestAnimationFrame throttling for scroll handler (prevents jank)
- [Phase 06-01]: Navbar transitions from transparent to opaque at 50% hero height scroll
- [Phase 06-01]: GPU-accelerated animations only: transform and opacity
- [Phase 06-01]: observer.unobserve() after each trigger to prevent memory leaks
- [Phase 06-02]: Separate drawer element outside nav for z-index stacking clarity
- [Phase 06-02]: JS-created overlay element for clean separation
- [Phase 06-02]: @media (hover: hover) guards all hover effects from touch devices
- [Phase 06-02]: 48x48px minimum touch target on hamburger and close buttons
- [Phase 06-02]: Focus trapping with Tab/Shift+Tab wrapping in open drawer

### Pending Todos

None yet.

### Blockers/Concerns

None yet.

## Session Continuity

Last session: 2026-02-17 (Phase 6 execution)
Stopped at: Completed Phase 06 Plans 01-02 (Animations & Interactivity)
Resume file: None

---

**Next step:** Phase 6 complete. Ready to begin Phase 7 when requested.
