# Requirements: Open Buro Landing Page

**Defined:** 2026-02-16
**Core Value:** Clearly communicate that Open Buro is the missing orchestration layer turning isolated open source apps into a real platform, and make it effortless for visitors to join the Alliance.

## v1 Requirements

Requirements for initial release. Each maps to roadmap phases.

### Navigation

- [ ] **NAV-01**: User can navigate to any section via sticky top navbar with smooth anchor scroll
- [ ] **NAV-02**: Navbar transitions from transparent to opaque background on scroll (backdrop-filter blur)
- [ ] **NAV-03**: User sees active section highlighted in navbar based on scroll position (Intersection Observer)
- [ ] **NAV-04**: User can open/close hamburger menu with slide-out drawer on mobile (<768px)
- [ ] **NAV-05**: "Join the Alliance" CTA button is always visible in navbar
- [ ] **NAV-06**: Logo displays "Open Buro" in Syne Bold with "open standard" badge

### Hero

- [ ] **HERO-01**: User sees full-screen (100vh) hero with gradient background and animated canvas constellation
- [ ] **HERO-02**: Canvas renders 15-25 drifting nodes connected by lines (requestAnimationFrame, pauses offscreen)
- [ ] **HERO-03**: Hero displays surtitle badge, H1 headline, subtitle, two CTAs (primary + secondary), and origin credit
- [ ] **HERO-04**: Text elements appear in sequenced fade-in animation (200ms stagger)
- [ ] **HERO-05**: Animated scroll indicator (chevron) visible at bottom of hero
- [ ] **HERO-06**: Canvas animation respects `prefers-reduced-motion` (disables or simplifies)

### Problem Section

- [ ] **PROB-01**: User sees "LE CONSTAT" badge, H2 title, and 2-column layout (text left, cards right)
- [ ] **PROB-02**: Three constat cards display with icon, title, and description
- [ ] **PROB-03**: Cards appear in staggered cascade animation on scroll (150ms delay between cards)

### Vision Section

- [ ] **VIS-01**: User sees "LA VISION" badge, H2 title, and subtitle
- [ ] **VIS-02**: Animated transformation diagram shows 3-step flow: isolated apps → Open Buro → unified workplace
- [ ] **VIS-03**: Diagram connection lines draw in on scroll (SVG path animation)
- [ ] **VIS-04**: Four feature cards display below diagram (packaging, navigation, interoperability, population management)
- [ ] **VIS-05**: Conclusion quote displayed in accent styling

### Pillars Section

- [ ] **PIL-01**: User sees "DEUX PILIERS" badge, H2 title, and 2-column layout
- [ ] **PIL-02**: Left column shows Standard technical axes as expandable accordions (7 categories)
- [ ] **PIL-03**: Right column shows Alliance mission with 5 descriptive blocks (manifesto, governance, adoption, visibility, funding)
- [ ] **PIL-04**: Pull quote "Code is Law... but Architecture is Politics" displayed with distinctive styling

### Timeline Section

- [ ] **TIME-01**: User sees "EN MOUVEMENT" badge, H2 title
- [ ] **TIME-02**: Horizontal timeline displays 6 milestones (2024 Q3 through 2026 S2)
- [ ] **TIME-03**: Past milestones show green/completed state, current milestone pulses blue, future milestones show muted/outline
- [ ] **TIME-04**: Timeline renders vertically on mobile with proper spacing
- [ ] **TIME-05**: Each milestone node shows title and description

### Ecosystem Section

- [ ] **ECO-01**: User sees "L'ECOSYSTEME" badge, H2 title
- [ ] **ECO-02**: Logo grid displays founding members as styled text placeholders (LINAGORA/Twake.ai, DINUM/La Suite numerique)
- [ ] **ECO-03**: Placeholder cards with dashed borders and "+" indicate space for future members
- [ ] **ECO-04**: CTA block with gradient border, descriptive text, primary button ("Join the Alliance") and ghost button

### News Section

- [ ] **NEWS-01**: User sees "ACTUALITES" badge, H2 title
- [ ] **NEWS-02**: Three article cards display horizontally (scrollable on mobile)
- [ ] **NEWS-03**: Each card shows date tag, gradient placeholder image, title, excerpt, and "Read" link
- [ ] **NEWS-04**: Article data is hardcoded but structured for easy extraction to CMS later

