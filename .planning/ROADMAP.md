# Roadmap: Open Buro Landing Page

## Overview

The journey from empty repository to a performant, visually striking single-page landing site that recruits Alliance members. Seven phases deliver the site progressively: foundation establishes structure and design system, hero creates the first impression with canvas animation, content sections communicate the value proposition, timeline and ecosystem showcase progress and partners, news and newsletter enable engagement, animations add polish and interactivity, and final optimization ensures production-ready performance.

## Phases

**Phase Numbering:**
- Integer phases (1, 2, 3): Planned milestone work
- Decimal phases (2.1, 2.2): Urgent insertions (marked with INSERTED)

Decimal phases appear between their surrounding integers in numeric order.

- [ ] **Phase 1: Foundation & Design System** - HTML structure, dark mode CSS, responsive layout
- [ ] **Phase 2: Hero & Canvas Animation** - Hero section with animated constellation background
- [ ] **Phase 3: Core Content Sections** - Problem, Vision, and Pillars sections
- [ ] **Phase 4: Timeline & Ecosystem** - Progress timeline and partner showcase
- [ ] **Phase 5: News & Newsletter** - Article cards and email signup form
- [ ] **Phase 6: Animations & Interactivity** - Scroll animations, mobile nav, theme controls
- [ ] **Phase 7: Performance & Launch** - SEO completion, optimization, accessibility audit

## Phase Details

### Phase 1: Foundation & Design System
**Goal**: Establish semantic HTML structure and dark mode design system that prevents FOUC and supports all future phases
**Depends on**: Nothing (first phase)
**Requirements**: NAV-01, NAV-05, NAV-06, FOOT-01, FOOT-02, DS-01, DS-02, DS-03, DS-04, DS-05, SEO-01, SEO-02, SEO-03, PERF-04
**Success Criteria** (what must be TRUE):
  1. User can view all 9 content sections in dark mode without page flash on load
  2. User sees readable content on mobile, tablet, and desktop with proper spacing and typography
  3. User sees sticky navbar with Open Buro logo and "Join the Alliance" CTA button
  4. User sees footer with navigation links, contact email, social media links, and license info
  5. Page loads as single HTML file with inline CSS and metadata for SEO/social sharing
**Plans**: TBD

Plans:
- (Plans will be generated via /gsd:plan-phase)

### Phase 2: Hero & Canvas Animation
**Goal**: Deliver impressive first-screen experience with animated network constellation that performs smoothly on mobile
**Depends on**: Phase 1
**Requirements**: HERO-01, HERO-02, HERO-03, HERO-04, HERO-05, HERO-06
**Success Criteria** (what must be TRUE):
  1. User sees full-height hero with animated constellation of 15-25 connected nodes drifting smoothly
  2. Canvas animation runs at 60 FPS on desktop and 30+ FPS on mobile without battery drain
  3. User sees hero text elements (badge, headline, subtitle, CTAs, credit) appear in smooth sequence
  4. Animated scroll indicator pulses at bottom of hero, guiding user to scroll
  5. Animation pauses when offscreen and respects prefers-reduced-motion preference
**Plans**: TBD

Plans:
- (Plans will be generated via /gsd:plan-phase)

### Phase 3: Core Content Sections
**Goal**: Communicate Open Buro's value proposition through Problem, Vision, and Pillars sections
**Depends on**: Phase 2
**Requirements**: PROB-01, PROB-02, PROB-03, VIS-01, VIS-02, VIS-03, VIS-04, VIS-05, PIL-01, PIL-02, PIL-03, PIL-04
**Success Criteria** (what must be TRUE):
  1. User sees Problem section with 3 constat cards explaining current marketplace challenges
  2. User sees Vision section with animated transformation diagram showing isolated apps becoming unified workplace
  3. Transformation diagram lines draw in smoothly when section enters viewport
  4. User sees Pillars section with Standard technical axes (7 expandable accordions) and Alliance mission blocks
  5. Pull quote "Code is Law... but Architecture is Politics" displays with distinctive styling
**Plans**: TBD

Plans:
- (Plans will be generated via /gsd:plan-phase)

