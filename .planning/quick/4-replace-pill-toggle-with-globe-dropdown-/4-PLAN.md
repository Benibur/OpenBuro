---
phase: quick-4
plan: 01
type: execute
wave: 1
depends_on: []
files_modified: [index.html]
autonomous: true
requirements: [QUICK-4]

must_haves:
  truths:
    - "Globe dropdown button visible in desktop navbar showing current lang code (e.g. EN) with chevron"
    - "Clicking globe button opens dropdown listing all 5 languages with checkmark on active"
    - "Selecting a language switches all page text and closes dropdown"
    - "Globe dropdown works identically in mobile drawer"
    - "Clicking outside dropdown closes it"
    - "Escape key closes dropdown"
    - "Arrow keys navigate dropdown options"
    - "DE, IT, ES languages use English fallback text"
    - "Old pill toggle CSS and HTML fully removed"
  artifacts:
    - path: "index.html"
      provides: "Globe dropdown language selector with 5-language support"
      contains: "lang-dropdown"
  key_links:
    - from: "globe dropdown button click"
      to: "dropdown menu toggle"
      via: "aria-expanded toggle + classList"
      pattern: "aria-expanded"
    - from: "dropdown option click"
      to: "setLanguage()"
      via: "event listener on menu items"
      pattern: "setLanguage"
    - from: "translations object"
      to: "de/it/es keys"
      via: "cloned from en translations"
      pattern: "translations\\.de"
---

<objective>
Replace the pill-style EN|FR language toggle with a compact globe icon dropdown that supports 5 languages (EN, FR, DE, IT, ES). The dropdown must be fully accessible (keyboard navigation, ARIA attributes) and work on both desktop navbar and mobile drawer.

Purpose: Enable future multi-language support with a scalable UI pattern instead of the binary pill toggle.
Output: Updated index.html with globe dropdown replacing pill toggle in both desktop and mobile contexts.
</objective>

<execution_context>
@/home/ben/.claude/get-shit-done/workflows/execute-plan.md
@/home/ben/.claude/get-shit-done/templates/summary.md
</execution_context>

<context>
@.planning/STATE.md
@index.html
</context>

<tasks>

<task type="auto">
  <name>Task 1: Replace pill toggle CSS + HTML with globe dropdown</name>
  <files>index.html</files>
  <action>
**CSS changes (replace section "22. LANGUAGE TOGGLE" at ~lines 2062-2100):**

Remove all `.lang-toggle`, `.lang-toggle button`, `.lang-toggle button.lang-active`, and `.mobile-lang-toggle` rules. Replace with:

```css
/* 22. LANGUAGE DROPDOWN */
.lang-dropdown {
  position: relative;
  font-family: var(--font-body);
  font-size: var(--text-sm);
}

.lang-dropdown-trigger {
  display: flex;
  align-items: center;
  gap: 0.4rem;
  background: none;
  border: 1px solid var(--color-border);
  border-radius: 0.5rem;
  padding: 0.4rem 0.65rem;
  cursor: pointer;
  color: var(--color-text);
  font-family: inherit;
  font-weight: 600;
  font-size: inherit;
  transition: border-color 0.2s ease, background 0.2s ease;
  min-height: 36px;
}

.lang-dropdown-trigger:hover {
  border-color: var(--color-primary);
}

.lang-dropdown-trigger svg {
  width: 18px;
  height: 18px;
  flex-shrink: 0;
}

.lang-dropdown-trigger .chevron {
  width: 12px;
  height: 12px;
  transition: transform 0.2s ease;
}

.lang-dropdown[aria-expanded="true"] .chevron {
  transform: rotate(180deg);
}

.lang-dropdown-menu {
  display: none;
  position: absolute;
  top: calc(100% + 4px);
  right: 0;
  min-width: 160px;
  background: var(--color-surface);
  border: 1px solid var(--color-border);
  border-radius: 0.5rem;
  box-shadow: 0 4px 12px rgba(0,0,0,0.1);
  z-index: 1000;
  padding: 0.25rem 0;
  list-style: none;
  margin: 0;
}

.dark .lang-dropdown-menu {
  box-shadow: 0 4px 12px rgba(0,0,0,0.3);
}

.lang-dropdown[aria-expanded="true"] .lang-dropdown-menu {
  display: block;
}

.lang-dropdown-menu li {
  margin: 0;
}

.lang-dropdown-menu button {
  display: flex;
  align-items: center;
  justify-content: space-between;
  width: 100%;
  padding: 0.5rem 0.75rem;
  background: none;
  border: none;
  cursor: pointer;
  color: var(--color-text);
  font-family: var(--font-body);
  font-size: var(--text-sm);
  font-weight: 500;
  text-align: left;
  transition: background 0.15s ease;
}

.lang-dropdown-menu button:hover,
.lang-dropdown-menu button:focus-visible {
  background: var(--color-bg-alt);
}

.lang-dropdown-menu button[aria-checked="true"] {
  font-weight: 700;
  color: var(--color-primary);
}

.lang-dropdown-menu .check-icon {
  width: 14px;
  height: 14px;
  opacity: 0;
}

.lang-dropdown-menu button[aria-checked="true"] .check-icon {
  opacity: 1;
}

/* Mobile drawer override: center the dropdown and open upward to avoid overflow */
.mobile-lang-toggle {
  display: flex;
  justify-content: center;
  margin-top: 1rem;
  margin-bottom: 0.5rem;
}

.mobile-lang-toggle .lang-dropdown-menu {
  bottom: calc(100% + 4px);
  top: auto;
  left: 50%;
  right: auto;
  transform: translateX(-50%);
}
```

