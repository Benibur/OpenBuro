# Architecture Patterns: Single-File Static Landing Pages

**Domain:** Single-page static landing site
**Researched:** 2026-02-16
**Confidence:** MEDIUM

## Recommended Architecture

Single-file landing pages follow a **vertical layer architecture** where everything lives in one HTML file but organized into distinct logical layers that communicate through events and shared state.

### System Overview

```
┌─────────────────────────────────────────────────────────────┐
│                       DESIGN LAYER                           │
│  CSS Custom Properties (Design System Variables)            │
│  ├── Primitive tokens (colors, spacing, typography)         │
│  ├── Semantic tokens (--color-primary, --space-section)     │
│  └── Component tokens (--nav-height, --hero-padding)        │
├─────────────────────────────────────────────────────────────┤
│                      PRESENTATION LAYER                      │
│  HTML Structure + Internal CSS (<style> in <head>)          │
│  ├── Sticky Navigation (mobile hamburger)                   │
│  ├── Hero Section (with canvas container)                   │
│  ├── Content Sections (7 sections with scroll triggers)     │
│  ├── Newsletter Form                                         │
│  └── Footer                                                  │
├─────────────────────────────────────────────────────────────┤
│                      BEHAVIOR LAYER                          │
│  JavaScript Modules (IIFE pattern in <script> tags)         │
│  ├── App Orchestrator (initialization, lifecycle)           │
│  ├── Navigation Controller (sticky, mobile menu)            │
│  ├── Canvas Animation Manager (hero animation)              │
│  ├── Scroll Observer (Intersection Observer wrapper)        │
│  ├── Form Handler (validation, submission)                  │
│  └── Theme Manager (dark mode toggle)                       │
└─────────────────────────────────────────────────────────────┘
```

### Component Boundaries

| Component | Responsibility | Communicates With |
|-----------|----------------|-------------------|
| **Design System (CSS Custom Properties)** | Define all design tokens (colors, spacing, typography); provide theming infrastructure | Referenced by all CSS rules; updated by Theme Manager |
| **Navigation Controller** | Sticky positioning, mobile hamburger toggle, smooth scroll to sections | App Orchestrator (init), Scroll Observer (active state updates) |
| **Canvas Animation Manager** | Initialize canvas, run animation loop via requestAnimationFrame, handle resize | App Orchestrator (init), window resize events |
| **Scroll Observer** | Wrap Intersection Observer API, trigger animations on sections, update nav active state | Navigation Controller (section visibility), Content Sections (animation triggers) |
| **Form Handler** | Validate email input (HTML5 + custom), handle submission, show success/error states | Newsletter Form element, optional backend API |
| **Theme Manager** | Toggle dark/light mode, sync with system preference, persist user choice | Design System (CSS custom properties), localStorage |
| **App Orchestrator** | Initialize all modules in correct order, handle DOMContentLoaded, coordinate lifecycle | All controllers and managers |

### Data Flow

```
User Action (Page Load)
    ↓
DOMContentLoaded Event
    ↓
App Orchestrator.init()
    ↓
┌──────────────────┬──────────────────┬──────────────────┐
│                  │                  │                  │
Nav Controller   Canvas Manager   Scroll Observer   Theme Manager
    .init()          .init()          .init()           .init()
    │                │                │                  │
    │                │                │                  │
Attach click    Get canvas ref   Create observer    Check system pref
handlers        Start RAF loop   Watch sections     Apply theme
    │                │                │                  │
    └────────────────┴────────────────┴──────────────────┘
                         │
                    Ready State
                         ↓
              User Interactions
         (scroll, click, form submit)
```

## Recommended File Structure

For a single-file landing page, structure follows **document order** with clear section markers:

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <!-- Meta tags, viewport, title -->

  <!-- External dependencies (CDN links) -->
  <!-- Tailwind CSS CDN -->
  <!-- Google Fonts -->

  <!-- DESIGN LAYER: Internal CSS -->
  <style>
    /* 1. CSS Custom Properties (Design Tokens) */
    :root { /* Light mode tokens */ }
    .dark { /* Dark mode overrides */ }

    /* 2. Base Styles */
    /* Reset, typography, utilities */

    /* 3. Layout Components */
    /* Grid, container, section spacing */

    /* 4. Feature Components */
    /* Navigation, hero, sections, forms */

    /* 5. Animation Definitions */
    /* @keyframes, transitions, scroll animations */

    /* 6. Responsive Overrides (Mobile-First) */
    /* @media (min-width: ...) for tablet, desktop */
  </style>
