# Pitfalls Research

**Domain:** Single-page static landing sites (dark mode, animation-heavy, no build step)
**Researched:** 2026-02-16
**Confidence:** HIGH

## Critical Pitfalls

### Pitfall 1: Tailwind CDN in Production

**What goes wrong:**
The full Tailwind CDN loads 300+ KB of unused CSS, causing Flash of Unstyled Content (FOUC), especially when dynamically adding elements. The CDN always points to the latest version, introducing potential breaking changes without warning.

**Why it happens:**
CDN seems like the easiest path for "no build step" requirement. Developers assume CDN performance is acceptable because it works fine in development.

**How to avoid:**
- Use Tailwind's Play CDN for prototyping only
- For production: Extract used classes and inline critical CSS in `<style>` tag
- Alternative: Use Tailwind CLI to generate purged CSS file (36 KB typical vs. 300+ KB full)
- Monitor: Page weight should be < 100 KB total for 2s load on 4G

**Warning signs:**
- Lighthouse Performance Score < 90
- "Reduce unused CSS" warning in Lighthouse
- Visible FOUC on page load (white flash before dark mode applies)
- Page weight > 150 KB

**Phase to address:**
Phase 1 (Foundation) - CSS strategy must be decided upfront. Cannot defer without performance regression.

---

### Pitfall 2: Dark Mode FOUC (Flash of Unstyled Content)

**What goes wrong:**
Page loads in light mode (white background), then flashes to dark mode after JavaScript executes. This creates jarring visual experience and appears unprofessional to European decision-makers expecting polish.

**Why it happens:**
Dark mode class is applied via JavaScript after DOM loads. Browser renders default (light) styles first, then JavaScript adds `.dark` class to `<html>`, causing re-render.

**How to avoid:**
Inject blocking inline script in `<head>` BEFORE any CSS:
```html
<script>
  // Runs before page renders
  const darkMode = localStorage.getItem('theme') === 'dark' ||
                   (!localStorage.getItem('theme') &&
                    window.matchMedia('(prefers-color-scheme: dark)').matches);
  if (darkMode) document.documentElement.classList.add('dark');
</script>
```

Critical: This MUST be inline and blocking. External scripts or defer/async will cause FOUC.

**Warning signs:**
- White flash on page load before dark colors appear
- Layout shift (CLS > 0.1) in Lighthouse
- Users report "flickering" or "flashing" on load
- Different appearance on refresh vs. initial load

**Phase to address:**
Phase 1 (Foundation) - Implement before any visual styling. Cannot fix after animations are built without full rework.

---

### Pitfall 3: Canvas Animation Performance on Mobile

**What goes wrong:**
Canvas animations that perform smoothly on desktop (60 FPS) drop to 15-20 FPS on mobile devices, drain battery within minutes, and cause device heating. Hero animation becomes unusable on target 4G connections.

**Why it happens:**
- Using `setInterval()` instead of `requestAnimationFrame()`
- Redrawing entire canvas every frame instead of dirty regions
- Using floating-point coordinates (forces anti-aliasing calculations)
- No mobile detection or reduced motion support
- Continuous animation even when page is backgrounded

**How to avoid:**
1. **Always use requestAnimationFrame:**
```javascript
function animate() {
  // Animation code
  requestAnimationFrame(animate);
}
requestAnimationFrame(animate);
```

2. **Pre-render static elements to offscreen canvas:**
```javascript
const offscreen = document.createElement('canvas');
// Draw static parts once to offscreen
// In animation loop: ctx.drawImage(offscreen, 0, 0);
```

3. **Use integer coordinates:**
```javascript
ctx.drawImage(img, Math.floor(x), Math.floor(y));
```

4. **Pause when not visible:**
```javascript
document.addEventListener('visibilitychange', () => {
  if (document.hidden) cancelAnimationFrame(animationId);
  else animate();
});
```

5. **Respect prefers-reduced-motion:**
```javascript
const prefersReducedMotion = window.matchMedia('(prefers-reduced-motion: reduce)').matches;
if (prefersReducedMotion) return; // Skip animations
```

