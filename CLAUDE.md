# CLAUDE.md

Guidance for maintaining the AriNutpam marketing site.

## What this is

The public marketing site for **AriNutpam** (legal name *Arinutpam Private Limited*), an
AI engineering & software consultancy in Coimbatore, India. It is a single-page landing
site — `index.html` — with content sections (services, about, projects, customers, AI
demo, blog, contact).

## Tech stack — keep it simple

- **Plain HTML + CSS + JS. No framework, no build step, no npm/node.** This is deliberate;
  the owner is non-technical and wants a zero-dependency site. Do **not** introduce React,
  bundlers, Tailwind, or a build pipeline unless explicitly asked.
- Edits are made directly to the source files and pushed; GitHub Pages serves them as-is.

## Layout

```
index.html            Single-page site (all content sections live here)
logo-preview.html     Dev-only logo gallery — not linked from the site
CNAME                 Custom domain for GitHub Pages (= arinutpam.ai)
robots.txt            Allows all crawlers; points to sitemap
sitemap.xml           Lists the homepage (update lastmod on meaningful changes)
favicon.ico           Root favicon (Google reads this)
css/
  variables.css       Design tokens (colors, spacing) — start here for theme changes
  reset.css           Reset/normalize
  layout.css          Page scaffolding / containers
  components.css      Buttons, cards, nav, form controls
  sections.css        Per-section styles (hero, about, projects, etc.)
  animations.css      Keyframes / reveal animations
  responsive.css      Media queries — keep responsive rules here
js/
  main.js             Theme toggle, nav, scroll reveals, stat counters
  neural-bg.js        Hero neural-network canvas animation (theme-aware)
  demo.js             In-browser AI demo (TensorFlow.js + MobileNet)
  contact.js          Contact form handling (Web3Forms)
assets/
  images/             favicon.svg, og-image (referenced but MISSING — see Gotchas), etc.
  arinutpam-logo-v18.svg   Current wordmark (v1–v17 are unused old versions)
  fonts/ icons/ video/
PoC/                  Proof-of-concept demo media (images/video). Brands must be blurred.
```

The 7 CSS files are loaded in order in `<head>`; `index.html` section anchors are
`#hero #services #about #projects #customers #demo #blog #contact`.

## Running / previewing locally

No build. Either open `index.html` directly, or for correct root-relative paths and the
AI demo (which fetches assets), serve it:

```
python -m http.server 8000      # then open http://localhost:8000
```

## Deployment

- **Hosting:** GitHub Pages, org `AriNutpam-Technologies`, repo `arinutpam-site`,
  served from the **`master`** branch root.
- **Deploy = push to `master`.** Pages rebuilds automatically in ~1–2 min. There is no
  staging environment; pushes go straight to production.
- `CNAME` must stay `arinutpam.ai` — do not delete/change it or the custom domain breaks.

## Domains & DNS (important context)

- **Primary domain: `https://arinutpam.ai`** (canonical everywhere — schema, OG, sitemap).
- DNS for `arinutpam.ai` is on **Cloudflare** → GitHub Pages (4 A records
  `185.199.108–111.153`, **DNS-only / grey cloud** so GitHub can issue HTTPS).
- **`arinutpam.in` 301-redirects to `arinutpam.ai`** via a Cloudflare Redirect Rule scoped
  to `(http.host eq "arinutpam.in") or (http.host eq "www.arinutpam.in")`. Those two hosts
  are proxied (orange). **Do not broaden** the rule — `arinutpam.in` also serves email
  (Microsoft 365) and MLOps tunnel subdomains (`cvat`, `fiftyone`, `minio`, `mlops`) that
  must NOT be redirected.

## Conventions

- **Brand name is `AriNutpam`** (mixed case) — never `ARINutpam` or `Arinutpam`. The legal
  name `Arinutpam Private Limited` stays as-is only in the schema's `legalName`.
- **Colors:** green `#10b981` + purple `#8b5cf6` accents. Change theme tokens in
  `css/variables.css`, not scattered across files.
- **Phone numbers:** always `+91` prefix, never a leading `0`.
- **No customer/client names** in public content. Blur brand marks in PoC images.
- Light/dark mode is supported — verify both when changing colors.
- Keep new styles in the matching CSS file (responsive rules → `responsive.css`, etc.).

## Common maintenance tasks

- **Edit copy / sections:** all in `index.html`.
- **Update the sitemap** `lastmod` when you make a meaningful content change.
- **SEO:** title/description/keywords, Open Graph, and JSON-LD `ProfessionalService` schema
  live in `<head>`. Keep the schema description AI/software-focused (the company is wrongly
  listed as an industrial manufacturer on third-party sites; the schema helps disambiguate).
- **Favicon:** source is `assets/images/favicon.svg`; raster set (`favicon.ico`, 96/192 PNGs,
  apple-touch-icon) is generated from it — regenerate all if the mark changes.

## Gotchas

- `assets/images/og-image.jpg` is referenced in OG tags + schema but **does not exist yet** —
  create it (or fix the path) before relying on link previews.
- `js/contact.js` uses **Web3Forms**; the access key is still the placeholder
  `YOUR_ACCESS_KEY_HERE` — the form won't deliver until a real key is set.
- Old logo versions `arinutpam-logo-v1.svg`–`v17.svg` and `logo-preview.html` are unused
  and safe to clean up.
- Git on Windows warns about LF→CRLF — harmless.

## Commits

- Commit/push only when asked. **Do not** add `Co-Authored-By` trailers to commit messages.
