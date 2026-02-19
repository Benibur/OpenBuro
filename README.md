# Open Buro

**The open European standard for workplace orchestration.**

Open Buro is the missing layer that turns isolated open source workplace apps into a unified digital platform — capable of rivaling Microsoft 365 and Google Workspace, without vendor lock-in.

## The Problem

Europe has mature open source alternatives for every workplace function: email, documents, project management, video conferencing, chat, calendar. But these tools remain isolated silos. Even with SSO, users get an app catalog — not a platform.

Meanwhile, dominant suites win not because they're better app-by-app, but because they deliver a **platform effect**: an interlocked ecosystem where everything flows together effortlessly. Leaving becomes perceived as risk, not a project. The dependency is strategic and political, not just budgetary.

## The Standard

Open Buro defines how independent open source services assemble and communicate through a common orchestration layer. The standard covers 7 domains:

- **Application Integration** — Unified SSO, standard app packaging, centralized registry, common settings API
- **Cross-service Navigation** — Shared home screen, unified nav, global app grid, cross-app command palette
- **Data Intelligence** — Business object definitions, cross-app event streaming, knowledge graph, unified search
- **Platform Collaboration** — Cross-service workspaces, threaded comments, unified notifications, shared presence
- **Inter-apps & AI** — Capability/intent casting, shared file picker, AI agents orchestrating across tools
- **Security & Encryption** — Platform-level E2E encryption, granular permissions, audit logging
- **Mobile & Desktop** — Native mobile apps, desktop client, browser extension

## The Alliance

The Open Buro Alliance is the collective movement — publishers, institutions, governments — that governs and promotes the standard. Structured as a neutral foundation (modeled on Linux Foundation / CNCF), no single vendor controls the standard.

**Founding members:**

- [Twake](https://twake.app/) (LINAGORA's collaborative platform)
- [La Suite numérique](https://lasuite.numerique.gouv.fr/) (DINUM, French government digital workplace)

We are at the very beginning, join us !

## Project Structure

```
index.html          # Landing page (single-file, production-ready)
assets/
  css/              # Local fonts & icons stylesheets
  fonts/            # Google Fonts (woff2) & Phosphor Icons
  js/               # Tailwind CSS browser runtime
news/               # News article pages
pages/              # Static sub-pages (manifesto, governance)
```

## Development

No build step required. Open `index.html` in a browser:

```bash
# With Python
python3 -m http.server 8000

# With Node
npx serve .
```

All resources are served locally — zero external HTTP requests at runtime.

## Tech Stack

- **Tailwind CSS v4** (browser runtime, served locally)
- **Vanilla JS** — Canvas particle animation, Intersection Observer scroll effects, accordion interactions
- **Google Fonts** (IBM Plex Sans, IBM Plex Mono, Plus Jakarta Sans — served locally)
- **Phosphor Icons** (duotone, served locally)

## Design

- Dark mode with constellation/network visual motif
- Mobile-first responsive design (breakpoints: 768px, 1024px, 1440px)
- Scroll-triggered animations (fade-in, stagger, SVG draw-in)
- RGPD-compliant by design: no cookies, no tracking, no analytics

## License

Content licensed under [CC BY-SA 4.0](https://creativecommons.org/licenses/by-sa/4.0/).

## Contact

- Website: [openburo.eu](https://open-buro.eu)
- Email: hello@openburo.eu