6. **Mobile detection and simplification:**
Reduce particle count, animation complexity, or use static image on mobile.

**Warning signs:**
- FPS drops below 30 in Chrome DevTools Performance tab
- Battery drain > 5% per minute
- Device heating during animation
- Lighthouse Performance Score < 85
- "Avoid excessive DOM size" warning
- Total Blocking Time (TBT) > 300ms

**Phase to address:**
Phase 2 (Hero Section) - Canvas optimization must happen during initial implementation. Retrofitting performance is 3x harder than building it right.

---

### Pitfall 4: Scroll-Triggered Animation Memory Leaks

**What goes wrong:**
Intersection Observer instances accumulate without cleanup, causing memory to grow from 50 MB to 500+ MB over multiple page visits. Browser becomes sluggish, eventual crash on mobile devices.

**Why it happens:**
Creating new Intersection Observer for each element without calling `unobserve()` or `disconnect()`. Single-page apps particularly vulnerable since page never fully unloads.

**How to avoid:**
1. **Use single shared observer for all elements:**
```javascript
const observer = new IntersectionObserver(handleIntersection, options);
document.querySelectorAll('.animate-on-scroll').forEach(el => {
  observer.observe(el);
});
```

2. **Cleanup after animation completes:**
```javascript
function handleIntersection(entries) {
  entries.forEach(entry => {
    if (entry.isIntersecting) {
      entry.target.classList.add('animated');
      observer.unobserve(entry.target); // Critical: cleanup
    }
  });
}
```

3. **Disconnect on page unload (if SPA):**
```javascript
window.addEventListener('beforeunload', () => {
  observer.disconnect();
});
```

**Warning signs:**
- Chrome DevTools Memory Profiler shows growing heap
- Browser becomes slower with each scroll
- Mobile Safari crashes after 30s of scrolling
- Multiple observer instances in DevTools

**Phase to address:**
Phase 3 (Content Sections) - Implement cleanup pattern when first adding scroll animations. Critical for European users on lower-end mobile devices.

---

### Pitfall 5: Image Optimization Failure

**What goes wrong:**
Landing page loads 5 MB hero image, 2 MB screenshots, pushing total page weight to 10+ MB. Load time on 4G: 15-20 seconds. Lighthouse score: 40. Bounce rate: 80%.

**Why it happens:**
Using original exports from design tools (PNG, uncompressed) without optimization. Assuming "it's just one image" won't matter. Not considering mobile viewport sizes.

**How to avoid:**
1. **Hero image:** WebP format, < 100 KB, 1920px max width
2. **Screenshots/content:** WebP format, < 50 KB each
3. **Icons/logos:** Inline SVG (not raster)
4. **Responsive images:**
```html
<picture>
  <source srcset="hero-mobile.webp" media="(max-width: 768px)">
  <source srcset="hero-desktop.webp" media="(min-width: 769px)">
  <img src="hero-desktop.webp" alt="..." loading="lazy">
</picture>
```

5. **Critical images:** Preload in `<head>`
```html
<link rel="preload" as="image" href="hero.webp">
```

**Warning signs:**
- Total page weight > 500 KB (check Network tab)
- Largest Contentful Paint (LCP) > 2.5s
- "Properly size images" warning in Lighthouse
- "Serve images in next-gen formats" warning
- 4G throttling shows > 3s load time

**Phase to address:**
Phase 1 (Foundation) - Image optimization strategy must be defined upfront. Changing image formats mid-project requires re-exporting all assets.

---

### Pitfall 6: Inline Everything Anti-Pattern

**What goes wrong:**
Attempting "single HTML file" literally: 10,000 lines of inline CSS, inline JavaScript, base64-encoded images. File size: 800 KB. Initial parse time: 4 seconds. Lighthouse score: 35.

**Why it happens:**
Misinterpreting "no build step" as "everything must be inline." Fear of HTTP requests.

**How to avoid:**
Single HTML file for *markup* is fine. External resources are optimal:

**Good architecture:**
```
index.html (15 KB)
  → styles.css (30 KB, cached)
  → script.js (20 KB, cached)
  → hero.webp (80 KB, cached)
```

