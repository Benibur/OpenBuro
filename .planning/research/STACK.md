# Technology Stack

**Project:** Open Buro Landing Page
**Domain:** Single-page static landing site
**Researched:** 2026-02-16
**Overall Confidence:** HIGH

## Recommended Stack

### Core Technologies

| Technology | Version | Purpose | Why Recommended | Confidence |
|------------|---------|---------|-----------------|------------|
| **Tailwind CSS** | v4.1+ (Play CDN) | Utility-first CSS framework | Modern, minimal, supports dark mode via `prefers-color-scheme`, OKLCH colors by default, CSS variables for theming. v4 is the current production version as of 2025-2026. | HIGH |
| **Vanilla JavaScript** | ES2020+ | Animation logic, Intersection Observer | Native browser APIs are mature, performant, and require no dependencies. Intersection Observer has 95%+ browser support. | HIGH |
| **HTML5 Canvas** | Native | Hero constellation/network animation | Standard for particle systems and complex visual effects. Hardware-accelerated, excellent performance when using requestAnimationFrame properly. | HIGH |
| **Google Fonts** | N/A | Typography (Syne + IBM Plex Sans) | Use CDN for simplicity. While self-hosting offers marginally better performance for sites with good CDN/HTTP2, Google Fonts CDN is reliable, automatically serves optimized variants per browser, and simplifies implementation for a single-page site. | MEDIUM |

### Supporting Libraries

| Library | Version | Purpose | When to Use | Confidence |
|---------|---------|---------|-------------|------------|
| **Sal.js** | 0.8.5 | Scroll animations | Optional: provides data-attribute-driven scroll animations via Intersection Observer. Alternative: implement custom Intersection Observer (42 lines of code for basic implementation). Use Sal.js for rapid prototyping; custom implementation for maximum control. | MEDIUM |
| **requestAnimationFrame** | Native API | Canvas animation timing | REQUIRED for all canvas animations. Synchronizes with browser refresh rate (60-144hz), auto-pauses on inactive tabs for battery savings. Replaces setInterval/setTimeout entirely. | HIGH |

### Development Tools

| Tool | Purpose | Notes | Confidence |
|------|---------|-------|------------|
| **HTML Minifier** | Minify HTML, inline CSS/JS | Use kangax/html-minifier or similar before deployment. Critical for Lighthouse Performance score. Can reduce file size 20-40%. | HIGH |
| **Lighthouse** | Performance auditing | Target: 90+ across all metrics. Focus on LCP < 2.5s and TBT optimization. For static sites, 98-100 scores are achievable. | HIGH |
| **Browser DevTools** | Canvas performance profiling | Use Performance tab to identify canvas rendering bottlenecks. Monitor frame rate with FPS meter. | HIGH |

## CDN URLs (Copy-Paste Ready)

### Tailwind CSS v4 Play CDN
```html
<script src="https://cdn.jsdelivr.net/npm/@tailwindcss/browser@4"></script>
```

**Warning:** Development only. Not intended for production. For a single-file landing page with no build step, this is acceptable if you understand the tradeoff (runtime CSS processing vs. pre-compiled).

**Configuration (optional):**
```html
<style type="text/tailwindcss">
  @theme {
    --color-primary: #your-color;
  }
</style>
```

### Google Fonts
```html
<link rel="preconnect" href="https://fonts.googleapis.com">
<link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
<link href="https://fonts.googleapis.com/css2?family=IBM+Plex+Sans:wght@400;500;600&family=Syne:wght@400;600;700&display=swap" rel="stylesheet">
```

**Performance optimization:** `display=swap` prevents invisible text flash (FOIT), `preconnect` reduces DNS/TLS handshake latency.

### Sal.js (Optional)
```html
<!-- CSS -->
<link rel="stylesheet" href="https://cdn.jsdelivr.net/npm/sal.js@0.8.5/dist/sal.css">

<!-- JavaScript -->
<script src="https://cdn.jsdelivr.net/npm/sal.js@0.8.5/dist/sal.min.js"></script>
<script>
  sal();
</script>
```

**Alternative:** Custom Intersection Observer (42 lines) for maximum control and zero dependencies.

## Installation

**No installation required.** This is a zero-build-step stack. All resources loaded via CDN.

### Optional: Local Development
```bash
# For minification before deployment
npm install -D html-minifier-terser

# Minify command
npx html-minifier-terser --collapse-whitespace --minify-css --minify-js index.html -o index.min.html
```

## Alternatives Considered

