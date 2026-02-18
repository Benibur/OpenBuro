---
phase: quick-2
plan: 01
type: execute
wave: 1
depends_on: []
files_modified:
  - index.html
  - assets/js/tailwind-browser.js
  - assets/css/phosphor-duotone.css
  - assets/fonts/phosphor/Phosphor-Duotone.woff2
  - assets/fonts/ibm-plex-mono-400-latin.woff2
  - assets/fonts/ibm-plex-mono-400-latin-ext.woff2
  - assets/fonts/ibm-plex-mono-500-latin.woff2
  - assets/fonts/ibm-plex-mono-500-latin-ext.woff2
  - assets/fonts/ibm-plex-sans-400-latin.woff2
  - assets/fonts/ibm-plex-sans-400-latin-ext.woff2
  - assets/fonts/ibm-plex-sans-500-latin.woff2
  - assets/fonts/ibm-plex-sans-500-latin-ext.woff2
  - assets/fonts/ibm-plex-sans-600-latin.woff2
  - assets/fonts/ibm-plex-sans-600-latin-ext.woff2
  - assets/fonts/plus-jakarta-sans-500-latin.woff2
  - assets/fonts/plus-jakarta-sans-500-latin-ext.woff2
  - assets/fonts/plus-jakarta-sans-600-latin.woff2
  - assets/fonts/plus-jakarta-sans-600-latin-ext.woff2
  - assets/fonts/plus-jakarta-sans-700-latin.woff2
  - assets/fonts/plus-jakarta-sans-700-latin-ext.woff2
  - assets/fonts/plus-jakarta-sans-800-latin.woff2
  - assets/fonts/plus-jakarta-sans-800-latin-ext.woff2
  - assets/css/fonts.css
autonomous: true
requirements: []

must_haves:
  truths:
    - "Page makes zero HTTP requests to external domains when loaded"
    - "All fonts render identically to current CDN-served versions"
    - "All Phosphor duotone icons render correctly"
    - "Tailwind CSS classes continue to work at runtime"
  artifacts:
    - path: "assets/js/tailwind-browser.js"
      provides: "Local copy of @tailwindcss/browser@4 runtime"
    - path: "assets/css/phosphor-duotone.css"
      provides: "Phosphor Icons duotone stylesheet with corrected local font paths"
    - path: "assets/fonts/phosphor/Phosphor-Duotone.woff2"
      provides: "Phosphor duotone icon font file"
    - path: "assets/css/fonts.css"
      provides: "@font-face declarations for all Google Fonts with local paths"
    - path: "assets/fonts/"
      provides: "All woff2 font files for IBM Plex Mono, IBM Plex Sans, Plus Jakarta Sans"
  key_links:
    - from: "index.html"
      to: "assets/js/tailwind-browser.js"
      via: "script src attribute"
      pattern: "src=[\"']assets/js/tailwind-browser\\.js[\"']"
    - from: "index.html"
      to: "assets/css/phosphor-duotone.css"
      via: "link href attribute"
      pattern: "href=[\"']assets/css/phosphor-duotone\\.css[\"']"
    - from: "index.html"
      to: "assets/css/fonts.css"
      via: "link href attribute"
      pattern: "href=[\"']assets/css/fonts\\.css[\"']"
    - from: "assets/css/phosphor-duotone.css"
      to: "assets/fonts/phosphor/Phosphor-Duotone.woff2"
      via: "@font-face src url"
      pattern: "url\\([\"']?\\.\\./fonts/phosphor/Phosphor-Duotone\\.woff2"
    - from: "assets/css/fonts.css"
      to: "assets/fonts/*.woff2"
      via: "@font-face src url"
      pattern: "url\\([\"']?\\.\\./fonts/"
---

<objective>
Eliminate all external HTTP requests from the Open Buro landing page by downloading and locally serving: Tailwind CSS browser runtime, Google Fonts (IBM Plex Mono, IBM Plex Sans, Plus Jakarta Sans), and Phosphor Icons duotone stylesheet + font.

Purpose: The page should load with zero external domain requests, improving privacy, performance, reliability, and offline capability.
Output: Local copies of all external resources under assets/, updated index.html references.
</objective>

<execution_context>
@/home/ben/.claude/get-shit-done/workflows/execute-plan.md
@/home/ben/.claude/get-shit-done/templates/summary.md
</execution_context>

<context>
@index.html (lines 56-62 — external resource references)
</context>

<tasks>