**Bad architecture:**
```
index.html (800 KB)
  → Everything inline, nothing cached
  → Re-downloaded on every visit
```

**Rule of thumb:**
- Critical CSS: Inline < 14 KB for above-the-fold
- Non-critical CSS: External file
- Critical JS: Inline if < 5 KB (dark mode script)
- Animation JS: External file with `defer`
- Images: NEVER base64 inline (except tiny SVG icons)

**Warning signs:**
- HTML file > 100 KB
- No browser caching benefits on repeat visits
- "Reduce JavaScript execution time" > 2s
- "Minimize main thread work" > 5s
- DevTools Coverage shows 90%+ unused code on load

**Phase to address:**
Phase 1 (Foundation) - Architecture decision. Changing from all-inline to external files requires restructuring.

---

### Pitfall 7: Animation Jank from Layout Thrashing

**What goes wrong:**
Smooth 60 FPS animation in Firefox desktop becomes 15 FPS stuttering mess in Chrome mobile. Animations feel "janky" and unprofessional. European decision-makers notice and question product quality.

**Why it happens:**
Triggering layout recalculations during animation by reading layout properties (offsetHeight, getBoundingClientRect) then writing styles in a loop:

```javascript
// BAD: Forces layout recalc 60 times per second
elements.forEach(el => {
  const height = el.offsetHeight; // Read (triggers layout)
  el.style.height = height + 10 + 'px'; // Write (invalidates layout)
});
```

**How to avoid:**
1. **Batch reads, then writes:**
```javascript
// GOOD: Read all, then write all
const heights = elements.map(el => el.offsetHeight);
elements.forEach((el, i) => {
  el.style.height = heights[i] + 10 + 'px';
});
```

2. **Use CSS transforms (GPU-accelerated):**
```javascript
// BAD: Changes layout
el.style.top = y + 'px';

// GOOD: GPU-accelerated, no layout
el.style.transform = `translateY(${y}px)`;
```

3. **Animate only transform and opacity:**
These properties don't trigger layout or paint, only composite.

4. **Use `will-change` sparingly:**
```css
.animate-on-scroll {
  will-change: transform, opacity; /* Pre-optimize for animation */
}
```
Warning: Don't use on > 10 elements or you'll exhaust GPU memory.

**Warning signs:**
- Chrome DevTools Performance shows red bars (long tasks)
- Frame rate < 60 FPS during animation
- "Avoid large layout shifts" warning
- Cumulative Layout Shift (CLS) > 0.1
- Visible stuttering on scroll
- Animation feels "heavy" or laggy

**Phase to address:**
Phase 3 (Content Sections) - When implementing scroll animations. Must get right first time or causes complete animation rework.

---

### Pitfall 8: Mobile Touch Target Violations

**What goes wrong:**
CTA buttons work perfectly on desktop but are frustrating on mobile. Users miss clicks, accidentally click wrong elements, abandon conversion. Lighthouse Accessibility score: 65.

**Why it happens:**
Designing for mouse precision (1px cursor) instead of finger touches (44+ px target). Desktop-first design shrunk to mobile.

**How to avoid:**
1. **Minimum touch targets: 48x48 px**
```css
.cta-button {
  min-height: 48px;
  min-width: 48px;
  padding: 12px 24px; /* Exceeds minimum */
}
```

2. **Spacing between interactive elements: ≥ 8px**
```css
.button-group button {
  margin: 8px;
}
```

3. **Mobile-first breakpoints in Tailwind:**
```html
<!-- Mobile (default): large touch targets -->
<button class="py-4 px-6 text-lg">
  <!-- Desktop: can be smaller -->
  <span class="md:py-2 md:px-4 md:text-base">Contact Us</span>
</button>
```

**Warning signs:**
- Lighthouse Accessibility score < 85
- "Touch targets not sized appropriately" warning
- High mobile bounce rate but normal desktop
- Users reporting "hard to click on phone"
- Click/touch events not registering reliably