**Desktop navbar HTML (replace lines ~2118-2121):**

Replace the `div.lang-toggle[role=radiogroup]` and its two buttons with:

```html
<div class="lang-dropdown" aria-expanded="false">
  <button class="lang-dropdown-trigger" aria-haspopup="listbox" aria-label="Select language">
    <svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round" aria-hidden="true"><circle cx="12" cy="12" r="10"/><path d="M2 12h20"/><path d="M12 2a15.3 15.3 0 0 1 4 10 15.3 15.3 0 0 1-4 10 15.3 15.3 0 0 1-4-10A15.3 15.3 0 0 1 12 2z"/></svg>
    <span class="lang-code">EN</span>
    <svg class="chevron" xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2.5" stroke-linecap="round" stroke-linejoin="round" aria-hidden="true"><polyline points="6 9 12 15 18 9"/></svg>
  </button>
  <ul class="lang-dropdown-menu" role="listbox" aria-label="Languages">
    <li role="option"><button data-lang="en" role="option" aria-checked="true">English<svg class="check-icon" xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2.5" stroke-linecap="round" stroke-linejoin="round"><polyline points="20 6 9 17 4 12"/></svg></button></li>
    <li role="option"><button data-lang="fr" role="option" aria-checked="false">Fran&#231;ais<svg class="check-icon" xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2.5" stroke-linecap="round" stroke-linejoin="round"><polyline points="20 6 9 17 4 12"/></svg></button></li>
    <li role="option"><button data-lang="de" role="option" aria-checked="false">Deutsch<svg class="check-icon" xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2.5" stroke-linecap="round" stroke-linejoin="round"><polyline points="20 6 9 17 4 12"/></svg></button></li>
    <li role="option"><button data-lang="it" role="option" aria-checked="false">Italiano<svg class="check-icon" xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2.5" stroke-linecap="round" stroke-linejoin="round"><polyline points="20 6 9 17 4 12"/></svg></button></li>
    <li role="option"><button data-lang="es" role="option" aria-checked="false">Espa&#241;ol<svg class="check-icon" xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2.5" stroke-linecap="round" stroke-linejoin="round"><polyline points="20 6 9 17 4 12"/></svg></button></li>
  </ul>
</div>
```

**Mobile drawer HTML (replace lines ~2151-2156):**

Replace the `div.mobile-lang-toggle > div.lang-toggle` block with identical dropdown markup but wrapped in `div.mobile-lang-toggle`:

```html
<div class="mobile-lang-toggle">
  <div class="lang-dropdown" aria-expanded="false">
    <!-- Same trigger + menu markup as desktop, identical structure -->
  </div>
</div>
```

Use the exact same inner HTML as the desktop version (globe SVG + lang-code span + chevron + full menu with all 5 languages).

**FOUC prevention script (line ~27):**