### Newsletter Section

- [ ] **NLTR-01**: User sees centered newsletter section with email icon, H2 title, description
- [ ] **NLTR-02**: Inline email input + submit button with configurable form action placeholder
- [ ] **NLTR-03**: GDPR consent text displayed below form field

### Footer

- [ ] **FOOT-01**: Footer displays logo, navigation links, contact email, social media links (GitHub, LinkedIn, Mastodon)
- [ ] **FOOT-02**: Copyright line with CC BY-SA 4.0 license and "European open source ecosystem" tagline

### Design System

- [ ] **DS-01**: Site uses dark mode design with CSS custom properties matching PRD color palette
- [ ] **DS-02**: Typography uses Syne (headings), IBM Plex Sans (body), IBM Plex Mono (code) from Google Fonts
- [ ] **DS-03**: Component styles match PRD specs: buttons (primary/secondary/ghost), cards (hover effects), badges
- [ ] **DS-04**: Responsive layout with mobile-first breakpoints at 768px, 1024px, 1440px
- [ ] **DS-05**: Section padding 6rem vertical (3rem mobile), container max 1200px centered

### Animations

- [ ] **ANIM-01**: All sections use scroll-triggered fade-in + translateY animation via Intersection Observer
- [ ] **ANIM-02**: Card grids use stagger animation (100-150ms delay between items)
- [ ] **ANIM-03**: Badges and icons use scale-in animation on scroll
- [ ] **ANIM-04**: Cards elevate on hover (translateY -4px + shadow + border accent, 300ms transition)
- [ ] **ANIM-05**: Links use animated underline (width 0 → 100% on hover)
- [ ] **ANIM-06**: Single shared Intersection Observer instance manages all animated elements
- [ ] **ANIM-07**: All animations respect `prefers-reduced-motion` media query

### SEO & Performance

- [ ] **SEO-01**: Page includes title, meta description, OG tags, Twitter card, canonical URL, lang="en"
- [ ] **SEO-02**: Semantic HTML structure (proper heading hierarchy, landmark regions, ARIA labels)
- [ ] **SEO-03**: SVG favicon (stylized "O" or network node)
- [ ] **PERF-01**: Page loads in < 2s on 4G connection
- [ ] **PERF-02**: Lighthouse score > 90 for Performance, Accessibility, and SEO
- [ ] **PERF-03**: No third-party cookies, no tracking scripts (RGPD by design)
- [ ] **PERF-04**: All content delivered in a single HTML file with inline CSS and JS

## v2 Requirements

Deferred to future release. Tracked but not in current roadmap.

### Internationalization

- **I18N-01**: User can switch between English, French, and German
- **I18N-02**: Language selector in navbar with JavaScript-based content swapping

### Content Management

- **CMS-01**: News articles loaded from markdown files or headless CMS
- **CMS-02**: Blog/resources section with dynamic content

### Member Experience

- **MEMB-01**: Alliance application form with submission workflow
- **MEMB-02**: Authenticated member area

### Analytics

- **ANLYT-01**: Privacy-respecting analytics integration (Matomo/Plausible)

### Theme

- **THEME-01**: Dark/Light mode toggle with system preference detection and localStorage persistence

## Out of Scope

Explicitly excluded. Documented to prevent scope creep.

| Feature | Reason |
|---------|--------|
| Server-side logic | Site is 100% static, no backend |
| Build step / bundler | Single HTML file, CDN dependencies only |
| Auto-playing video | Accessibility and performance concern (anti-feature per research) |
| JavaScript frameworks (React/Vue) | Overkill for single landing page, adds unnecessary weight |
| Cookie consent banner | No cookies used — RGPD by design |
| Live chat / chatbot | No support team bandwidth, email contact sufficient |
| Real-time interactive demos | Performance burden, maintenance cost, message clarity |
| Multi-page navigation | Breaks single-page focus, dilutes messaging |
| Stock photography | Undermines open source authenticity |

## Traceability

Which phases cover which requirements. Updated during roadmap creation.

| Requirement | Phase | Status |
|-------------|-------|--------|
| (populated by roadmapper) | | |

**Coverage:**
- v1 requirements: 47 total
- Mapped to phases: 0
- Unmapped: 47 (pending roadmap)

---
*Requirements defined: 2026-02-16*
*Last updated: 2026-02-16 after initial definition*