</head>

<body>
  <!-- PRESENTATION LAYER: HTML Structure -->

  <!-- Sticky Navigation -->
  <nav><!-- Logo, desktop links, mobile hamburger --></nav>

  <!-- Hero Section -->
  <section id="hero">
    <canvas id="hero-canvas"></canvas>
    <!-- Hero content -->
  </section>

  <!-- Content Sections (7 sections) -->
  <section id="section-1" class="scroll-animate">...</section>
  <section id="section-2" class="scroll-animate">...</section>
  <!-- ... -->

  <!-- Newsletter Section -->
  <section id="newsletter">
    <form id="newsletter-form">...</form>
  </section>

  <!-- Footer -->
  <footer>...</footer>

  <!-- BEHAVIOR LAYER: JavaScript Modules -->
  <script>
    // App Orchestrator
    // Navigation Controller
    // Canvas Animation Manager
    // Scroll Observer
    // Form Handler
    // Theme Manager
  </script>
</body>
</html>
```

### Structure Rationale

- **CSS in `<head>`**: Prevents flash of unstyled content (FOUC), allows progressive rendering as HTML streams
- **JavaScript at `</body>`**: Ensures DOM is ready before script execution; no need for DOMContentLoaded if placed here (but still used for clarity)
- **External dependencies first**: CDN links load before internal styles for proper cascade order
- **Mobile-first CSS**: Base styles target mobile, media queries progressively enhance for larger screens
- **Component tokens layer**: Maps semantic meaning to design primitives, enabling dark mode via single class toggle

## Architectural Patterns

### Pattern 1: Revealing Module Pattern (JavaScript Organization)

**What:** Encapsulate each feature in an IIFE that returns a public API, preventing global namespace pollution

**When to use:** For every distinct piece of functionality (nav, canvas, scroll observer, forms, theme)

**Trade-offs:**
- **Pros**: Clean separation, no global pollution, clear dependencies, testable units
- **Cons**: Slightly more verbose than simple functions; requires manual dependency injection

**Example:**
```javascript
const NavigationController = (function() {
  // Private state
  let isMenuOpen = false;
  let navElement = null;
  let hamburgerBtn = null;

  // Private methods
  function toggleMenu() {
    isMenuOpen = !isMenuOpen;
    navElement.classList.toggle('menu-open', isMenuOpen);
    hamburgerBtn.setAttribute('aria-expanded', isMenuOpen);
  }

  function handleScroll() {
    const shouldStick = window.scrollY > 50;
    navElement.classList.toggle('sticky', shouldStick);
  }

  // Public API
  return {
    init: function() {
      navElement = document.querySelector('nav');
      hamburgerBtn = document.getElementById('hamburger-btn');

      hamburgerBtn?.addEventListener('click', toggleMenu);
      window.addEventListener('scroll', handleScroll);
    },

    setActiveSection: function(sectionId) {
      // Update nav links active state
      const links = navElement.querySelectorAll('a[href^="#"]');
      links.forEach(link => {
        const isActive = link.getAttribute('href') === `#${sectionId}`;
        link.classList.toggle('active', isActive);
      });
    }
  };
})();
```

### Pattern 2: CSS Custom Properties Theming

**What:** Define all colors, spacing, and typography as CSS custom properties with semantic naming, enabling instant theme switching

**When to use:** For any project requiring dark mode or multiple themes

**Trade-offs:**
- **Pros**: Zero-latency theme switching, no JavaScript DOM manipulation for styling, system preference integration
- **Cons**: Requires disciplined token naming; must avoid hard-coded values throughout CSS

**Example:**
```css
/* Layered token system: Primitive → Semantic → Component */
:root {
  /* Primitives (raw values) */
  --gray-50: #f9fafb;
  --gray-900: #111827;
  --blue-600: #2563eb;

  /* Semantic tokens (light mode defaults) */
  --color-bg: var(--gray-50);
  --color-text: var(--gray-900);
  --color-primary: var(--blue-600);

  /* Component tokens */
  --nav-height: 4rem;
  --section-padding: 5rem 1.5rem;
}

/* Dark mode overrides (only redefine semantic layer) */
.dark {
  --color-bg: var(--gray-900);
  --color-text: var(--gray-50);
  --color-primary: #60a5fa; /* Lighter blue for contrast */
}