### Phase 4: Timeline & Ecosystem
**Goal**: Showcase project momentum through visual timeline and display founding partners with recruitment CTA
**Depends on**: Phase 3
**Requirements**: TIME-01, TIME-02, TIME-03, TIME-04, TIME-05, ECO-01, ECO-02, ECO-03, ECO-04
**Success Criteria** (what must be TRUE):
  1. User sees horizontal timeline with 6 milestones spanning 2024 Q3 through 2026 S2
  2. Past milestones display green/completed state, current milestone pulses blue, future milestones show muted
  3. Timeline renders vertically on mobile with proper spacing and readability
  4. User sees founding members (LINAGORA/Twake.ai, DINUM/La Suite numerique) as styled text placeholders
  5. Prominent CTA block with gradient border invites visitors to join the Alliance
**Plans**: 2 plans

Plans:
- [ ] 04-01-PLAN.md — Timeline section with 6 milestones and state-based styling
- [ ] 04-02-PLAN.md — Ecosystem section with member display and gradient CTA

### Phase 5: News & Newsletter
**Goal**: Enable visitor engagement through news display and email capture with GDPR compliance
**Depends on**: Phase 4
**Requirements**: NEWS-01, NEWS-02, NEWS-03, NEWS-04, NLTR-01, NLTR-02, NLTR-03
**Success Criteria** (what must be TRUE):
  1. User sees 3 article cards with date, placeholder image, title, excerpt, and read link
  2. Article cards scroll horizontally on mobile and display in grid on desktop
  3. User can submit email address via newsletter form with configurable action endpoint
  4. Form displays GDPR consent text and validates email format before submission
  5. Newsletter section appears with email icon, clear heading, and description
**Plans**: 1 plan

Plans:
- [ ] 05-01-PLAN.md — Add News section (3 article cards) and Newsletter section (email form with GDPR compliance)

### Phase 6: Animations & Interactivity
**Goal**: Add scroll-triggered animations, mobile navigation, and interactive polish that elevates experience without sacrificing performance
**Depends on**: Phase 5
**Requirements**: NAV-02, NAV-03, NAV-04, ANIM-01, ANIM-02, ANIM-03, ANIM-04, ANIM-05, ANIM-06, ANIM-07
**Success Criteria** (what must be TRUE):
  1. All content sections fade in with translateY animation as user scrolls (Intersection Observer)
  2. Navbar transitions from transparent to opaque background with blur as user scrolls past hero
  3. Active section highlights in navbar based on scroll position
  4. User can open/close mobile hamburger menu with smooth slide-out drawer and proper ARIA labels
  5. Card grids animate with staggered cascade (100-150ms delay), cards elevate on hover
  6. All animations respect prefers-reduced-motion and use single shared Intersection Observer
**Plans**: TBD

Plans:
- (Plans will be generated via /gsd:plan-phase)

### Phase 7: Performance & Launch
**Goal**: Optimize for production deployment with Lighthouse 90+ scores and launch-ready assets
**Depends on**: Phase 6
**Requirements**: PERF-01, PERF-02, PERF-03
**Success Criteria** (what must be TRUE):
  1. Page loads in under 2 seconds on 4G connection (tested with Chrome DevTools throttling)
  2. Lighthouse audit scores exceed 90 for Performance, Accessibility, and SEO
  3. No third-party cookies or tracking scripts present (GDPR by design)
  4. Site works as static file drop on any hosting (tested with local file:// protocol)
  5. All images optimized, fonts loaded efficiently, critical CSS inlined
**Plans**: TBD

Plans:
- (Plans will be generated via /gsd:plan-phase)

## Progress

**Execution Order:**
Phases execute in numeric order: 1 → 2 → 3 → 4 → 5 → 6 → 7

| Phase | Plans Complete | Status | Completed |
|-------|----------------|--------|-----------|
| 1. Foundation & Design System | 0/TBD | Not started | - |
| 2. Hero & Canvas Animation | 0/TBD | Not started | - |
| 3. Core Content Sections | 0/TBD | Not started | - |
| 4. Timeline & Ecosystem | 0/2 | Planned | - |
| 5. News & Newsletter | 0/1 | Planned | - |
| 6. Animations & Interactivity | 0/TBD | Not started | - |
| 7. Performance & Launch | 0/TBD | Not started | - |