**Phase to address:**
Phase 4 (Mobile Optimization) - Dedicated phase for mobile testing. Must test on real devices, not just DevTools emulation.

---

### Pitfall 9: GDPR Cookie Banner Performance Killer

**What goes wrong:**
Cookie consent banner loads 500 KB third-party script (CookieBot, OneTrust), blocks rendering, adds 2 seconds to load time. Lighthouse score drops from 95 to 65. European users required to use this experience.

**Why it happens:**
Copy-pasting vendor-recommended implementation without understanding performance impact. Vendor scripts load tracking pixels, analytics, complex UI frameworks.

**How to avoid:**
1. **For landing page (no cookies used): No banner needed**
If you're not setting cookies, GDPR doesn't require banner. Simple privacy policy link suffices.

2. **If analytics needed: Respect DNT, defer consent:**
```html
<!-- Load analytics AFTER user consent, not before -->
<script>
  function loadAnalytics() {
    // Load only after user clicks "Accept"
  }
</script>
```

3. **Lightweight custom banner:**
```html
<!-- 2 KB custom banner vs. 500 KB vendor script -->
<div id="cookie-banner" class="fixed bottom-0 inset-x-0 p-4 bg-gray-900">
  <p>We use cookies. <a href="/privacy">Learn more</a></p>
  <button onclick="acceptCookies()">Accept</button>
</div>
```

4. **Never use Google Tag Manager on landing page:**
GTM loads 200+ KB for simple analytics. Use direct analytics script or defer entirely.

**Warning signs:**
- Third-party script > 100 KB
- "Reduce third-party code" warning in Lighthouse
- Blocking scripts in Network waterfall
- TTI (Time to Interactive) > 3.5s
- Cookie banner appears before hero content

**Phase to address:**
Phase 5 (Analytics & Legal) - Near end of project. DO NOT implement early. Confirm actual cookie usage first.

---

### Pitfall 10: Ignoring Mobile Data Costs

**What goes wrong:**
Landing page optimized for "4G" but only tested on unlimited desktop WiFi. Real European 4G users on metered connections (10€ for 5 GB) spend 0.10€ just to load landing page due to unnecessary assets. Users resent this and bounce.

**Why it happens:**
Testing on fast connections. Not considering data cost constraints in European markets where unlimited data is less common than USA.

**How to avoid:**
1. **Total page weight budget: < 500 KB**
   - HTML: 15 KB
   - Critical CSS (inline): 10 KB
   - External CSS: 30 KB
   - JavaScript: 50 KB
   - Images (all): 300 KB
   - Fonts: 50 KB
   - **Total: 455 KB**

2. **Test on real 4G throttling:**
```
Chrome DevTools → Network → Throttling → "Fast 4G"
- Download: 4 Mbps
- Upload: 1 Mbps
- Latency: 20ms
```

Target: Page fully loaded in < 2 seconds on Fast 4G.

3. **Lazy load below-the-fold images:**
```html
<img src="feature.webp" loading="lazy" alt="...">
```

4. **Font optimization:**
- Use system fonts: -apple-system, BlinkMacSystemFont, "Segoe UI"
- Or load 1-2 weights maximum
- Use `font-display: swap` to prevent invisible text

**Warning signs:**
- Total network transfer > 1 MB
- Lighthouse "Avoid enormous network payloads" warning
- > 50 network requests
- Load time > 2s on "Fast 4G" throttling
- Fonts blocking render for > 500ms

**Phase to address:**
Phase 4 (Mobile Optimization) - Test with real throttling. Optimize before launch, not after complaints.

---

## Technical Debt Patterns

Shortcuts that seem reasonable but create long-term problems.