/* All components reference semantic tokens */
body {
  background: var(--color-bg);
  color: var(--color-text);
}
```

### Pattern 3: Intersection Observer for Scroll Animations

**What:** Centralized scroll observation using Intersection Observer API to trigger animations when elements enter viewport

**When to use:** For scroll-triggered animations on multiple sections (better performance than scroll event listeners)

**Trade-offs:**
- **Pros**: Highly performant (async, no scroll event thrashing), built-in threshold controls, works with lazy loading
- **Cons**: Requires polyfill for IE11 (not relevant in 2026); threshold tuning needed for good UX

**Example:**
```javascript
const ScrollObserver = (function() {
  let observer = null;

  function handleIntersection(entries) {
    entries.forEach(entry => {
      if (entry.isIntersecting) {
        // Add animated class when entering viewport
        entry.target.classList.add('animated');

        // Optional: unobserve after animation to prevent re-triggering
        // observer.unobserve(entry.target);
      }
    });
  }

  return {
    init: function(options = {}) {
      const defaultOptions = {
        root: null, // viewport
        rootMargin: '0px 0px -100px 0px', // Trigger 100px before element enters
        threshold: 0.15 // 15% visible
      };

      observer = new IntersectionObserver(
        handleIntersection,
        { ...defaultOptions, ...options }
      );

      // Observe all sections with .scroll-animate class
      document.querySelectorAll('.scroll-animate').forEach(section => {
        observer.observe(section);
      });
    }
  };
})();
```

### Pattern 4: Canvas Animation with requestAnimationFrame

**What:** Manage hero canvas animation using RAF loop with proper initialization and cleanup

**When to use:** For animated backgrounds, particle effects, or generative graphics in hero sections

**Trade-offs:**
- **Pros**: 60fps smooth animations, automatic pause when tab inactive, hardware accelerated
- **Cons**: Higher complexity than CSS animations; requires resize handling and cleanup

**Example:**
```javascript
const CanvasAnimationManager = (function() {
  let canvas = null;
  let ctx = null;
  let animationId = null;
  let particles = [];

  function resizeCanvas() {
    canvas.width = canvas.offsetWidth;
    canvas.height = canvas.offsetHeight;
  }

  function animate() {
    // Clear canvas
    ctx.clearRect(0, 0, canvas.width, canvas.height);

    // Update and draw particles
    particles.forEach(particle => {
      particle.update();
      particle.draw(ctx);
    });

    // Request next frame
    animationId = requestAnimationFrame(animate);
  }

  return {
    init: function() {
      canvas = document.getElementById('hero-canvas');
      if (!canvas) return;

      ctx = canvas.getContext('2d');
      resizeCanvas();

      // Initialize particles
      particles = Array.from({ length: 50 }, () => new Particle(canvas));

      // Start animation loop
      animate();

      // Handle window resize
      window.addEventListener('resize', resizeCanvas);
    },

    destroy: function() {
      if (animationId) {
        cancelAnimationFrame(animationId);
      }
      window.removeEventListener('resize', resizeCanvas);
    }
  };
})();
```

### Pattern 5: Progressive Form Validation

**What:** Combine HTML5 validation with custom JavaScript for real-time feedback without annoying users mid-typing

**When to use:** For all forms, especially newsletter signups with minimal fields

**Trade-offs:**
- **Pros**: Accessible (works without JS), progressive enhancement, good UX with debounced feedback
- **Cons**: Must maintain validation logic in both HTML and JS; server-side validation still required

**Example:**
```javascript
const FormHandler = (function() {
  let form = null;
  let emailInput = null;
  let submitBtn = null;

  function validateEmail(email) {
    // HTML5 built-in validation
    if (!emailInput.validity.valid) {
      return emailInput.validationMessage;
    }

    // Custom validation (optional)
    if (email.endsWith('.con')) { // Common typo
      return 'Did you mean .com?';
    }

    return ''; // Valid
  }

  function showError(message) {
    const errorEl = form.querySelector('.error-message');
    errorEl.textContent = message;
    errorEl.classList.add('visible');
    emailInput.setAttribute('aria-invalid', 'true');
  }

  function clearError() {
    const errorEl = form.querySelector('.error-message');
    errorEl.classList.remove('visible');
    emailInput.setAttribute('aria-invalid', 'false');
  }

  async function handleSubmit(e) {
    e.preventDefault();

    const email = emailInput.value.trim();
    const error = validateEmail(email);

    if (error) {
      showError(error);
      return;
    }

    clearError();
    submitBtn.disabled = true;
    submitBtn.textContent = 'Subscribing...';

    try {
      // Submit to backend/service
      const response = await fetch('/api/newsletter', {
        method: 'POST',
        headers: { 'Content-Type': 'application/json' },
        body: JSON.stringify({ email })
      });

      if (!response.ok) throw new Error('Subscription failed');

      form.innerHTML = '<p class="success">Thanks for subscribing!</p>';
    } catch (err) {
      showError('Something went wrong. Please try again.');
      submitBtn.disabled = false;
      submitBtn.textContent = 'Subscribe';
    }
  }

  return {
    init: function() {
      form = document.getElementById('newsletter-form');
      if (!form) return;

      emailInput = form.querySelector('input[type="email"]');
      submitBtn = form.querySelector('button[type="submit"]');

      // Validate on blur (after user leaves field)
      emailInput.addEventListener('blur', () => {
        const error = validateEmail(emailInput.value.trim());
        if (error) showError(error);
        else clearError();
      });

      // Clear error on input (don't validate while typing)
      emailInput.addEventListener('input', clearError);

      form.addEventListener('submit', handleSubmit);
    }
  };
})();
```

### Pattern 6: App Orchestrator (Initialization)

**What:** Central initialization controller that boots all modules in correct dependency order

**When to use:** Always, for single-file apps with multiple JavaScript modules

**Trade-offs:**
- **Pros**: Single source of truth for startup, clear dependency order, easy to debug init issues
- **Cons**: Adds one extra abstraction layer; all modules must expose .init() method

**Example:**
```javascript
const App = (function() {
  return {
    init: function() {
      // Initialize in dependency order
      // 1. Theme first (affects rendering)
      ThemeManager.init();

      // 2. Visual components (independent)
      NavigationController.init();
      CanvasAnimationManager.init();

      // 3. Scroll observer (depends on DOM being ready)
      ScrollObserver.init();

      // 4. Forms and interactions
      FormHandler.init();

      console.log('Open Buro landing page initialized');
    }
  };
})();