| Category | Recommended | Alternative | Why Not | Confidence |
|----------|-------------|-------------|---------|------------|
| **CSS Framework** | Tailwind v4 CDN | Bootstrap, Bulma, or custom CSS | Bootstrap is heavier (requires more JS), custom CSS lacks utility patterns for rapid development. Tailwind v4 uses CSS variables natively for theming. | HIGH |
| **Scroll Animations** | Vanilla Intersection Observer | AOS, ScrollMagic, GSAP ScrollTrigger | AOS is unmaintained, ScrollMagic is complex overkill for simple fade/slide effects, GSAP adds 50kb+. Intersection Observer is native, performant, and well-supported. | HIGH |
| **Canvas Animation** | Vanilla JS + requestAnimationFrame | Three.js, PixiJS, Particles.js | Three.js/PixiJS are 3D libraries (overkill for 2D particles). Particles.js is unmaintained. Custom vanilla implementation is <100 lines for constellation effect. | HIGH |
| **Font Loading** | Google Fonts CDN | Self-hosted fonts | For single-page sites without existing CDN infrastructure, self-hosting adds complexity with marginal performance gains. Google Fonts serves 30+ optimized variants per font automatically. | MEDIUM |
| **Dark Mode** | CSS `prefers-color-scheme` + `color-scheme` property | JavaScript toggle with localStorage | For a marketing landing page, respecting system preference is cleaner. Add manual toggle only if user override is explicitly required. | HIGH |

## What NOT to Use

| Avoid | Why | Use Instead | Confidence |
|-------|-----|-------------|------------|
| **Tailwind v3 CDN** | Outdated. v4 released early 2025 with CSS variables, OKLCH colors, improved performance. | Tailwind v4 Play CDN | HIGH |
| **jQuery** | Unnecessary in 2026. Vanilla JS APIs (querySelector, fetch, Intersection Observer) cover all use cases. Adds 30kb+ for no benefit. | Vanilla JavaScript | HIGH |
| **setInterval/setTimeout for animations** | Causes jank, doesn't sync with display refresh rate, wastes battery on inactive tabs. | requestAnimationFrame | HIGH |
| **window.onscroll for animations** | Fires constantly, causes layout thrashing, blocks main thread. Major performance killer. | Intersection Observer API | HIGH |
| **Auto-inverting colors for dark mode** | Creates visual inconsistencies. Images/logos look wrong when inverted. | Design dark surfaces intentionally using CSS variables and `light-dark()` function | HIGH |
| **Excessive canvas layers** | Each canvas is a bitmap. Too many = memory bloat. | Single canvas for hero animation, CSS for UI elements | MEDIUM |

## Stack Patterns by Use Case

### Pattern 1: Maximum Performance (Lighthouse 98-100)
```
- Inline critical CSS (above-the-fold only)
- Defer Google Fonts with font-display: swap
- Custom Intersection Observer (no Sal.js)
- Single minified HTML file
- Canvas animation only when in viewport
- No external dependencies beyond fonts
```

**When:** Production site where performance is critical.

### Pattern 2: Rapid Prototyping (Current approach)
```
- Tailwind v4 Play CDN
- Google Fonts CDN
- Sal.js for animations
- Unminified for easy editing
- Canvas always animating (for visual development)
```

**When:** Initial development, design iteration, stakeholder demos.

### Pattern 3: Hybrid (Recommended for launch)
```
- Tailwind v4 Play CDN (acceptable for single-page with disclaimer)
- Google Fonts CDN with preconnect
- Custom Intersection Observer (replace Sal.js)
- Minified HTML with inline critical CSS
- Canvas animation pauses when offscreen
- Manual dark mode toggle + system preference
```

**When:** Production-ready but maintaining zero-build-step constraint.

## Version Compatibility

| Technology | Compatible With | Notes |
|-----------|-----------------|-------|
| Tailwind v4 Play CDN | Modern browsers (ES2020+) | No IE11 support. Chrome 90+, Firefox 88+, Safari 14+. |
| Intersection Observer | 95%+ browser support | Polyfill available for older browsers if needed. |
| HTML5 Canvas | Universal | Supported since IE9, all modern browsers. |
| requestAnimationFrame | Universal | Supported since IE10, all modern browsers. |
| CSS `prefers-color-scheme` | 96%+ browser support | Chrome 76+, Firefox 67+, Safari 12.1+. |
| Google Fonts | Universal | Works with all browsers. |

## Performance Optimization Checklist

Based on 2026 best practices for achieving Lighthouse 90+ scores:

### Critical (Required for 90+ score)
- [ ] **LCP < 2.5s**: Optimize hero section rendering. Inline critical CSS. Preload hero fonts.
- [ ] **TBT < 300ms**: Minimize JavaScript execution. Defer non-critical scripts.
- [ ] **Minify HTML/CSS/JS**: Use html-minifier-terser for 20-40% file size reduction.
- [ ] **Optimize canvas**: Use requestAnimationFrame, limit redraw areas, pause when offscreen.
- [ ] **Font optimization**: Use `font-display: swap`, preconnect to fonts.googleapis.com.