| Shortcut | Immediate Benefit | Long-term Cost | When Acceptable |
|----------|-------------------|----------------|-----------------|
| Using Tailwind CDN | Zero config setup | 300 KB unused CSS, FOUC, breaking changes | Development/prototyping only, never production |
| Skipping WebP conversion | Faster asset export | 5-10x larger images, slow loads | Never for landing pages |
| Using setInterval for animation | Simpler code | Janky animations, battery drain, poor mobile performance | Never, requestAnimationFrame always better |
| Inline base64 images | "One file" simplicity | Huge HTML, no caching, slow parse | Only for <2 KB SVG icons |
| Omitting prefers-reduced-motion | Fewer code branches | Accessibility violation, motion sickness | Never, required for a11y |
| Loading all fonts upfront | Consistent typography | 200+ KB, 1s+ render blocking | Only if using system fonts |
| No mobile testing until end | Faster development | Complete mobile rework needed | Never for mobile-first project |
| Copy-paste vendor cookie scripts | "Compliant" quickly | 500 KB performance hit | Only if legal requires specific vendor |

## Integration Gotchas

Common mistakes when connecting to external services.

| Integration | Common Mistake | Correct Approach |
|-------------|----------------|------------------|
| Google Analytics | Loading gtag.js in `<head>` (blocking) | Load with `defer` or after user interaction |
| Font providers (Google Fonts) | Loading 6+ font weights | Load 2 weights max (regular + bold), use font-display: swap |
| Video embeds (YouTube) | Iframe embed in hero | Use poster image, load iframe on click (saves 500+ KB) |
| Social share buttons | Loading Facebook/Twitter SDKs | Use simple links, no SDK (saves 300+ KB) |
| Form services (Mailchimp) | Embedded form with full CSS | Use direct API call, custom styled form |
| Illustration libraries (Lottie) | Loading 2 MB animation files | Use CSS animations or optimized WebP sequences |

## Performance Traps

Patterns that work at small scale but fail as usage grows.

| Trap | Symptoms | Prevention | When It Breaks |
|------|----------|------------|----------------|
| Animating all scroll elements simultaneously | Smooth with 5 elements | Limit to 10-15 observers max, use single shared observer | > 20 animated elements |
| Canvas particle count | 100 particles = 60 FPS | Reduce to 30-50 on mobile, use `performance.now()` adaptive throttling | > 100 particles on mobile |
| Loading full-resolution images | Works on desktop 4K | Responsive images with srcset, WebP format | Mobile 4G users |
| Synchronous JavaScript in main thread | Fast on desktop | Use requestIdleCallback for non-critical work | JavaScript > 50 KB |
| Too many Google Fonts weights | Designer prefers exact weights | Use variable fonts or limit to 2 weights | > 100 KB total fonts |
| Intersection Observer threshold: 1.0 | Element must be fully visible | Use threshold: 0.1 (10% visible) for better UX | Long content sections |

## Security Mistakes

Domain-specific security issues beyond general web security.

| Mistake | Risk | Prevention |
|---------|------|------------|
| Loading Tailwind CDN from unpinned latest | Breaking changes injected, supply chain attack | Pin specific version or self-host purged CSS |
| Allowing arbitrary canvas input | XSS via canvas text injection | Sanitize all text before ctx.fillText() |
| No Content Security Policy | XSS from injected scripts | Add CSP meta tag restricting script sources |
| Loading analytics without user consent | GDPR violation (€20M fine) | Gate all tracking behind explicit consent |
| HTTP links in HTTPS page | Mixed content warning, blocked resources | Use HTTPS for all external resources |
| Inline event handlers (onclick=) | XSS vulnerability | Use addEventListener() instead |

## UX Pitfalls

Common user experience mistakes in this domain.

| Pitfall | User Impact | Better Approach |
|---------|-------------|-----------------|
| Auto-playing canvas animation without pause control | Motion sickness, battery drain annoyance | Add pause/play button, respect prefers-reduced-motion |
| Dark mode without light mode toggle | Alienates 30-40% of users who prefer light | Offer toggle, default to system preference |
| Animations blocking CTA button | Cannot click "Sign up" until animation completes | Ensure CTA is immediately interactive |
| Mobile viewport too zoomed out | Text too small, requires pinch-zoom | Set viewport meta: width=device-width, initial-scale=1 |
| Navigation menu on landing page | Provides exits, reduces conversion 10-20% | Remove navigation, single CTA focus |
| Too many scroll-triggered animations | Overwhelming, feels gimmicky | Animate 3-5 key sections max |
| Hover-only interactions | Broken on mobile (no hover state) | Use click/tap, or show info by default on mobile |
| Auto-playing audio | Infuriating, immediate bounce | Never auto-play audio/video with sound |

