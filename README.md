# Karan Chauhan — Portfolio

Personal portfolio for **Karan Chauhan**, a Full Stack Engineer. The design pairs warm "parchment"
paper and sepia ink with a single red accent and classic serif type.

The entire site is **one self-contained file**: [`index.html`](./index.html). All CSS and JS are
inlined. There is **no build step, no framework, and no backend.**

---

## Table of contents

1. [Tech stack](#tech-stack)
2. [Files in this folder](#files-in-this-folder)
3. [Running & deploying](#running--deploying)
4. [Colors](#colors)
5. [Typography](#typography)
6. [Layout & spacing tokens](#layout--spacing-tokens)
7. [Page structure](#page-structure)
8. [How to modify common things](#how-to-modify-common-things)
9. [JavaScript behaviors](#javascript-behaviors)
10. [Responsive breakpoints](#responsive-breakpoints)
11. [Accessibility](#accessibility)
12. [Notes](#notes)

---

## Tech stack

| Layer | Choice | Notes |
|---|---|---|
| Markup | Plain HTML5 | Single file, semantic sections |
| Styles | Plain CSS (inlined in `<style>`) | CSS custom properties (variables) for the whole theme |
| Behavior | Vanilla JavaScript (inlined in `<script>`) | No libraries |
| Fonts | Google Fonts (CDN `<link>`) | The only external dependency |
| Build | **None** | Open the file directly |
| Hosting | Any static host | It's just one HTML file |

The only thing fetched over the network is the Google Fonts stylesheet. With no internet the page
still works — it falls back to the system fonts declared in each font variable (Georgia / system
serif / system mono).

---

## Files in this folder

| File | Purpose |
|---|---|
| `index.html` | **The whole site** — markup + inlined CSS + inlined JS |
| `Karan-Chauhan-Resume.pdf` | The file the "Download Résumé" button serves |
| `favicon.ico`, `favicon.svg`, `favicon-96x96.png`, `apple-touch-icon.png` | Icons |
| `web-app-manifest-192x192.png`, `web-app-manifest-512x512.png` | PWA icons referenced by the manifest |
| `site.webmanifest` | Web app manifest |
| `robots.txt`, `sitemap.xml` | Search-engine files |
| `vercel.json` | Hosting config |
| `README.md` | This document |

---

## Running & deploying

**Just open it:** double-click `index.html`, or drag it into a browser.

**Optional local server** (only needed if a browser blocks something over `file://`):

```bash
python -m http.server 8000
# then visit http://localhost:8000
```

**Deploy:** upload the folder to any static host (GitHub Pages, Vercel, Netlify, Cloudflare Pages,
S3, etc.). No configuration required.

---

## Colors

All colors live as CSS variables in `:root` at the top of the `<style>` block. **Change a color
once there and it updates everywhere.** The palette is intentionally restrained: paper tones + ink +
a single red accent.

| Variable | Hex | Role |
|---|---|---|
| `--paper` | `#f2e9d8` | Page background |
| `--panel` | `#e9dec7` | Inset boxes (cards, drawer-active) |
| `--panel-2` | `#e2d4b8` | Deeper inset (rarely used) |
| `--ink` | `#241d16` | Primary text / borders |
| `--ink-soft` | `#6b6052` | Secondary text |
| `--ink-faint` | `#6f6149` | Captions, meta, muted labels |
| `--rule` | `#d6c8ab` | Hairline dividers |
| `--rule-bold` | `#b8a07a` | Stronger dividers, drop-shadows |
| `--rubric` | `#a8331f` | **The one accent — red.** Links, highlights, active states |
| `--rubric-dk` | `#9e2f1f` | Darker red (used for button shadows) |

**Why red?** It's a nod to *rubrication* — the manuscript practice of marking important text in red
ink. It's the only accent on the page; keep it that way to preserve the look.

---

## Typography

Four font variables, each with a clear job. They're declared in `:root` and loaded via one Google
Fonts link in `<head>`.

| Variable | Font | Used for | Fallback |
|---|---|---|---|
| `--display` | **IM Fell English** | Big headings, name, project titles | Georgia, serif |
| `--sc` | **IM Fell English SC** (small caps) | Labels, pills, tags, nav | Georgia, serif |
| `--body` | **EB Garamond** | Body copy, descriptions, the lede | Georgia, serif |
| `--mono` | **JetBrains Mono** | Dates, stack tags, system notes, colophon | ui-monospace, monospace |

**The Google Fonts link (in `<head>`):**

```html
<link href="https://fonts.googleapis.com/css2?family=EB+Garamond:ital,wght@0,400;0,500;0,600;1,400&family=IM+Fell+English:ital@0;1&family=IM+Fell+English+SC&family=JetBrains+Mono:wght@400;500;700&display=swap" rel="stylesheet">
```

If you add a new font weight/style, update **both** this link (so it's downloaded) and the relevant
CSS. To swap a typeface entirely, change its variable value in `:root` and adjust the link.

**Design intent:** the antique IM Fell faces carry personality; EB Garamond keeps long text
readable; JetBrains Mono reads as the "system/data" voice (dates, stack tags, colophon).

---

## Layout & spacing tokens

| Variable | Value | Meaning |
|---|---|---|
| `--col` | `880px` | Max content width (the centered column) |
| `--navh` | `58px` | Fixed navbar height (body is padded by this; sections use it for `scroll-margin-top`) |
| `--ease` | `cubic-bezier(0.22, 1, 0.36, 1)` | Shared easing for all transitions |

The centered column is the `.sheet` wrapper. Section vertical rhythm is set on `section { padding }`
and per-block rules — see [Notes](#notes) about not letting section paddings fight.

---

## Page structure

Top to bottom, with the `id` used for navigation/anchors:

| Order | Section | `id` | Heading |
|---|---|---|---|
| — | Navbar (fixed) | — | brand + links |
| 1 | Masthead | `top` | Name + titles + lede + résumé button |
| 2 | Skills | `skills` | **Skills** |
| 3 | Projects | `projects` | **Projects** |
| 4 | Experience | `experience` | **Experience** |
| 5 | Achievements | `achievements` | **Achievements** |
| 6 | Contact | `contact` | **Contact** |
| — | Footer (colophon) | — | — |
| — | Back-to-top button | `toTop` | — |

Each section header has two parts: a big **`<h2>`** and an italic **`.plain`** one-line description.

---

## How to modify common things

### Add a project

Projects live in the `#projects` section, split into two `.project-group` blocks (**Web & App
Builds** and **Games**). Copy an existing `<article class="project">` into the right group and edit
it:

```html
<article class="project">
    <h4 class="project-title">Project Name</h4>
    <p class="project-stack">Tech · Tags · Here</p>
    <p class="project-desc">One or two sentences on what it does and the impact.</p>
    <a class="project-link" href="https://your-link/" target="_blank" rel="noopener">View project →</a>
</article>
```

- Update the category **count** in that group's `.cat-head` (`<span class="cat-count">5</span>`).
- There are **no per-item numbers** to maintain — order is just DOM order within the group.

### Add a project category

Duplicate a whole `.project-group` (the `.cat-head` + its `<article>`s):

```html
<div class="project-group">
    <div class="cat-head">
        <h3 class="cat-name">Category Name</h3>
        <span class="cat-rule"></span>
        <span class="cat-count">N</span>
    </div>
    <!-- articles... -->
</div>
```

### Edit the skills

In `#skills`, the `.skills-grid` holds one `.skill-group` per category. Each group has an `<h3>`
label and a `.items` line listing the technologies:

```html
<div class="skill-group"><h3>Languages</h3><p class="items">TypeScript · JavaScript · Python</p></div>
```

### Edit experience / education

In `#experience`, each `.timeline-entry` is one role. Edit `.when` (date + context), `<h3>` (title),
`.org` (company/school), and the `<ul><li>` bullets. Keep bullets **impact-first** (lead with the
outcome, not the task).

### Edit achievements

In `#achievements`, each `<li>` has an `.achievement-kind` label (Award / Publication) and an
`.achievement-text` line with the actual achievement.

### Edit contact details

In `#contact`: the `.email` link, the `.channels` links (LinkedIn, GitHub, phone), and the location
span. Update both the `href` and the visible text.

### Change navigation

Nav links live in `<ul class="nav-links">`. Each `href="#id"` must match a section `id`:

```html
<li><a href="#newid">Label</a></li>
```

If you add a section, also add its `id` to the page — the **scroll-spy** auto-detects from the nav
links, so as long as the `href` matches a section `id`, the active-link highlight just works.

### The résumé download button

The masthead has a **Download Résumé** button (`.resume-btn` in the `#top` header). It's a plain
link with the `download` attribute:

```html
<a class="resume-btn" href="Karan-Chauhan-Resume.pdf" download="Karan-Chauhan-Resume.pdf" aria-label="Download résumé (PDF)">
    <span>Download Résumé</span>
    <span class="rb-ic" aria-hidden="true">↓</span>
</a>
```

- `href` is the file it fetches; `download` is the filename the visitor's browser saves it as.
- If you'd rather link a hosted/Drive copy, change `href` to that URL and **remove** the `download`
  attribute (cross-origin URLs ignore `download` and will open instead of saving).

### Change the masthead intro

Edit `.intro` (the italic lede about what you do) in the `#top` header.

---

## JavaScript behaviors

All JS is inlined at the bottom of `index.html`. Each block is small and self-contained:

| Behavior | What it does |
|---|---|
| Nav drawer (`setMenu`) | Hamburger toggles the mobile drawer; manages the scrim, `aria-expanded`, body scroll-lock (`body.nav-open`), closes on link tap / scrim click / Escape / resize > 900px. |
| Scroll-spy | `IntersectionObserver` highlights the nav link for the section in view. Auto-built from the nav links' `href`s. |
| Reveal on scroll | `IntersectionObserver` fades `.reveal` sections in once. |
| Back-to-top | `#toTop` appears after 400px of scroll; smooth-scrolls up (instant if reduced-motion); hides while the mobile drawer is open. |

---

## Responsive breakpoints

| Max width | What changes |
|---|---|
| `1080px` (down to 901) | Navbar link gaps/size tighten so all links still fit |
| `900px` | **Navbar collapses to a hamburger + slide-in drawer**; back-to-top tucks to 16px margins |
| `760px` | Masthead ribbon stacks centered; body font slightly smaller |
| `480px` | Back-to-top shrinks to a 42px tap target |
| `460px` | Project rows stack the title/stack vertically |

---

## Accessibility

Already built in — preserve these if you edit:

- **Skip link** (`.skip-link`) for keyboard users, jumping to content.
- **Visible focus rings** (`:focus-visible` → red outline).
- Hamburger uses `aria-expanded` / `aria-controls` / dynamic `aria-label`.
- **`prefers-reduced-motion`** disables animations, reveals, and smooth scroll.
- Decorative SVGs are `aria-hidden`; links that open new tabs use `rel="noopener"`.

---

## Notes

- **`color-mix()`** is used once (the navbar's translucent background). Modern browsers support it;
  on very old browsers it degrades. Swap for an `rgba()` if you need to support legacy browsers.
- **One accent only.** The design's discipline comes from using `--rubric` (red) as the *single*
  accent. Adding a second accent color will dilute the look — change `--rubric` instead.
- **Section paddings:** when adding sections, reuse the existing `section` / `.block` rhythm rather
  than adding competing top+bottom margins, to avoid doubled gaps.

---

_Single-file build. No dependencies beyond Google Fonts. Edit `index.html` and refresh._