### Important (Boosts score 5-10 points)
- [ ] **Intersection Observer for lazy effects**: Don't animate offscreen elements.
- [ ] **Canvas layering**: If animation gets complex, split static/dynamic layers.
- [ ] **Responsive images**: Serve appropriate sizes for mobile/desktop.
- [ ] **Minimize DOM**: Lighthouse penalizes excessive DOM nodes (keep < 1500).
- [ ] **Semantic HTML**: Use proper heading hierarchy (single H1, logical H2-H6).

### Nice-to-Have (Marginal gains)
- [ ] **Offscreen canvas rendering**: For repeated drawing operations.
- [ ] **Resource hints**: `dns-prefetch`, `preconnect` for external resources.
- [ ] **Defer Google Fonts**: Load fonts after main content renders.

## Dark Mode Implementation

### Recommended Approach (2026 Best Practice)
```html
<head>
  <meta name="color-scheme" content="light dark">
  <style type="text/tailwindcss">
    @theme {
      /* Define light/dark color pairs */
      --color-bg-base: light-dark(#ffffff, #0a0a0a);
      --color-text-base: light-dark(#1a1a1a, #f5f5f5);
    }
  </style>
</head>
```

**Why:**
- `color-scheme` meta tag signals browser to use appropriate system UI (scrollbars, form controls)
- `light-dark()` CSS function (supported in Safari 17.5+, Chrome 124+, Firefox 128+) provides clean syntax
- `prefers-color-scheme` media query as fallback for older browsers
- No JavaScript needed unless providing manual toggle

**For manual toggle + localStorage override:**
```html
<script>
  // Check for saved preference or system preference
  const theme = localStorage.getItem('theme') ||
    (window.matchMedia('(prefers-color-scheme: dark)').matches ? 'dark' : 'light');
  document.documentElement.setAttribute('data-theme', theme);

  // Listen for system preference changes
  window.matchMedia('(prefers-color-scheme: dark)')
    .addEventListener('change', e => {
      if (!localStorage.getItem('theme')) {
        document.documentElement.setAttribute('data-theme', e.matches ? 'dark' : 'light');
      }
    });
</script>
```

## Canvas Animation Best Practices

### Constellation/Network Effect Implementation
```javascript
// Essential pattern for 60fps performance
class ParticleNetwork {
  constructor(canvas) {
    this.canvas = canvas;
    this.ctx = canvas.getContext('2d');
    this.particles = [];
    this.animationId = null;
    this.isVisible = true;

    this.init();
    this.setupIntersectionObserver();
  }

  init() {
    // Create particles
    for (let i = 0; i < 50; i++) {
      this.particles.push({
        x: Math.random() * this.canvas.width,
        y: Math.random() * this.canvas.height,
        vx: (Math.random() - 0.5) * 0.5,
        vy: (Math.random() - 0.5) * 0.5,
      });
    }
  }

  setupIntersectionObserver() {
    // Pause animation when canvas is offscreen
    const observer = new IntersectionObserver(entries => {
      this.isVisible = entries[0].isIntersecting;
      if (this.isVisible) {
        this.animate();
      } else {
        cancelAnimationFrame(this.animationId);
      }
    });
    observer.observe(this.canvas);
  }

  animate() {
    if (!this.isVisible) return;

    // Clear with alpha for trail effect (optional)
    this.ctx.fillStyle = 'rgba(10, 10, 10, 0.05)';
    this.ctx.fillRect(0, 0, this.canvas.width, this.canvas.height);

    // Update and draw particles
    this.particles.forEach(p => {
      p.x += p.vx;
      p.y += p.vy;

      // Bounce off edges
      if (p.x < 0 || p.x > this.canvas.width) p.vx *= -1;
      if (p.y < 0 || p.y > this.canvas.height) p.vy *= -1;

      // Draw particle
      this.ctx.fillStyle = '#4f46e5';
      this.ctx.beginPath();
      this.ctx.arc(p.x, p.y, 2, 0, Math.PI * 2);
      this.ctx.fill();
    });

    // Draw connections (distance threshold)
    this.particles.forEach((p1, i) => {
      this.particles.slice(i + 1).forEach(p2 => {
        const dx = p1.x - p2.x;
        const dy = p1.y - p2.y;
        const dist = Math.sqrt(dx * dx + dy * dy);

        if (dist < 150) {
          this.ctx.strokeStyle = `rgba(79, 70, 229, ${1 - dist / 150})`;
          this.ctx.lineWidth = 1;
          this.ctx.beginPath();
          this.ctx.moveTo(p1.x, p1.y);
          this.ctx.lineTo(p2.x, p2.y);
          this.ctx.stroke();
        }
      });
    });

    this.animationId = requestAnimationFrame(() => this.animate());
  }
}

// Initialize
const canvas = document.getElementById('hero-canvas');
canvas.width = window.innerWidth;
canvas.height = window.innerHeight;
new ParticleNetwork(canvas);

// Handle resize with debounce
let resizeTimeout;
window.addEventListener('resize', () => {
  clearTimeout(resizeTimeout);
  resizeTimeout = setTimeout(() => {
    canvas.width = window.innerWidth;
    canvas.height = window.innerHeight;
  }, 250);
});
```