// Boot on DOMContentLoaded
document.addEventListener('DOMContentLoaded', App.init);
```

## Anti-Patterns to Avoid

### Anti-Pattern 1: Inline Styles on HTML Elements

**What people do:** Add `style="color: blue;"` directly on HTML elements for quick styling

**Why it's wrong:** Breaks maintainability at scale. With 7 sections and responsive design, updating colors/spacing requires editing 20+ places instead of one CSS rule. Also impossible to override with media queries and prevents theming.

**Do this instead:** Use semantic CSS classes and reference design tokens:
```html
<!-- Bad -->
<div style="padding: 80px 24px; background: #1e293b;">

<!-- Good -->
<div class="section section-dark">
```

```css
/* Define once, reuse everywhere, theme-aware */
.section {
  padding: var(--section-padding);
}

.section-dark {
  background: var(--color-bg-dark);
}
```

### Anti-Pattern 2: Global Variable Pollution

**What people do:** Define functions and state directly in global scope without encapsulation

**Why it's wrong:** Variables clash with Tailwind's internal state, Google Fonts scripts, or future third-party integrations. Hard to track where state is modified. No clear module boundaries.

**Do this instead:** Use the Revealing Module Pattern (see Pattern 1) or namespace all globals:
```javascript
// Bad
let isMenuOpen = false;
function toggleMenu() { /* ... */ }

