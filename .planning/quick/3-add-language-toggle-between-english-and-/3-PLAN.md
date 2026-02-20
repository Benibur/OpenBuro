---
phase: quick-3
plan: 01
type: execute
wave: 1
depends_on: []
files_modified:
  - index.html
autonomous: true
requirements: [I18N-TOGGLE]

must_haves:
  truths:
    - "User can see a language toggle (EN/FR) in the navbar"
    - "Clicking the toggle switches ALL visible text content to French"
    - "Clicking again switches back to English"
    - "Language preference persists across page reload via localStorage"
    - "The html lang attribute updates to match selected language"
    - "Page layout and animations are unaffected by language switch"
  artifacts:
    - path: "index.html"
      provides: "Language toggle UI, i18n system, French translations"
      contains: "data-i18n"
  key_links:
    - from: "language toggle button"
      to: "i18n translation system"
      via: "click handler calling setLanguage()"
      pattern: "setLanguage"
    - from: "data-i18n attributes"
      to: "translations object"
      via: "querySelectorAll('[data-i18n]')"
      pattern: "data-i18n"
---

<objective>
Add a language toggle between English and French to the Open Buro landing page.

Purpose: The site targets a European audience (French government DINUM + broader EU). A bilingual EN/FR toggle makes the content accessible to francophone stakeholders while keeping the broader English default.

Output: Updated index.html with language toggle in navbar, i18n system in JS, and complete French translations for all visible text content.
</objective>

<execution_context>
@/home/ben/.claude/get-shit-done/workflows/execute-plan.md
@/home/ben/.claude/get-shit-done/templates/summary.md
</execution_context>

<context>
@index.html
</context>

<tasks>

<task type="auto">
  <name>Task 1: Add i18n infrastructure, toggle UI, and data-i18n attributes to all translatable elements</name>
  <files>index.html</files>
  <action>
This task adds the complete i18n system to index.html. Work in three parts:

**Part A: Language toggle UI in navbar**

1. Add a language toggle button in the navbar (`.nav-container`), placed between the nav links and the CTA button. Use a simple pill-style toggle showing "EN | FR" with the active language highlighted using `--color-primary`. Style it inline in the existing `<style>` block:
   ```css
   .lang-toggle {
     display: flex;
     align-items: center;
     gap: 0;
     border: 1px solid var(--color-border);
     border-radius: 999px;
     overflow: hidden;
     font-size: var(--text-sm);
     font-family: var(--font-body);
     font-weight: 600;
   }
   .lang-toggle button {
     background: none;
     border: none;
     padding: 0.35rem 0.75rem;
     cursor: pointer;
     color: var(--color-text-muted);
     transition: all 0.2s ease;
     font-family: inherit;
     font-weight: inherit;
     font-size: inherit;
   }
   .lang-toggle button.lang-active {
     background: var(--color-primary);
     color: #fff;
   }
   ```

2. Add the toggle HTML in the nav-container (after `.nav-desktop`, before the CTA link):
   ```html
   <div class="lang-toggle" role="radiogroup" aria-label="Language selection">
     <button id="lang-en" class="lang-active" role="radio" aria-checked="true" aria-label="English">EN</button>
     <button id="lang-fr" role="radio" aria-checked="false" aria-label="Francais">FR</button>
   </div>
   ```

3. Also add an identical toggle in the mobile drawer (`.mobile-drawer-links`) at the bottom, before the CTA button, centered.

**Part B: Add data-i18n attributes to ALL translatable text elements**

Add `data-i18n="key"` attributes to every element containing visible English text. Use a hierarchical key naming convention. Cover ALL sections:

- **Meta/Head**: Update dynamically via JS (title, meta description) -- use special keys `meta.title`, `meta.description`
- **Navbar**: nav links (`nav.problem`, `nav.vision`, `nav.pillars`, `nav.timeline`, `nav.ecosystem`, `nav.news`, `nav.newsletter`), CTA button text (`nav.cta`)
- **Hero**: badge (`hero.badge`), h1 (`hero.title`), subtitle (`hero.subtitle`), CTA buttons (`hero.cta.join`, `hero.cta.learn`)
- **Problem section**: badge (`problem.badge`), h2 (`problem.title`), paragraphs (use `problem.text1`, `problem.text2`, etc. -- preserve inner HTML with `<strong>` tags), list items (`problem.li1`, `problem.li2`, `problem.li3`), closing paragraph (`problem.conclusion`), card titles and descriptions (`problem.card1.title`, `problem.card1.desc`, etc.)
- **Vision section**: badge, h2, subtitle, SVG text elements (title, labels within SVG groups), diagram labels
- **Pillars section**: badge, h2, subtitle, "The Standard" and "The Alliance" headings, all 7 accordion trigger titles, all 7 accordion content paragraphs, all 5 alliance block titles and descriptions, blockquote text and cite
- **Timeline section**: badge, h2, intro text, all milestone dates/titles/descriptions
- **Ecosystem section**: badge, h2, intro, member card text, placeholder text, CTA title/description/buttons
- **News section**: badge, h2, intro, all 3 article titles, excerpts, date tags, "Read" link text
- **Newsletter section**: h2, description, "Coming soon" text
- **Footer**: tagline, "Navigation" heading, "Contact" heading, copyright text (preserve links)