## "Looks Done But Isn't" Checklist

Things that appear complete but are missing critical pieces.

- [ ] **Dark mode:** Works on initial load but breaks on refresh without inline blocking script
- [ ] **Canvas animation:** Runs smoothly on desktop but not tested on actual mobile device (emulator insufficient)
- [ ] **Scroll animations:** Intersection Observers created but never cleaned up with unobserve()
- [ ] **Images:** Optimized for desktop but no responsive srcset for mobile
- [ ] **Performance:** Lighthouse 95 on desktop but not tested on 4G throttling
- [ ] **Touch targets:** All buttons look good but < 44px on mobile viewport
- [ ] **Fonts:** Beautiful typography but fonts not loading with font-display: swap (invisible text)
- [ ] **Analytics:** Implemented but loads before cookie consent (GDPR violation)
- [ ] **Accessibility:** Looks fine but missing alt text, ARIA labels, keyboard navigation
- [ ] **Error states:** Form works with valid input but no error handling
- [ ] **Loading states:** Fast on WiFi but no loading indicator for 4G users
- [ ] **SEO:** Page works but missing meta description, Open Graph tags, structured data

## Recovery Strategies

When pitfalls occur despite prevention, how to recover.

| Pitfall | Recovery Cost | Recovery Steps |
|---------|---------------|----------------|
| Tailwind CDN in production | LOW | 1. Generate purged CSS with Tailwind CLI (1 hour)<br>2. Replace CDN link with inline critical CSS + external file<br>3. Test for regressions |
| Dark mode FOUC | LOW | 1. Add inline blocking script in `<head>` (30 min)<br>2. Test localStorage persistence<br>3. Verify no white flash |
| Canvas performance issues | MEDIUM | 1. Switch setInterval → requestAnimationFrame (1 hour)<br>2. Implement offscreen canvas pre-rendering (2 hours)<br>3. Add mobile detection for reduced complexity (1 hour)<br>4. Profile with Chrome DevTools, optimize bottlenecks (2-4 hours) |
| Intersection Observer leaks | LOW | 1. Refactor to single shared observer (1 hour)<br>2. Add unobserve() calls after animation (30 min)<br>3. Test memory with DevTools Profiler |
| Unoptimized images | MEDIUM | 1. Export all images to WebP format (2 hours)<br>2. Generate responsive sizes (1 hour)<br>3. Update HTML with picture/srcset (1 hour)<br>4. Verify Lighthouse score improvement |
| Inline everything anti-pattern | HIGH | 1. Extract CSS to external file (2 hours)<br>2. Extract JavaScript to external file (2 hours)<br>3. Configure caching headers (1 hour)<br>4. Test performance before/after (1 hour)<br>5. Potential rework of build process |
| Animation jank from layout thrashing | MEDIUM | 1. Identify forced reflows in DevTools Performance (1 hour)<br>2. Refactor to batch reads/writes (2-3 hours)<br>3. Switch to transform/opacity animations (2-4 hours)<br>4. May require complete animation rework if deeply embedded |
| Mobile touch target violations | LOW | 1. Update all interactive elements to min 48x48px (1 hour)<br>2. Add spacing between elements (30 min)<br>3. Test on real devices |
| GDPR cookie banner bloat | MEDIUM | 1. Remove vendor script (5 min)<br>2. Build custom lightweight banner (2 hours)<br>3. Implement consent logic (1 hour)<br>4. Legal review (external dependency) |
| Ignoring mobile data costs | MEDIUM | 1. Audit all assets with Coverage tool (30 min)<br>2. Optimize images, fonts, scripts (3-4 hours)<br>3. Implement lazy loading (1 hour)<br>4. Test on 4G throttling |

## Pitfall-to-Phase Mapping

How roadmap phases should address these pitfalls.