// Good
const OpenBuroApp = {
  navigation: NavigationController,
  canvas: CanvasAnimationManager,
  // Single global object
};
```

### Anti-Pattern 3: Scroll Event Listeners for Animations

**What people do:** Attach `scroll` event listener to trigger section animations:
```javascript
// Anti-pattern
window.addEventListener('scroll', () => {
  sections.forEach(section => {
    if (isInViewport(section)) {
      section.classList.add('animated');
    }
  });
});
```

**Why it's wrong:** Fires hundreds of times per second during scroll, causing jank and poor performance. Requires manual viewport calculations. Blocks main thread.

**Do this instead:** Use Intersection Observer API (see Pattern 3). It's async, performant, and purpose-built for viewport detection.

### Anti-Pattern 4: Mixing Dark Mode Logic Across CSS and JS

**What people do:** Toggle individual styles via JavaScript:
```javascript
// Anti-pattern
document.body.style.backgroundColor = isDark ? '#111' : '#fff';
document.querySelectorAll('.section').forEach(el => {
  el.style.color = isDark ? '#eee' : '#222';
});
```

**Why it's wrong:** Unmanageable at scale. Every new component requires JS updates. Inline styles override CSS specificity. Can't leverage CSS transitions. Causes layout thrashing.

**Do this instead:** Use CSS custom properties with a single class toggle (see Pattern 2):
```javascript
// Good: Single class toggle
document.documentElement.classList.toggle('dark', isDark);
```

All color/style changes cascade automatically via CSS custom properties.

### Anti-Pattern 5: No Mobile Hamburger Menu Accessibility

**What people do:** Create hamburger menus without proper ARIA attributes or keyboard navigation

**Why it's wrong:** Screen readers can't understand menu state. Keyboard users can't operate the menu. Fails WCAG accessibility standards.

**Do this instead:**
```html
<button
  id="hamburger-btn"
  aria-label="Toggle navigation menu"
  aria-expanded="false"
  aria-controls="mobile-menu">
  <span class="hamburger-icon"></span>
</button>

<nav id="mobile-menu" aria-hidden="true">
  <!-- Menu links -->
</nav>
```

```javascript
// Sync ARIA state with visual state
function toggleMenu() {
  isOpen = !isOpen;
  hamburgerBtn.setAttribute('aria-expanded', isOpen);
  mobileMenu.setAttribute('aria-hidden', !isOpen);
  menu.classList.toggle('open', isOpen);
}
```

### Anti-Pattern 6: Hard-Coded Breakpoints Throughout CSS

**What people do:** Repeat `@media (min-width: 768px)` dozens of times with different pixel values

**Why it's wrong:** Inconsistent breakpoints create layout bugs. Hard to refactor if design system changes. Doesn't align with Tailwind's breakpoint system.

**Do this instead:** Define breakpoints as CSS custom properties or use consistent named values:
```css
/* Define once */
:root {
  --breakpoint-md: 768px;
  --breakpoint-lg: 1024px;
}

/* Use consistently - BUT CSS custom properties don't work in media queries yet */
/* So document standard breakpoints at top of CSS: */

/*
  Breakpoint System (mobile-first):
  - Base: 0-767px (mobile)
  - md: 768px+ (tablet)
  - lg: 1024px+ (desktop)
*/

/* Then use consistently */
@media (min-width: 768px) { /* md */ }
@media (min-width: 1024px) { /* lg */ }
```

## Data Flow Patterns

### Page Load Flow

```
Browser Request
    ↓
HTML Streaming Begins
    ↓
<head> Parsed
    ↓
External CSS/Fonts Fetched (parallel)
    ↓
Internal <style> Parsed → CSSOM Built
    ↓
<body> Parsed → DOM Built
    ↓
First Paint (hero visible)
    ↓
<script> Executes
    ↓
DOMContentLoaded → App.init()
    ↓
Modules Initialize (theme, nav, canvas, observer, forms)
    ↓
Canvas RAF Loop Starts
    ↓
Intersection Observers Active
    ↓
Fully Interactive (TTI)
```

### User Interaction Flows

**1. Scroll Interaction:**
```
User Scrolls
    ↓
Intersection Observer Callback (async)
    ↓
Entry.isIntersecting Check
    ↓
Add .animated Class to Section
    ↓
CSS Transition Triggers (hardware accelerated)
    ↓
Update Navigation Active State
```

**2. Mobile Menu Toggle:**
```
User Clicks Hamburger
    ↓
NavigationController.toggleMenu()
    ↓
Update isMenuOpen State
    ↓
Toggle .menu-open Class
    ↓
Update ARIA Attributes (aria-expanded, aria-hidden)
    ↓
CSS Transition (menu slide-in)
```

**3. Dark Mode Toggle:**
```
User Clicks Theme Toggle
    ↓
ThemeManager.toggleTheme()
    ↓
Update isDark State
    ↓
Toggle .dark Class on <html>
    ↓
CSS Custom Properties Cascade
    ↓
All Colors Update Instantly (zero layout shift)
    ↓
