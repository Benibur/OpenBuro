---
phase: quick-4
plan: 01
subsystem: i18n-language-selector
tags: [language, dropdown, accessibility, i18n, navbar, mobile]
dependency_graph:
  requires: [quick-3]
  provides: [5-language-globe-dropdown, keyboard-accessible-lang-selector]
  affects: [index.html, navbar, mobile-drawer]
tech_stack:
  added: []
  patterns: [aria-expanded-toggle, data-lang-attribute, setLanguage-data-driven, keyboard-listbox-navigation]
key_files:
  created: []
  modified: [index.html]
decisions:
  - Globe dropdown with aria-expanded toggle replaces pill radiogroup toggle
  - DE/IT/ES use English fallback text (architecture ready for real translations)
  - data-driven setLanguage() replaces hardcoded getElementById approach
  - Mobile dropdown opens upward (bottom: calc(100% + 4px)) to prevent viewport overflow
  - click-outside handler closes all dropdowns via document.addEventListener('click')
metrics:
  duration: ~300s
  completed: 2026-02-20
  tasks_completed: 2
  files_modified: 1
---

# Quick Task 4: Replace Pill Toggle with Globe Dropdown - Summary

**One-liner:** Globe icon dropdown with 5-language support (EN/FR/DE/IT/ES) replacing binary pill toggle, with full ARIA keyboard navigation, click-outside close, and data-driven setLanguage() refactor.

## What Was Built

The binary EN|FR pill toggle in the navbar and mobile drawer was replaced with a compact globe icon dropdown supporting 5 languages. The new component uses `aria-expanded` state toggling, a `<ul role="listbox">` menu with `aria-checked` states on each option, and full keyboard navigation (Arrow keys, Escape, Enter/Space). DE, IT, and ES are English fallback placeholders ready for real translations.

## Tasks Completed

| Task | Description | Commit | Key Changes |
|------|-------------|--------|-------------|
| 1 | Replace pill toggle CSS + HTML with globe dropdown | 51664a0 | Remove `.lang-toggle` CSS, add `.lang-dropdown` system, update FOUC script for 5 langs |
| 2 | Add DE/IT/ES translations + refactor JS for dropdown behavior | 2e903ea | 3 new translation objects, refactored setLanguage(), dropdown event logic, keyboard nav |

## Key Technical Decisions

1. **Globe dropdown replaces pill radiogroup** - Scalable pattern for N languages vs. binary toggle hard-wired to EN/FR IDs.

2. **data-driven setLanguage()** - Removed hardcoded `getElementById('lang-en')` / `getElementById('lang-fr-mobile')` references. Now uses `document.querySelectorAll('.lang-dropdown')` to update all instances simultaneously.

3. **Mobile dropdown opens upward** - `.mobile-lang-toggle .lang-dropdown-menu` uses `bottom: calc(100% + 4px); top: auto` to prevent overflow on narrow viewports.

4. **English fallback for DE/IT/ES** - Translation objects are cloned EN content with distinguishing meta.title. Architecture ready for real translations to be dropped in without code changes.

5. **FOUC prevention updated** - Head script validates `savedLang` against `['en','fr','de','it','es']` array instead of using unchecked `|| 'en'` fallback.

## Verification Results

- `grep -c "lang-toggle" index.html` returns 3 (all `.mobile-lang-toggle` wrapper class - expected)
- `grep -c "lang-dropdown" index.html` returns 30 (new dropdown present throughout)
- `grep "de:" index.html` confirms DE translations at line 3667
- `grep "id=\"lang-en\"\|lang-en-mobile"` returns 0 (old hardcoded IDs fully removed)
- i18n comment updated to "Multi-language system (EN/FR/DE/IT/ES)"

## Deviations from Plan

None - plan executed exactly as written. Translation objects were compacted to multi-key lines for readability rather than expanded per-line, which is functionally identical.

## Self-Check: PASSED

- `/home/ben/Dev-local/openburo/index.html` - FOUND (modified)
- Commit 51664a0 - FOUND
- Commit 2e903ea - FOUND
- `.lang-dropdown` in index.html - FOUND (30 occurrences)
- `de:` translation object - FOUND (line 3667)
- Old `lang-en`/`lang-fr` IDs - NOT FOUND (correctly removed)