**Performance notes:**
- 50 particles = ~1225 distance calculations per frame. Keep particle count reasonable.
- Use `Math.floor()` on coordinates to avoid sub-pixel rendering.
- Clear only changed regions for complex animations (overkill for this use case).
- Consider offscreen canvas for static background elements.

## SEO Meta Tags (Essential for Landing Page)

```html
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">

  <!-- Primary Meta Tags -->
  <title>Open Buro - European Workplace Orchestration Standard</title>
  <meta name="title" content="Open Buro - European Workplace Orchestration Standard">
  <meta name="description" content="An open European standard for workplace orchestration. [140-160 chars, focus on user intent]">

  <!-- Open Graph / Facebook -->
  <meta property="og:type" content="website">
  <meta property="og:url" content="https://openburo.eu/">
  <meta property="og:title" content="Open Buro - European Workplace Orchestration Standard">
  <meta property="og:description" content="[Same as meta description]">
  <meta property="og:image" content="https://openburo.eu/og-image.png">

  <!-- Twitter -->
  <meta property="twitter:card" content="summary_large_image">
  <meta property="twitter:url" content="https://openburo.eu/">
  <meta property="twitter:title" content="Open Buro - European Workplace Orchestration Standard">
  <meta property="twitter:description" content="[Same as meta description]">
  <meta property="twitter:image" content="https://openburo.eu/og-image.png">

  <!-- Canonical -->
  <link rel="canonical" href="https://openburo.eu/">

  <!-- Color Scheme -->
  <meta name="color-scheme" content="dark light">
  <meta name="theme-color" content="#0a0a0a" media="(prefers-color-scheme: dark)">
  <meta name="theme-color" content="#ffffff" media="(prefers-color-scheme: light)">
</head>
```

**Notes:**
- Title should be 50-60 characters
- Description should be 140-160 characters
- Use action words in descriptions to boost CTR
- Include only one H1 per page (site name/tagline)

## Sources

### High Confidence (Official Docs, Context7)
- [Tailwind CSS Play CDN Documentation](https://tailwindcss.com/docs/installation/play-cdn) - Official installation guide
- [MDN: Intersection Observer API](https://developer.mozilla.org/en-US/docs/Web/API/Intersection_Observer_API) - Browser API reference
- [MDN: requestAnimationFrame](https://developer.mozilla.org/en-US/docs/Web/API/Window/requestAnimationFrame) - Animation timing API
- [MDN: Optimizing Canvas](https://developer.mozilla.org/en-US/docs/Web/API/Canvas_API/Tutorial/Optimizing_canvas) - Performance best practices
- [MDN: prefers-color-scheme](https://developer.mozilla.org/en-US/docs/Web/CSS/Reference/At-rules/@media/prefers-color-scheme) - Dark mode media query

### Medium Confidence (Verified Community Sources)
- [Sal.js GitHub](https://github.com/mciastek/sal) - Scroll animation library
- [Sal.js CDN (jsDelivr)](https://www.jsdelivr.com/package/npm/sal.js) - CDN hosting
- [Chrome DevTools: Lighthouse Performance Scoring](https://developer.chrome.com/docs/lighthouse/performance/performance-scoring) - Official scoring guide
- [Google Fonts Optimization Guide](https://wp-rocket.me/blog/self-hosting-google-fonts/) - Performance comparison
- [HTML5 Canvas Performance Tips (GitHub Gist)](https://gist.github.com/jaredwilli/5469626) - Community best practices

### Low Confidence (WebSearch, Multiple Sources)
- [Landing Page Best Practices 2026](https://www.involve.me/blog/landing-page-best-practices) - Industry trends
- [SVG vs Canvas Animation (2026)](https://www.augustinfotech.com/blogs/svg-vs-canvas-animation-what-modern-frontends-should-use-in-2026/) - Technology comparison
- [Dark Mode Best Practices (Medium)](https://medium.com/@social_7132/dark-mode-done-right-best-practices-for-2026-c223a4b92417) - Design patterns

---

**Stack Research for:** Single-page static landing site (Open Buro)
**Researched:** 2026-02-16
**Confidence:** HIGH (Core technologies verified via official docs and MDN)