Update the FOUC prevention script to handle DE/IT/ES in addition to EN/FR. The current `localStorage.getItem('lang') || 'en'` already works since it stores any string. No change needed unless you want to validate against the 5 supported values -- add a validation:
```js
const supportedLangs = ['en','fr','de','it','es'];
const savedLang = localStorage.getItem('lang');
document.documentElement.lang = supportedLangs.includes(savedLang) ? savedLang : 'en';
```
  </action>
  <verify>Open index.html in browser. Confirm: (1) Globe icon + "EN" + chevron visible in desktop navbar where pill toggle was, (2) Same dropdown visible in mobile drawer, (3) No remnants of old pill toggle CSS/HTML, (4) Chevron rotates when dropdown opens.</verify>
  <done>Old pill toggle fully replaced with globe dropdown markup and styles in both desktop nav and mobile drawer. FOUC script updated for 5 languages.</done>
</task>

<task type="auto">
  <name>Task 2: Add DE/IT/ES translations + refactor JS for dropdown behavior</name>
  <files>index.html</files>
  <action>
**Add 3 new translation objects (after the `fr: {...}` block, before closing `};` of translations):**

Clone the entire `en` translations object three times to create `de`, `it`, and `es` entries. These are English fallback placeholders. Only change the `meta.title` and `meta.description` to indicate the language name (so it is clear these are placeholders):
- `de: { ...all en keys copied... }` -- keep all values identical to EN
- `it: { ...all en keys copied... }` -- keep all values identical to EN
- `es: { ...all en keys copied... }` -- keep all values identical to EN

This makes the architecture ready for real translations to be dropped in later.

**Refactor `setLanguage(lang)` function (replace lines ~3559-3621):**

Remove all the hardcoded button ID logic (`lang-en`, `lang-fr`, `lang-en-mobile`, `lang-fr-mobile`, classList toggling). Replace with a data-driven approach:

```js
function setLanguage(lang) {
  if (!translations[lang]) return;

  // Update all data-i18n elements (unchanged)
  document.querySelectorAll('[data-i18n]').forEach(el => {
    const key = el.getAttribute('data-i18n');
    const value = translations[lang][key];
    if (value !== undefined) {
      if (el.getAttribute('data-i18n-html') === 'true') {
        el.innerHTML = value;
      } else {
        el.textContent = value;
      }
    }
  });

  // Update html lang attribute
  document.documentElement.lang = lang;

  // Update title and meta description
  const titleEl = document.querySelector('title');
  if (titleEl) titleEl.textContent = translations[lang]['meta.title'];
  const metaDesc = document.querySelector('meta[name="description"]');
  if (metaDesc) metaDesc.setAttribute('content', translations[lang]['meta.description']);

  // Save preference
  localStorage.setItem('lang', lang);

  // Update ALL dropdown instances (desktop + mobile)
  document.querySelectorAll('.lang-dropdown').forEach(dropdown => {
    // Update displayed lang code
    const codeEl = dropdown.querySelector('.lang-code');
    if (codeEl) codeEl.textContent = lang.toUpperCase();

    // Update aria-checked on menu buttons
    dropdown.querySelectorAll('.lang-dropdown-menu button[data-lang]').forEach(btn => {
      const isActive = btn.getAttribute('data-lang') === lang;
      btn.setAttribute('aria-checked', isActive ? 'true' : 'false');
    });

    // Close dropdown after selection
    dropdown.setAttribute('aria-expanded', 'false');
  });
}
```

**Add dropdown toggle + keyboard + click-outside logic (replace the event listener setup at lines ~3653-3662):**

Remove the old 4-button listener block. Replace with:

```js
// Initialize language dropdowns (desktop + mobile)
document.querySelectorAll('.lang-dropdown').forEach(dropdown => {
  const trigger = dropdown.querySelector('.lang-dropdown-trigger');
  const menu = dropdown.querySelector('.lang-dropdown-menu');
  const items = menu.querySelectorAll('button[data-lang]');

  // Toggle dropdown on trigger click
  trigger.addEventListener('click', (e) => {
    e.stopPropagation();
    const isOpen = dropdown.getAttribute('aria-expanded') === 'true';
    // Close all other dropdowns first
    document.querySelectorAll('.lang-dropdown').forEach(d => d.setAttribute('aria-expanded', 'false'));
    if (!isOpen) {
      dropdown.setAttribute('aria-expanded', 'true');
      // Focus first item
      items[0]?.focus();
    }
  });

  // Language selection
  items.forEach(btn => {
    btn.addEventListener('click', (e) => {
      e.stopPropagation();
      setLanguage(btn.getAttribute('data-lang'));
    });
  });

  // Keyboard navigation
  menu.addEventListener('keydown', (e) => {
    const focused = document.activeElement;
    const itemArr = Array.from(items);
    const idx = itemArr.indexOf(focused);

    if (e.key === 'ArrowDown') {
      e.preventDefault();
      const next = idx < itemArr.length - 1 ? idx + 1 : 0;
      itemArr[next].focus();
    } else if (e.key === 'ArrowUp') {
      e.preventDefault();
      const prev = idx > 0 ? idx - 1 : itemArr.length - 1;
      itemArr[prev].focus();
    } else if (e.key === 'Escape') {
      dropdown.setAttribute('aria-expanded', 'false');
      trigger.focus();
    } else if (e.key === 'Enter' || e.key === ' ') {
      e.preventDefault();
      if (focused.hasAttribute('data-lang')) {
        setLanguage(focused.getAttribute('data-lang'));
        trigger.focus();
      }
    }
  });
});

// Close dropdowns on click outside
document.addEventListener('click', () => {
  document.querySelectorAll('.lang-dropdown').forEach(d => d.setAttribute('aria-expanded', 'false'));
});
```

**Update the i18n initialization (around line ~3650-3651):**

Keep the existing `setLanguage(savedLang)` call, but ensure `savedLang` defaults properly:
```js
const supportedLangs = ['en', 'fr', 'de', 'it', 'es'];
const savedLang = localStorage.getItem('lang');
setLanguage(supportedLangs.includes(savedLang) ? savedLang : 'en');
```

**Update the i18n comment (line ~3243):**

Change `Bilingual language system (EN/FR)` to `Multi-language system (EN/FR/DE/IT/ES)`.
  </action>
  <verify>
Test in browser:
1. Click globe dropdown in desktop nav -- all 5 languages listed, checkmark on active
2. Select "Francais" -- page switches to FR, dropdown closes, button shows "FR"
3. Select "Deutsch" -- page shows English text (fallback), button shows "DE"
4. Refresh page -- language persists from localStorage
5. Press Escape while dropdown is open -- closes
6. Use arrow keys to navigate options
7. Click outside dropdown -- closes
8. Open mobile drawer -- same dropdown works there
9. Test on narrow viewport (~375px) -- dropdown doesn't overflow screen
  </verify>
  <done>
All 5 languages selectable from globe dropdown. EN and FR show real translations. DE, IT, ES show English fallback text. Dropdown keyboard-accessible with Escape/Arrow/Enter. Click-outside closes. Language persists across refresh. Both desktop and mobile dropdowns sync state.
  </done>
</task>

</tasks>

<verification>
1. `grep -c "lang-toggle" index.html` returns 0 (old pill toggle CSS/HTML fully removed)
2. `grep -c "lang-dropdown" index.html` returns multiple hits (new dropdown present)
3. `grep -c "translations.de" index.html` or `grep "de:" index.html` confirms DE translations exist
4. `grep "lang-en-mobile\|lang-fr-mobile\|lang-en\"\|lang-fr\"" index.html` returns 0 (old hardcoded IDs removed)
5. Open in browser at 375px width -- dropdown doesn't overflow viewport
6. Keyboard test: Tab to dropdown trigger, Enter to open, Arrow Down through items, Enter to select, Escape to close
</verification>

<success_criteria>
- Globe icon dropdown replaces pill toggle in desktop navbar and mobile drawer
- 5 languages available: EN, FR, DE, IT, ES
- EN/FR show full translations; DE/IT/ES show English fallback
- Dropdown accessible: aria-expanded, aria-haspopup, aria-checked, keyboard navigation
- Click outside and Escape close dropdown
- Language choice persists in localStorage across refresh
- No FOUC on any of the 5 languages
- Old pill toggle code (CSS classes, HTML, JS hardcoded IDs) fully removed
- Works on mobile viewports without overflow
</success_criteria>

<output>
After completion, create `.planning/quick/4-replace-pill-toggle-with-globe-dropdown-/4-SUMMARY.md`
</output>
