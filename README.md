# Fahad Iqbal — Portfolio

Personal site for **Fahad Iqbal**. One scrolling homepage plus flagship case-study pages. Built with **plain HTML, CSS, and JavaScript**. Zero dependencies, zero trackers, zero third-party scripts.

> Engineer who designs.

---

## Overview

| | |
|---|---|
| Stack | HTML5 · CSS3 · vanilla JS |
| Fonts | Self-hosted **Fraunces** + **Inter** (latin, variable, woff2) |
| Host | GitHub Pages (project site or user site) |
| Contact | Formspree (public form ID only) |
| Audience | Clients and agencies first; recruiters second |

The architecture is a deliberate security and performance choice: no packages means no supply-chain surface, no bundler, and a strict Content Security Policy that the site can actually keep.

---

## File structure

```
portfolio/
├── index.html                          homepage (13 sections + footer)
├── 404.html
├── case-studies/
│   └── portfolio-website.html          flagship case study
├── css/style.css                       one stylesheet, design tokens in :root
├── js/script.js                        one script, defer, progressive enhancement
├── assets/
│   ├── fonts/                          inter-latin.woff2 · fraunces-latin.woff2
│   ├── images/                         favicon, apple-touch, og-image.png
│   ├── icons/                          unused (icons are inline SVG)
│   └── resume/                         drop Fahad-Iqbal-Resume.pdf here
├── favicon.svg
├── site.webmanifest
├── robots.txt
├── sitemap.xml
├── README.md
└── .gitignore
```

Every HTML page links `css/style.css` and `js/script.js` with **relative** paths so the site works at `/`, at `/portfolio/`, or on a custom domain.

---

## How to edit content

Copy in `index.html` is the source of truth. Do not invent facts.

| Block | Where |
|---|---|
| Identity, hero, about | `index.html` — sections `#hero`, `#about` |
| Capabilities (four tiers) | `#capabilities` |
| Selected work | `#work` — project cards. Flagships also get a file in `case-studies/` |
| Creative gallery | `#creative` — see PLACEHOLDER-07 |
| GitHub repos | `#github` — static cards, no live API |
| Experience / education / certs | `#experience` `#education` `#certifications` |
| Process / exploring / resume | `#process` `#exploring` `#resume-section` |
| Contact form + email | `#contact` |
| Case study body | `case-studies/portfolio-website.html` |
| Colour, type, space | `css/style.css` — `:root` tokens |
| Behaviour | `js/script.js` — labelled sections 01–12 |

### Adding a project

1. Duplicate the Project 1 card in `#project-grid`.
2. Fill every field of the template in the HTML comment above Projects 2–5. Use `[TO BE DOCUMENTED]` rather than inventing.
3. Set `data-published="true"` and remove `data-pending`.
4. If it is a flagship, add `case-studies/<slug>.html` from the Portfolio Website page as a template, then add prev/next links once two flagships exist.
5. Tags must be one or more of: `development` `cybersecurity` `ai` `creative` (lowercase in `data-tags`).

### Adding creative work

Replace a pending tile in `#creative` with a `<button class="gallery-item">` that carries `data-gallery-src`, `data-gallery-title`, `data-gallery-meta`, and `data-gallery-alt`. Mix `data-ratio="4:3"` and `data-ratio="1:1"`. Real exports only. Meaningful alt text.

### Build state (dev vs launch)

The site ships in **development state** so unfilled slots are visible and obviously unfinished.

```html
<body class="no-js" data-build="dev">
```

Before public launch, switch every page to:

```html
<body class="no-js" data-build="launch">
```

That hides every `[data-pending]` node (projects 2–5, gallery tiles, repo placeholders, LinkedIn pending labels, resume-pending notes). Fill or delete those nodes first.

---

## Placeholder register

Fill in this order. Never ship a clickable URL that 404s.

