---
phase: quick-3
plan: 01
type: execute
wave: 1
depends_on: []
files_modified: [index.html]
autonomous: true
requirements: [QUICK-3]

must_haves:
  truths:
    - "A bold BETA badge is visible in the hero section near the main title"
    - "The badge is visually prominent — large, colored, impossible to miss"
    - "The badge works in both light and dark mode"
    - "The badge animates in with the rest of the hero text sequence"
    - "The badge is responsive and looks good on mobile"
  artifacts:
    - path: "index.html"
      provides: "BETA badge CSS class and HTML element in hero"
      contains: "beta-badge"
  key_links:
    - from: ".beta-badge CSS"
      to: "hero-content HTML"
      via: "class applied to span element near h1"
      pattern: "beta-badge"
---

<objective>
Add a visually prominent BETA badge in the hero title area of the Open Buro landing page.

Purpose: Clearly signal to visitors that the project is in beta stage. The badge must be eye-catching — a bold colored tag that stands out against the hero background, positioned near the main h1 title.
Output: Updated index.html with BETA badge CSS and HTML.
</objective>

<execution_context>
@/home/ben/.claude/get-shit-done/workflows/execute-plan.md
@/home/ben/.claude/get-shit-done/templates/summary.md
</execution_context>

<context>
@.planning/STATE.md
@index.html (hero section lines ~2109-2118, badge CSS lines ~501-509, CSS custom properties lines ~84-126)
</context>

<tasks>

<task type="auto">
  <name>Task 1: Add BETA badge CSS and HTML to hero section</name>
  <files>index.html</files>
  <action>
1. Add a new CSS class `.beta-badge` in the BADGES section (after the existing `.badge` rule, around line 509). Style it as a bold, visually prominent tag:
   - `display: inline-block`
   - `font-family: 'Syne', sans-serif` (match heading font for boldness)
   - `font-size: 1rem` (larger than regular badges)
   - `font-weight: 800`
   - `letter-spacing: 0.15em`
   - `text-transform: uppercase`
   - `color: #ffffff`
   - `background: linear-gradient(135deg, #f59e0b, #ef4444)` — warm amber-to-red gradient for maximum visual punch against the dark hero
   - `padding: 0.4rem 1.2rem`
   - `border-radius: 6px`
   - `box-shadow: 0 0 20px rgba(245, 158, 11, 0.4), 0 4px 12px rgba(0, 0, 0, 0.3)` — glowing effect
   - `position: relative` (for potential pseudo-element shimmer)
   - `animation: betaPulse 2s ease-in-out infinite` — subtle pulse to draw attention
   - Do NOT use CSS custom properties for the gradient colors — this badge should be the same bold amber-red in both light and dark mode (it only appears on the dark hero background)

2. Add a `@keyframes betaPulse` animation:
   - 0%, 100%: `box-shadow: 0 0 20px rgba(245, 158, 11, 0.4), 0 4px 12px rgba(0, 0, 0, 0.3)`
   - 50%: `box-shadow: 0 0 30px rgba(245, 158, 11, 0.6), 0 4px 16px rgba(0, 0, 0, 0.4)`
   This creates a gentle glow pulse without being obnoxious.

3. Add `prefers-reduced-motion` override for `.beta-badge`:
   - Inside the existing `@media (prefers-reduced-motion: reduce)` block (around line 833), add:
   - `.beta-badge { animation: none; }`

4. In the hero HTML (line 2113, the h1 element), add the BETA badge inline next to the title. Replace the current h1 line:
   ```
   <h1 class="hero-text" style="color: #ffffff;">The European Standard for Workplace Orchestration</h1>
   ```
   with:
   ```
   <h1 class="hero-text" style="color: #ffffff;">The European Standard for Workplace Orchestration <span class="beta-badge" aria-label="Beta version">BETA</span></h1>
   ```
   This positions the badge right next to the title text, inline, so it flows naturally with the heading. The `aria-label` ensures screen readers announce it properly.

5. Add a small responsive tweak inside the existing `@media (max-width: 400px)` block (around line 2052):
   ```css
   .beta-badge {
     font-size: 0.75rem;
     padding: 0.3rem 0.8rem;
   }
   ```
   This prevents the badge from being too large on very small screens.
  </action>
  <verify>
  Open index.html in a browser. Confirm:
  - The BETA badge appears inline next to the h1 title in the hero section
  - It has a bold amber-to-red gradient background with white text
  - It has a subtle glowing pulse animation
  - It is clearly visible and prominent against the dark hero background
  - On mobile viewport (400px), the badge scales down proportionally
  - Run: grep -n "beta-badge" index.html (should show CSS class definition, keyframes, reduced-motion override, and HTML usage)
  </verify>
  <done>A bold, eye-catching BETA badge with amber-red gradient, glow effect, and pulse animation is displayed inline next to the hero h1 title. It is accessible (aria-label), responsive, and respects reduced-motion preferences.</done>
</task>

</tasks>

<verification>
- Visual: BETA badge is impossible to miss in the hero section
- Accessibility: aria-label present, reduced-motion respected
- Responsive: Badge scales on small screens
- Code: grep confirms beta-badge class in CSS and HTML
</verification>

<success_criteria>
The BETA badge is visually prominent in the hero area — a bold colored tag with gradient background and glow effect, positioned inline with the main h1 title. It animates with a subtle pulse, respects accessibility preferences, and scales on mobile.
</success_criteria>

<output>
After completion, create `.planning/quick/3-add-a-visually-prominent-beta-badge-in-t/3-SUMMARY.md`
</output>