For elements with inner HTML (containing `<strong>`, `<a>`, `<i>` tags), the translation system must use `innerHTML` not `textContent`. Mark these with `data-i18n-html="true"`.

**Part C: JavaScript i18n engine**

Add a new script section (or add to the existing DOMContentLoaded block) with:

1. A `translations` object with `en` and `fr` keys, each containing ALL translation key-value pairs. The English values should match the current text exactly. The French translations should be high-quality, professional French appropriate for an institutional/government audience (not casual). Key French translations:

   - "European Workplace Orchestration Standard" -> "Standard Europeen d'Orchestration de l'Espace de Travail"
   - "The European Standard for Workplace Orchestration" -> "Le Standard Europeen pour l'Orchestration de l'Espace de Travail"
   - "Join the Alliance" -> "Rejoindre l'Alliance"
   - "Learn More" -> "En savoir plus"
   - "Problem" -> "Probleme" (nav)
   - "Vision" -> "Vision"
   - "Pillars" -> "Piliers"
   - "Timeline" -> "Calendrier"
   - "Ecosystem" -> "Ecosysteme"
   - "News" -> "Actualites"
   - "Newsletter" -> "Lettre d'info"
   - "The Workplace Tech Paradox" -> "Le Paradoxe Technologique du Poste de Travail"
   - "From Fragmentation to Unity" -> "De la Fragmentation a l'Unite"
   - "Standard + Alliance" -> "Standard + Alliance"
   - "Project Timeline" -> "Calendrier du Projet"
   - "The Alliance" -> "L'Alliance"
   - "Latest News" -> "Dernieres Actualites"
   - "Stay Informed" -> "Restez Informe"
   - And ALL other text elements -- every single visible string

   Use proper French with accented characters (e, a, etc.) encoded as UTF-8 directly.

2. A `setLanguage(lang)` function that:
   - Queries all `[data-i18n]` elements and sets their textContent (or innerHTML if `data-i18n-html="true"`) from translations[lang][key]
   - Updates `document.documentElement.lang` to the language code
   - Updates `<title>` and meta description
   - Saves to `localStorage.setItem('lang', lang)`
   - Updates toggle button active states (both desktop and mobile toggles)
   - Updates aria-checked on radio buttons

3. Initialization in DOMContentLoaded:
   - Read `localStorage.getItem('lang')` -- default to `'en'` if not set
   - Call `setLanguage(savedLang)` on load
   - Attach click handlers to all 4 toggle buttons (2 desktop EN/FR + 2 mobile EN/FR)

4. Do NOT translate: emoji characters in accordion headers, SVG path data, CSS class names, aria-label values on decorative elements, or JavaScript variable names.

5. For SVG text elements in the vision diagram, use `data-i18n` on the `<text>` elements inside the SVG.

**Important implementation notes:**
- The FOUC prevention script at the top of `<head>` should also check for saved language and set `lang` attribute early (similar to dark mode pattern)
- Preserve ALL existing functionality: dark mode, animations, accordion, canvas, scroll behavior, mobile drawer
- The toggle should be visible on both mobile and desktop
- Use UTF-8 encoded French characters directly (the file already has charset UTF-8)
  </action>
  <verify>
1. Open index.html in browser -- language toggle visible in navbar
2. Click FR -- all text switches to French, html lang="fr"
3. Click EN -- all text switches back to English, html lang="en"
4. Reload page -- language preference persists
5. Check mobile view -- toggle visible in mobile drawer
6. Verify no JavaScript errors in console
7. Verify accordion, canvas animation, scroll animations still work
8. Run: grep -c "data-i18n" index.html -- should return 80+ (one per translatable element)
  </verify>
  <done>
Language toggle visible in desktop navbar and mobile drawer. Clicking FR switches all visible text to French. Clicking EN switches back to English. Language preference saved in localStorage and restored on reload. html lang attribute updates. All existing functionality preserved. Every visible text string on the page has a French translation.
  </done>
</task>

</tasks>

<verification>
1. Desktop: Language toggle visible, click FR, all content French, click EN, all content English
2. Mobile: Open drawer, language toggle visible, switching works
3. Persistence: Set to FR, reload page, still FR
4. html lang attribute: Inspect element, confirms lang="fr" when French active
5. No regressions: Canvas animation runs, scroll animations trigger, accordion expands/collapses, dark mode toggles, mobile drawer opens/closes
6. Quality: French text reads naturally, no untranslated strings visible
</verification>

<success_criteria>
- Language toggle (EN|FR pill) present in desktop navbar and mobile drawer
- ALL visible English text has a French translation (80+ translatable elements)
- Language switches instantly on click with no page reload
- Language preference persists in localStorage across sessions
- html lang attribute updates to "en" or "fr"
- Zero regressions on existing features (animations, dark mode, accordion, mobile drawer)
- French translations are professional quality (institutional tone)
</success_criteria>

<output>
After completion, create `.planning/quick/3-add-language-toggle-between-english-and-/3-SUMMARY.md`
</output>