<task type="auto">
  <name>Task 1: Download all external resources locally</name>
  <files>
    assets/js/tailwind-browser.js
    assets/css/phosphor-duotone.css
    assets/fonts/phosphor/Phosphor-Duotone.woff2
    assets/fonts/ibm-plex-mono-400-latin.woff2
    assets/fonts/ibm-plex-mono-400-latin-ext.woff2
    assets/fonts/ibm-plex-mono-500-latin.woff2
    assets/fonts/ibm-plex-mono-500-latin-ext.woff2
    assets/fonts/ibm-plex-sans-400-latin.woff2
    assets/fonts/ibm-plex-sans-400-latin-ext.woff2
    assets/fonts/ibm-plex-sans-500-latin.woff2
    assets/fonts/ibm-plex-sans-500-latin-ext.woff2
    assets/fonts/ibm-plex-sans-600-latin.woff2
    assets/fonts/ibm-plex-sans-600-latin-ext.woff2
    assets/fonts/plus-jakarta-sans-500-latin.woff2
    assets/fonts/plus-jakarta-sans-500-latin-ext.woff2
    assets/fonts/plus-jakarta-sans-600-latin.woff2
    assets/fonts/plus-jakarta-sans-600-latin-ext.woff2
    assets/fonts/plus-jakarta-sans-700-latin.woff2
    assets/fonts/plus-jakarta-sans-700-latin-ext.woff2
    assets/fonts/plus-jakarta-sans-800-latin.woff2
    assets/fonts/plus-jakarta-sans-800-latin-ext.woff2
    assets/css/fonts.css
  </files>
  <action>
    Create directory structure:
    - `assets/js/`
    - `assets/css/`
    - `assets/fonts/`
    - `assets/fonts/phosphor/`

    **1. Tailwind CSS browser runtime:**
    Download `https://cdn.jsdelivr.net/npm/@tailwindcss/browser@4` to `assets/js/tailwind-browser.js`.

    **2. Phosphor Icons:**
    - Download `https://cdn.jsdelivr.net/npm/@phosphor-icons/web@2.1.2/src/duotone/style.css` to `assets/css/phosphor-duotone.css`.
    - Download `https://cdn.jsdelivr.net/npm/@phosphor-icons/web@2.1.2/src/duotone/Phosphor-Duotone.woff2` to `assets/fonts/phosphor/Phosphor-Duotone.woff2`.
    - Edit `assets/css/phosphor-duotone.css`: Replace the @font-face src URLs. The original CSS uses `url("./Phosphor-Duotone.woff2")` etc. (relative to CSS file location). Replace ALL font references so they point to `../fonts/phosphor/Phosphor-Duotone.woff2` (relative from `assets/css/` to `assets/fonts/phosphor/`).
    - Only keep the woff2 format in the @font-face src (remove woff, ttf, svg references — woff2 has universal browser support). This avoids needing to download 3 extra font files.

    **3. Google Fonts:**
    Fetch the Google Fonts CSS from `https://fonts.googleapis.com/css2?family=IBM+Plex+Mono:wght@400;500&family=IBM+Plex+Sans:wght@400;500;600&family=Plus+Jakarta+Sans:wght@500;600;700;800&display=swap` using a Chrome user-agent header to get woff2 format URLs.

    For each font family and weight, download ONLY the `latin` and `latin-ext` subset woff2 files (skip cyrillic, cyrillic-ext, vietnamese, greek). This is a European site — latin + latin-ext covers all European languages.

    Naming convention for downloaded files: `{family}-{weight}-{subset}.woff2`, e.g.:
    - `ibm-plex-mono-400-latin.woff2`
    - `ibm-plex-mono-400-latin-ext.woff2`
    - `ibm-plex-mono-500-latin.woff2`
    - `ibm-plex-mono-500-latin-ext.woff2`
    - `ibm-plex-sans-400-latin.woff2`
    - `ibm-plex-sans-400-latin-ext.woff2`
    - `ibm-plex-sans-500-latin.woff2`
    - `ibm-plex-sans-500-latin-ext.woff2`
    - `ibm-plex-sans-600-latin.woff2`
    - `ibm-plex-sans-600-latin-ext.woff2`
    - `plus-jakarta-sans-500-latin.woff2`
    - `plus-jakarta-sans-500-latin-ext.woff2`
    - `plus-jakarta-sans-600-latin.woff2`
    - `plus-jakarta-sans-600-latin-ext.woff2`
    - `plus-jakarta-sans-700-latin.woff2`
    - `plus-jakarta-sans-700-latin-ext.woff2`
    - `plus-jakarta-sans-800-latin.woff2`
    - `plus-jakarta-sans-800-latin-ext.woff2`

    All go into `assets/fonts/`.

    **4. Create `assets/css/fonts.css`:**
    Write @font-face declarations for each downloaded font. Preserve the exact `unicode-range` values from the original Google Fonts CSS for each subset. Use `font-display: swap`. Use relative paths from `assets/css/` to `assets/fonts/`, e.g., `url("../fonts/ibm-plex-mono-400-latin.woff2")`.

    Example structure for one entry:
    ```css
    /* latin-ext */
    @font-face {
      font-family: 'IBM Plex Mono';
      font-style: normal;
      font-weight: 400;
      font-display: swap;
      src: url("../fonts/ibm-plex-mono-400-latin-ext.woff2") format("woff2");
      unicode-range: U+0100-02BA, U+02BD-02C5, ...;
    }
    /* latin */
    @font-face {
      font-family: 'IBM Plex Mono';
      font-style: normal;
      font-weight: 400;
      font-display: swap;
      src: url("../fonts/ibm-plex-mono-400-latin.woff2") format("woff2");
      unicode-range: U+0000-00FF, ...;
    }
    ```

    For IBM Plex Sans, also include `font-stretch: 100%;` (the Google Fonts CSS includes this for that family).
  </action>
  <verify>
    - Confirm all files exist: `ls assets/js/tailwind-browser.js assets/css/phosphor-duotone.css assets/css/fonts.css assets/fonts/phosphor/Phosphor-Duotone.woff2`
    - Confirm all 18 Google Font woff2 files exist: `ls assets/fonts/*.woff2 | wc -l` should be 18
    - Confirm phosphor-duotone.css references local path: `grep '../fonts/phosphor/Phosphor-Duotone.woff2' assets/css/phosphor-duotone.css`
    - Confirm fonts.css has 18 @font-face blocks: `grep -c '@font-face' assets/css/fonts.css` should be 18
    - Confirm no external URLs remain in local CSS files: `grep -r 'https://' assets/css/` should return nothing
  </verify>
  <done>All external resources (Tailwind JS, Phosphor CSS + font, Google Fonts woff2 files + CSS) are downloaded locally under assets/ with correct relative paths in CSS files.</done>
