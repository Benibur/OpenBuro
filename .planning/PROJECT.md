# Open Buro Landing Page

## What This Is

A single-page static landing website for Open Buro, the open European standard that orchestrates independent open source services into a unified workplace capable of rivaling Microsoft 365. The site lives at openburo.eu and targets European tech decision-makers, open source publishers, CIOs, and digital policy leaders — convincing them to join the Open Buro Alliance.

## Core Value

The site must clearly communicate that Open Buro is the missing orchestration layer that turns isolated open source apps into a real platform — and make it effortless for visitors to join the Alliance.

## Requirements

### Validated

(None yet — ship to validate)

### Active

- [ ] Single-file static HTML page (index.html) with all CSS/JS inline, deployable anywhere
- [ ] Dark-mode design with constellation/network visual motif throughout
- [ ] 9 content sections with anchor navigation: Hero, Problem, Vision, Two Pillars, Progress, Ecosystem, News, Newsletter, Footer
- [ ] Sticky navbar with smooth scroll, active section highlighting, mobile hamburger menu
- [ ] Hero section with dynamic canvas-animated network of nodes background
- [ ] Scroll-triggered animations (fade-in, stagger, draw-in) via Intersection Observer
- [ ] Responsive design: mobile-first, breakpoints at 768px, 1024px, 1440px
- [ ] Newsletter form with configurable placeholder action URL
- [ ] All content in English (structure ready for future i18n)
- [ ] SEO meta tags, Open Graph, Twitter card, favicon
- [ ] Lighthouse score > 90 (Performance, Accessibility, SEO)
- [ ] RGPD-compliant: no third-party cookies, no tracking
- [ ] Partner logos as styled text placeholders (LINAGORA/Twake.ai, DINUM/La Suite numerique)
- [ ] Timeline/roadmap visualization (horizontal desktop, vertical mobile)
- [ ] Transformation diagram (apps isolated -> Open Buro -> unified workplace) animated on scroll

### Out of Scope

- Server-side logic — site is 100% static, no backend
- Multi-language support — English only for v1, but HTML structure should anticipate i18n
- Blog/CMS — news articles are hardcoded in v1
- Analytics integration — no Matomo/Plausible in v1
- Dark/Light mode toggle — dark mode only for v1
- Member authentication area — future evolution
- Video/demo embeds — future evolution

## Context

Open Buro was born from the collaboration between La Suite numerique (DINUM, French government digital workplace) and Twake.ai (LINAGORA's collaborative platform). The insight: Europe has mature open source alternatives for every workplace function (email, drive, chat, calendar, video) but lacks the orchestration layer that turns them from isolated silos into an integrated platform experience.

The standard defines how independent open source services can be packaged, navigated, and interconnected to deliver a "Smart Platform Experience" — unified navigation, cross-app workflows, shared intelligence, and platform-level security.

The Alliance is the collective movement (publishers, institutions, governments) that governs and promotes the standard.

The site is the primary recruitment tool for the Alliance and the public face of the standard.

## Constraints

- **Tech stack**: Single HTML file, Tailwind CSS via CDN, Google Fonts (Syne + IBM Plex Sans + IBM Plex Mono), Lucide icons or inline SVG, vanilla JS for animations
- **Performance**: Load time < 2s on 4G, Lighthouse > 90
- **Privacy**: No cookies, no tracking, RGPD by design
- **Deployment**: Must work as a static file drop (no build step required)
- **Language**: English (v1), structure anticipates i18n
- **Design**: Dark mode dominant, design system defined in PRD with specific color palette, typography scale, component specs

## Key Decisions

| Decision | Rationale | Outcome |
|----------|-----------|---------|
| Single HTML file (no build step) | Maximum deployability, simplest hosting | -- Pending |
| Canvas animation for hero (not CSS SVG) | More dynamic/impressive network effect, acceptable JS weight | -- Pending |
| Tailwind via CDN (not local) | No build step, rapid prototyping | -- Pending |
| English content (overriding PRD's French default) | Broader European audience reach | -- Pending |
| Text placeholders for partner logos | No logo files available yet, swap later | -- Pending |
| Newsletter form with placeholder action | No email service chosen yet, configurable later | -- Pending |

---
*Last updated: 2026-02-16 after initialization*