Save Preference to localStorage
```

**4. Newsletter Submission:**
```
User Types Email → Input Event → Clear Previous Errors
    ↓
User Leaves Field → Blur Event → Validate Email (HTML5 + Custom)
    ↓
User Submits Form → Submit Event
    ↓
preventDefault() → Block Native Submission
    ↓
Validate Email Again
    ↓
Show Loading State (disable button)
    ↓
Fetch POST to Backend/Service
    ↓
Success → Replace Form with Success Message
    ↓
Error → Show Error Message, Re-enable Button
```

## Build Order Recommendations

For a single-file landing page, build in this order to minimize rework and enable incremental testing:

### Phase 1: Foundation (Critical Path)
1. **HTML Structure** — Semantic markup for all sections (nav, hero, 7 content sections, newsletter, footer)
2. **Design System (CSS Custom Properties)** — Define all tokens (colors, spacing, typography) in `:root` and `.dark`
3. **Base CSS** — Reset, typography, container, utilities (built on design tokens)

**Why first:** Establishes visual foundation. Can preview content structure immediately. Design tokens make theming trivial later.

### Phase 2: Layout & Responsive (Visual Structure)
4. **Mobile Layout (base styles)** — Single-column, vertical flow, mobile spacing
5. **Desktop Layout (media queries)** — Multi-column where appropriate, increased spacing
6. **Navigation (HTML/CSS only)** — Sticky behavior via `position: sticky`, hamburger icon styling

**Why second:** Visual hierarchy complete. Can validate responsive design before adding interactivity. Hamburger menu styled but not functional yet.

### Phase 3: Core Interactivity (JavaScript Modules)
7. **App Orchestrator** — `App.init()` skeleton with `DOMContentLoaded` listener
8. **Navigation Controller** — Mobile menu toggle, smooth scroll, sticky class on scroll
9. **Theme Manager** — Dark mode toggle, `prefers-color-scheme` detection, localStorage persistence

**Why third:** Core UX features functional. Navigation and theming are highest-priority interactions. Can test JS module pattern.

### Phase 4: Visual Enhancement (Animations)
10. **Scroll Observer** — Intersection Observer setup, `.animated` class application
11. **CSS Animations** — Define `@keyframes` for fade-in, slide-up, etc. triggered by `.animated` class
12. **Canvas Animation Manager** — Hero canvas setup, particle system or generative graphics

**Why fourth:** Progressive enhancement. Page works without animations. Canvas is most complex, saved for last in critical features.

### Phase 5: Forms & Validation
13. **Newsletter Form (HTML5 validation)** — Proper `type="email"`, `required`, error message elements
14. **Form Handler JS** — Custom validation, blur/input events, submit handling, loading states

**Why fifth:** Forms require backend integration. Can be developed while waiting for API. HTML5 validation provides baseline.

### Phase 6: Polish & Optimization
15. **Accessibility Audit** — ARIA labels, keyboard navigation, focus states, color contrast
16. **Performance Optimization** — Lazy load offscreen images, minimize CSS/JS, check Lighthouse score
17. **Cross-browser Testing** — Safari, Firefox, Chrome on mobile/desktop

**Why last:** Refinement phase. Core functionality complete. Can validate against real usage.

## Integration Points

### External Services

| Service | Integration Pattern | Notes |
|---------|---------------------|-------|
| **Tailwind CSS CDN** | `<link>` tag in `<head>` | Use Play CDN for prototypes; generates classes on-demand. For production, consider build step to purge unused classes (200KB → 10KB). |
| **Google Fonts** | `<link>` tag in `<head>` with `display=swap` | Preconnect to `fonts.googleapis.com` and `fonts.gstatic.com` for performance. Limit to 2-3 font families max. |
| **Newsletter API** | `fetch()` POST from FormHandler | Options: ConvertKit, Mailchimp, custom backend. Handle CORS. Always validate server-side. |
| **Analytics (optional)** | Async script tag before `</body>` | Google Analytics, Plausible, or Fathom. Respect DNT header. Cookie consent if EU traffic. |

### Internal Boundaries

| Boundary | Communication | Notes |
|----------|---------------|-------|
| **CSS Design Tokens ↔ JavaScript** | JS reads computed styles or toggles classes only | Never set colors/spacing in JS. Toggle `.dark` class, let CSS handle the rest. Avoids inline styles. |
| **ScrollObserver ↔ NavigationController** | ScrollObserver calls `NavigationController.setActiveSection(id)` | One-way communication. Observer owns section visibility state. |
| **CanvasAnimationManager ↔ Window Events** | Listen to `resize` for canvas dimensions | Use debouncing (300ms) to avoid resize thrashing. |
| **FormHandler ↔ Backend API** | Async `fetch()` with error handling | Always show loading state. Timeout after 10s. Graceful degradation if offline. |
| **ThemeManager ↔ localStorage** | Persist user theme choice as `{ theme: 'dark' \| 'light' }` | Respect `prefers-color-scheme` on first visit. User choice overrides system. |

## Scaling Considerations

| Scale | Architecture Adjustments |
|-------|--------------------------|
| **Single landing page (current)** | Inline CSS/JS in single HTML file is optimal. Fast initial load (one HTTP request). Easy to deploy (upload one file). No build step needed. |
| **Multiple landing pages (2-5 pages)** | Extract shared CSS to external `.css` file (cached across pages). Keep page-specific JS inline or create `shared.js` + `page-specific.js`. Consider templating system (Nunjucks, Handlebars) to avoid duplicating nav/footer. |
| **Marketing site (10+ pages)** | Migrate to static site generator (Astro, 11ty). Component-based architecture. Shared layouts/partials. PostCSS for CSS processing. Bundle optimization. |
| **Web app (interactive SPA)** | Migrate to React/Vue/Svelte. Requires build tooling. Single-file architecture no longer appropriate. Route-based code splitting. |

### When to Migrate Away from Single-File

**Triggers:**
- Need more than 3 landing pages (shared components justify extraction)
- CSS exceeds 1000 lines (hard to navigate)
- JavaScript exceeds 500 lines (consider TypeScript, modules)
- Team has more than 2 developers (merge conflicts on single file)
- SEO requires dynamic meta tags per page
- Analytics shows page weight hurting mobile conversions

**Migration path:**
1. Extract CSS to `styles.css`
2. Extract shared JS modules to `app.js`
3. Create page-specific bundles as needed
4. Adopt static site generator for templating
5. Add build step (Vite, Parcel, or esbuild)

## Sources

### High Confidence (Official Docs)
- [MDN: Intersection Observer API](https://developer.mozilla.org/en-US/docs/Web/API/Intersection_Observer_API)
- [MDN: Basic Canvas Animations](https://developer.mozilla.org/en-US/docs/Web/API/Canvas_API/Tutorial/Basic_animations)
- [MDN: Responsive Web Design](https://developer.mozilla.org/en-US/docs/Learn_web_development/Core/CSS_layout/Responsive_Design)
- [MDN: Organizing CSS](https://developer.mozilla.org/en-US/docs/Learn_web_development/Core/Styling_basics/Organizing)
- [MDN: JavaScript Modules](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Modules)

### Medium Confidence (Web Search Verified, Multiple Sources)
- [Patterns.dev: Module Pattern](https://www.patterns.dev/vanilla/module-pattern/)
- [CSS-Tricks: Dark Mode Guide](https://css-tricks.com/a-complete-guide-to-dark-mode-on-the-web/)
- [Tailwind CSS: Responsive Design](https://tailwindcss.com/docs/responsive-design)
- [FreeCodePamp: Scroll Animations with Intersection Observer](https://www.freecodecamp.org/news/scroll-animations-with-javascript-intersection-observer-api/)
- [Web.dev: light-dark() CSS Function](https://web.dev/articles/light-dark)
- [Go Make Things: Vanilla JS Project Structure](https://gomakethings.com/how-i-structure-my-vanilla-js-projects/)
- [Strapi: Vanilla JS Form Handling](https://strapi.io/blog/vanilla-javascript-form-handling-guide)
- [Zell Liew: Mobile-First CSS](https://zellwk.com/blog/how-to-write-mobile-first-css/)

### Landing Page Best Practices (2026)
- [Landing Page Design Structure That Converts](https://toimi.pro/blog/landing-page-design-structure-conversion/)
- [Involve.me: Landing Page Best Practices](https://www.involve.me/blog/landing-page-best-practices)
- [Unbounce: Best Landing Page Examples 2026](https://unbounce.com/landing-page-examples/best-landing-page-examples/)

---
*Architecture research for: Open Buro single-file static landing page*
*Researched: 2026-02-16*
*Confidence: MEDIUM (verified patterns from official docs + web search consensus)*