| ID | Item | Status in this build | How to fill |
|---|---|---|---|
| **01** | Domain / canonical | `https://YOUR-DOMAIN` in canonical, OG, JSON-LD, robots, sitemap | Buy domain → replace every `YOUR-DOMAIN` → add a `CNAME` file |
| **02** | Formspree endpoint | `https://formspree.io/f/FORM_ID` | Create a form at formspree.io, paste the id. Until then the JS submit path refuses to POST and asks the visitor to email directly |
| **03** | Resume PDF | missing | See `assets/resume/README.md` |
| **04** | LinkedIn | omitted | Verify `https://www.linkedin.com/in/fahadiqbal-z`. If it 404s, leave LinkedIn out. If it works: add the link in header/footer/contact + JSON-LD `sameAs` |
| **05** | Featured GitHub repos | 3 pending slots | Real names, one-liners, language, URL under `fahadiqbal-z`. Never invent a repo |
| **06** | Projects 2–5 | empty templates, hidden at launch | Fill the template; do not write flavour copy |
| **07** | Creative gallery | 6 pending tiles | 6–12 real works + alt text |
| **08** | OG image | draft at `assets/images/og-image.png` (1200×630, spec followed) | Re-export if the composition should include a real work still |
| **09** | Project 1 “to be documented” | hardest challenge, results, learned, extra tech, screenshots | Update the case study after a real pass of use |
| **10** | Optional CGPA line | commented out in `#education` | Uncomment only if you want it public |

---

## Deploy (GitHub Pages)

1. Create a repo (user site `fahadiqbal-z.github.io`, or project site e.g. `portfolio`).
2. Push this folder to `main`. If the repo is a project site, either put these files at the repo root or set Pages to the subfolder — relative URLs already work either way.
3. GitHub → **Settings → Pages** → Source: `main` `/ (root)` → save.
4. Enable HTTPS (default).
5. After the first deploy: open the live origin, submit a test form (Formspree must allow that origin), confirm fonts load, confirm no mixed content.

Custom domain later:

1. Add a `CNAME` file containing the domain.
2. Replace every `https://YOUR-DOMAIN`.
3. Point DNS. Confirm HTTPS.

GitHub Pages will serve `404.html` for unknown paths on user/org sites. Project sites need a `404.html` at the repo root — this file is already there.

No CI, no environment files, no secrets.

---

## Security rationale

- **Zero npm packages.** No `package.json`, no lockfile, no `node_modules`. The attack surface is the three files you can read.
- **No secrets in the frontend.** The Formspree form id is a public endpoint identifier — the only third-party token allowed.
- **CSP** (meta, because GitHub Pages cannot set headers):

  `default-src 'self'; img-src 'self' data:; style-src 'self'; script-src 'self' + JSON-LD hash; font-src 'self'; form-action https://formspree.io; connect-src https://formspree.io; base-uri 'self'; frame-ancestors 'none'; object-src 'none'`

- Honeypot field `company` on the contact form.
- External links: `rel="noopener noreferrer"` + `target="_blank"`.
- Referrer-Policy: `strict-origin-when-cross-origin`.
- Nothing is written with `innerHTML`. Dynamic strings use `textContent`.

---

## Pre-launch checklist (§30)

**Content**
- [ ] No invented facts; every remaining `[PLACEHOLDER]` / `[TO BE DOCUMENTED]` is either filled or hidden
- [ ] `data-build="launch"` on every page
- [ ] Certifications still say “none yet” unless a real cert is earned
- [ ] LinkedIn verified or omitted
- [ ] Formspree ID real, resume PDF real, domain real

**Design**
- [ ] Accent used only as specified (hero period, chips, links, ticks, primary buttons, focus)
- [ ] No gradients, glass, neon, rounded-card soup

**Functional**
- [ ] Nav, scrollspy, drawer, filters, copy-email, form states, case-study back link
- [ ] Search only appears if there are 8+ published projects
- [ ] No clickable placeholder URLs

**Responsive / a11y / perf**
- [ ] 320px–1920px, no horizontal scroll, tap targets ≥ 44px
- [ ] Keyboard walkthrough; focus visible; drawer and lightbox escapable
- [ ] `prefers-reduced-motion` — everything visible, no motion
- [ ] Lighthouse Performance / SEO / Accessibility ≥ 95 on mobile throttle

**Security**
- [ ] CSP console clean with the real form endpoint
- [ ] Malicious form input (`<script>`, 10k chars) does not execute
- [ ] Still no `package.json`

---

## Local preview

Any static server from this folder:

```bash
python3 -m http.server 8080
```

Open `/index.html`, `/case-studies/portfolio-website.html`, and a nonsense path to see `/404.html` (the 404 page itself is also at `/404.html`).

JavaScript off: content, nav, mailto, and native form POST still work. Filtering, lightbox, copy-email, and the fetch upgrade need JS.

---

## Licence

Site content © Fahad Iqbal. Fraunces and Inter are SIL Open Font License.