| Pitfall | Prevention Phase | Verification |
|---------|------------------|--------------|
| Tailwind CDN in production | Phase 1: Foundation | Final CSS file < 50 KB, no CDN link in production HTML |
| Dark mode FOUC | Phase 1: Foundation | No white flash on page load, localStorage persists preference |
| Canvas animation performance | Phase 2: Hero Section | 60 FPS on desktop, 30+ FPS on mobile, < 5% battery drain/min |
| Intersection Observer leaks | Phase 3: Content Sections | Memory stable after 10 scroll cycles (Chrome DevTools Memory) |
| Image optimization failure | Phase 1: Foundation | Total images < 300 KB, all WebP format, responsive srcset |
| Inline everything anti-pattern | Phase 1: Foundation | HTML < 30 KB, external cached resources, repeat visit < 0.5s |
| Animation jank from layout thrashing | Phase 3: Content Sections | No red bars in Performance tab, CLS < 0.1, 60 FPS scroll |
| Mobile touch target violations | Phase 4: Mobile Optimization | All interactive elements ≥ 48x48px, Lighthouse a11y > 90 |
| GDPR cookie banner bloat | Phase 5: Analytics & Legal | Cookie banner < 5 KB if needed, defer analytics until consent |
| Ignoring mobile data costs | Phase 4: Mobile Optimization | Total page < 500 KB, < 2s load on "Fast 4G" throttling |

## Sources

### High Confidence (Official Documentation)
- [MDN: Optimizing Canvas Performance](https://developer.mozilla.org/en-US/docs/Web/API/Canvas_API/Tutorial/Optimizing_canvas)
- [MDN: Intersection Observer API](https://developer.mozilla.org/en-US/docs/Web/API/Intersection_Observer_API)
- [MDN: requestAnimationFrame](https://developer.mozilla.org/en-US/docs/Web/API/Window/requestAnimationFrame)
- [Chrome DevTools: Lighthouse Performance Scoring](https://developer.chrome.com/docs/lighthouse/performance/performance-scoring)
- [Tailwind CSS: Optimizing for Production](https://v3.tailwindcss.com/docs/optimizing-for-production)

### Medium Confidence (Community Best Practices, 2026)
- [Landing Page Mistakes (Moosend)](https://moosend.com/blog/landing-page-mistakes/)
- [Dark Mode Best Practices 2026](https://www.tech-rz.com/blog/dark-mode-design-best-practices-in-2026/)
- [requestAnimationFrame vs setInterval Performance](https://blog.webdevsimplified.com/2021-12/request-animation-frame/)
- [Tailwind CDN Production Discussion (GitHub)](https://github.com/tailwindlabs/tailwindcss/discussions/7637)
- [Fixing Dark Mode FOUC](https://notanumber.in/blog/fixing-react-dark-mode-flickering)
- [Mobile-First Design 2026](https://seizemarketingagency.com/mobile-first-design/)
- [GDPR Cookie Consent 2025 Requirements](https://secureprivacy.ai/blog/gdpr-cookie-consent-requirements-2025)
- [Lighthouse Score Optimization Guide](https://www.checklyhq.com/blog/how-we-improved-the-lighthouse-score-of-our-landing-page-to-96/)
- [Intersection Observer for Animations](https://www.itzami.com/blog/boost-your-css-animations-with-intersection-observer-api)

### Medium Confidence (Performance Research)
- [Canvas Animation Performance Tips (GitHub Gist)](https://gist.github.com/jaredwilli/5469626)
- [Mobile Animation Battery Impact](https://www.linkedin.com/advice/0/how-do-you-balance-animation-battery-use-your-mobile)
- [Responsive Design Common Mistakes](https://www.wordtracker.com/blog/marketing/5-responsive-mobile-design-mistakes-even-smart-developers-make)
- [Page Load Time Optimization](https://crystallize.com/blog/page-load-time)

---
*Pitfalls research for: Open Buro Landing Page (single HTML, dark mode, animation-heavy)*
*Researched: 2026-02-16*
*Confidence: HIGH for technical pitfalls (based on official MDN, Chrome DevTools docs), MEDIUM for UX/business impact (based on community sources)*