</task>

<task type="auto">
  <name>Task 2: Update index.html to reference local resources</name>
  <files>index.html</files>
  <action>
    Edit lines 56-61 of index.html to replace all external resource references with local ones:

    **Remove these lines entirely:**
    - Line 58: `<link rel="preconnect" href="https://fonts.googleapis.com">`
    - Line 59: `<link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>`
    - Line 60: `<link href="https://fonts.googleapis.com/css2?family=..." rel="stylesheet">`

    **Replace with local references:**
    - Line 57: Change `<script src="https://cdn.jsdelivr.net/npm/@tailwindcss/browser@4"></script>` to `<script src="assets/js/tailwind-browser.js"></script>`
    - Line 61: Change `<link rel="stylesheet" type="text/css" href="https://cdn.jsdelivr.net/npm/@phosphor-icons/web@2.1.2/src/duotone/style.css">` to `<link rel="stylesheet" type="text/css" href="assets/css/phosphor-duotone.css">`
    - Add a new link for fonts CSS: `<link rel="stylesheet" href="assets/css/fonts.css">`

    The resulting external resources block should look like:
    ```html
    <!-- Local Resources (previously CDN) -->
    <script src="assets/js/tailwind-browser.js"></script>
    <link rel="stylesheet" href="assets/css/fonts.css">
    <link rel="stylesheet" type="text/css" href="assets/css/phosphor-duotone.css">
    ```

    **Do NOT touch any other part of index.html.** Specifically, do not modify:
    - Meta tag URLs (og:url, canonical, twitter:url — these are metadata, not resource fetches)
    - The Creative Commons link (just a hyperlink)
    - SVG namespace declarations
    - Any inline CSS or JS
  </action>
  <verify>
    - Confirm no external resource fetches remain: `grep -n 'cdn.jsdelivr\|fonts.googleapis\|fonts.gstatic' index.html` should return nothing
    - Confirm local references exist: `grep 'assets/js/tailwind-browser.js' index.html` and `grep 'assets/css/phosphor-duotone.css' index.html` and `grep 'assets/css/fonts.css' index.html` all return matches
    - Confirm meta URLs are untouched: `grep 'openburo.eu' index.html` still returns canonical/og/twitter URLs
    - Open index.html via a local HTTP server (`python3 -m http.server 8080`) and verify in browser DevTools Network tab that zero requests go to external domains. Alternatively, grep index.html for any remaining `https://` in `src=` or `href=` attributes that are not meta/link[rel=canonical]/a tags: should only find meta tags and anchor hrefs.
  </verify>
  <done>index.html references only local resources. The three CDN links and two preconnect links are replaced with three local file references. All meta tag URLs remain unchanged.</done>
</task>

</tasks>

<verification>
1. All files downloaded and present under assets/
2. index.html contains zero CDN/external resource references (src= and stylesheet href= attributes)
3. Local CSS files contain no external URLs — all paths are relative
4. Font-face declarations in fonts.css cover all 3 families and all required weights
5. Phosphor Icons CSS correctly references local woff2 font file
6. Page renders correctly when served locally (fonts load, icons display, Tailwind styles apply)
</verification>

<success_criteria>
- `grep -rn 'https://' index.html` returns ONLY meta tags (og:url, canonical, twitter:url, og:image), the Creative Commons anchor href, and no resource-loading tags (script src, link[rel=stylesheet] href)
- All 18 Google Font woff2 files exist in assets/fonts/
- Phosphor-Duotone.woff2 exists in assets/fonts/phosphor/
- tailwind-browser.js exists in assets/js/
- fonts.css and phosphor-duotone.css exist in assets/css/
- Page loads and renders correctly from local HTTP server with no external network requests
</success_criteria>

<output>
After completion, create `.planning/quick/2-move-all-external-resources-locally-no-o/2-SUMMARY.md`
</output>
