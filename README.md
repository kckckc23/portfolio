# Karan Chauhan — Portfolio

A personal portfolio for **Karan Chauhan** (Software Engineer & Game Designer), built as a
**fine-press Dungeons & Dragons character sheet**: warm "parchment" paper, sepia ink, a single
red "rubric" accent, antique display type, and playful D&D framing kept *secondary* to plain,
recruiter-friendly headings.

The entire site is **one self-contained file**: [`index.html`](./index.html). All CSS and JS are
inlined. There is **no build step, no framework, and no backend.**

---

## Table of contents

1. [Tech stack](#tech-stack)
2. [Files in this folder](#files-in-this-folder)
3. [Running & deploying](#running--deploying)
4. [Brand kit — colors](#brand-kit--colors)
5. [Brand kit — typography](#brand-kit--typography)
6. [Layout & spacing tokens](#layout--spacing-tokens)
7. [Page structure](#page-structure)
8. [The D&D → plain-English naming map](#the-dd--plain-english-naming-map)
9. [How to modify common things](#how-to-modify-common-things)
10. [JavaScript behaviors](#javascript-behaviors)
11. [Responsive breakpoints](#responsive-breakpoints)
12. [Accessibility](#accessibility)
13. [Gotchas & notes](#gotchas--notes)

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

> Note: the **PDF Tools Workspace** project listed on the page uses `pdf-lib` + WebAssembly — but
> that's a *separate* project that's only linked to. This portfolio itself ships none of that.

---

## Files in this folder

| File | Status | Purpose |
|---|---|---|
| `index.html` | **Active — this is the whole site** | Markup + inlined CSS + inlined JS |
| `resume.pdf` | **You provide this** | The file the "Download Résumé" button serves (see below) |
| `README.md` | This document | — |

---

## Running & deploying

**Just open it:** double-click `index.html`, or drag it into a browser.

**Optional local server** (only needed if a browser blocks something over `file://`):

```bash
# from c:\GoingMerry\experiments
python -m http.server 8000
# then visit http://localhost:8000
```

**Deploy:** upload `index.html` to any static host (GitHub Pages, Vercel, Netlify, Cloudflare
Pages, S3, etc.). No configuration required. If you want a clean repo, you can ship *only*
`index.html`.

---

## Brand kit — colors

All colors live as CSS variables in `:root` at the top of the `<style>` block. **Change a color
once there and it updates everywhere.** The palette is intentionally restrained: parchment tones +
ink + a single red accent.

| Variable | Hex | Role |
|---|---|---|
| `--paper` | `#f2e9d8` | Page background (parchment) |
| `--panel` | `#e9dec7` | Inset boxes (stat blocks, cards, drawer-active) |
| `--panel-2` | `#e2d4b8` | Deeper inset (rarely used) |
| `--ink` | `#241d16` | Primary text / borders (sepia black) |
| `--ink-soft` | `#6b6052` | Secondary text |
| `--ink-faint` | `#a1937c` | Captions, meta, muted labels |
| `--rule` | `#d6c8ab` | Hairline dividers |
| `--rule-bold` | `#b8a07a` | Stronger dividers, drop-shadows |
| `--rubric` | `#c8412b` | **The one accent — red.** Links, highlights, active states |
| `--rubric-dk` | `#9e2f1f` | Darker red (available, used sparingly) |

**Why red?** It's a deliberate nod to *rubrication* — the medieval manuscript practice of marking
important text in red ink. It's the only accent on the page; keep it that way to preserve the look.

---

## Brand kit — typography

Three typefaces, each with a clear job. They're declared as variables and loaded via one Google
Fonts link in `<head>`.

| Variable | Font | Used for | Fallback |
|---|---|---|---|
| `--display` | **IM Fell English** | Big headings, name, project titles, the d20 number | Georgia, serif |
| `--sc` | **IM Fell English SC** (small caps) | Eyebrows, pills, tags, nav, "reward" links | Georgia, serif |
| `--body` | **EB Garamond** | Body copy, descriptions, the lede | Georgia, serif |
| `--mono` | **JetBrains Mono** | Stats, dates, dice notation, system notes, colophon | ui-monospace, monospace |

**The Google Fonts link (in `<head>`):**

```html
<link href="https://fonts.googleapis.com/css2?family=EB+Garamond:ital,wght@0,400;0,500;0,600;1,400&family=IM+Fell+English:ital@0;1&family=IM+Fell+English+SC&family=JetBrains+Mono:wght@400;500;700&display=swap" rel="stylesheet">
```

If you add a new font weight/style, update **both** this link (so it's downloaded) and the relevant
CSS. To swap a typeface entirely, change its `--variable` value in `:root` and adjust the link.

**Design intent:** the antique IM Fell faces carry personality; EB Garamond keeps long text
readable; JetBrains Mono reads as the "system/data" voice (stats, dates, the decoder note).

---

## Layout & spacing tokens

| Variable | Value | Meaning |
|---|---|---|
| `--col` | `880px` | Max content width (the centered column) |
| `--navh` | `58px` | Fixed navbar height (body is padded by this; sections use it for `scroll-margin-top`) |
| `--ease` | `cubic-bezier(0.22, 1, 0.36, 1)` | Shared easing for all transitions |

The centered column is the `.sheet` wrapper. Section vertical rhythm is set on `section { padding }`
and per-block rules — see [Gotchas](#gotchas--notes) about not letting section paddings fight.

---

## Page structure

Top to bottom, with the `id` used for navigation/anchors:

| Order | Section | `id` | Themed name (eyebrow) | Plain name (heading) |
|---|---|---|---|---|
| — | Navbar (fixed) | — | — | brand + links |
| 1 | Masthead | `top` | Player's Handbook / Character Sheet | Name + titles + lede + decoder |
| 2 | Skills stat block | `scores` | tag: *ability scores* | **Top Skills** |
| 3 | Projects | `quests` | The Campaign Log | **Projects** |
| 4 | Experience | `log` | The Adventure Log | **Experience** |
| 5 | Tools | `spellbook` | Spellbook | **Tools & Tech** |
| 6 | Achievements | `loot` | The Trophy Hall | **Achievements** |
| 7 | Contact | `raven` | Send a Raven | **Contact** |
| — | Footer (colophon) | — | — | — |
| — | Back-to-top button | `toTop` | — | — |

Each section header has three parts: a small red **eyebrow** (the D&D flourish), a big **`<h2>`**
(the plain name normies read), and an italic **`.plain`** one-line description.

---

## The D&D → plain-English naming map

The page deliberately keeps plain words *prominent* and D&D words *secondary* so non-gamers
understand it instantly. If you add content, follow the same pattern. Current mapping:

| D&D term (flourish) | Plain meaning (what's shown big) |
|---|---|
| Ability Scores | Top Skills (rated; higher number = stronger) |
| The Campaign Log / Quests | Projects |
| Main Quests | Web & App builds |
| Side-Quests | Games |
| The Adventure Log / XP | Experience (work + education) |
| Spellbook / Proficiencies | Tools & Tech |
| The Trophy Hall / Loot & Trophies | Achievements |
| Send a Raven | Contact |
| Active Conditions | A humorous "status effects" block (anime/brainrot joke) |
| Claim reward / Enter dungeon | "View / play this project" links |
| Sealed scroll | A private project (no public link) |

In the navbar, the plain word is the visible label and the D&D term rides along as a hover
`title` tooltip and as a gloss inside the mobile drawer.

---

## How to modify common things

### Add a project
Projects live in the `#quests` section, split into two `.quest-group` blocks (**Web & App Builds**
and **Games**). Copy an existing `<article class="quest">` into the right group and edit it:

```html
<article class="quest">
    <span class="qtitle">Project Name</span>
    <span class="qtype">Tech · Tags · Here</span>
    <p class="qdesc">One or two sentences on what it does and the impact.</p>
    <a class="qreward" href="https://your-link/" target="_blank" rel="noopener">Claim reward →</a>
</article>
```

- For a **private** project with no link, replace the `<a>` with:
  `<span class="qreward sealed">⚔ Sealed scroll — private</span>`
- Update the category **count** in that group's `.cat-head` (`<span class="cat-count">5</span>`).
- There are **no per-item numbers** to maintain — order is just DOM order within the group.

### Add a project category
Duplicate a whole `.quest-group` (the `.cat-head` + its `<article>`s):

```html
<div class="quest-group">
    <div class="cat-head">
        <h3 class="cat-name">Plain Category Name</h3>
        <span class="cat-tag">D&amp;D Flourish</span>
        <span class="cat-rule"></span>
        <span class="cat-count">N</span>
    </div>
    <!-- articles... -->
</div>
```

### Edit the skill ratings (stat block)
In `#scores`, each `.score` box is one skill. The big number is the "modifier", the small circled
number is the "score":

```html
<div class="score"><div class="ab">JavaScript</div><div class="mod">+4</div><div class="raw">18</div></div>
```

Change `.ab` (label), `.mod` (the big +N), `.raw` (the circled number). These are hand-set flavor
values — there's no formula enforced.

### Edit experience / education
In `#log`, each `.xp-entry` is one role. Edit `.when` (date + "+N Level"), `<h3>` (title),
`.org` (company/school), and the `<ul><li>` bullets. Keep bullets **impact-first** (lead with the
outcome, not the task).

### Edit achievements
In `#loot`, each `<li>` in `.loot` has a `.rarity` (Legendary / Epic / Rare — gaming flavor) and a
`.item` (the actual achievement).

### Edit the "Conditions" joke
In `#scores`, the `.conditions` block holds the anime/brainrot humor (`Chronically Online`,
`Bedrotting`, `Anime Brainrot`). Edit the `<li>` text freely.

### Edit contact details
In `#raven`: the `.email` link, the `.channels` links (LinkedIn, GitHub, phone), and the location
span. Update both the `href` and the visible text.

### Change navigation
Nav links live in `<ul class="nav-links">`. Each `href="#id"` must match a section `id`. Each link
shows a plain label + a D&D gloss span + a `title` tooltip:

```html
<li><a href="#newid" title="a.k.a. D&D Name">Plain Label<span class="nav-gloss">D&D Name</span></a></li>
```

If you add a section, also add its `id` to the page — the **scroll-spy** auto-detects from the nav
links, so as long as the `href` matches a section `id`, the active-link highlight just works.

### Change the age / level
The "Lv." labels auto-compute from a birthday. To change the birthday, edit one line in the
`setLevel()` script:

```js
const BIRTH = { y: 2005, m: 1, d: 23 };   // year, month (1-12), day
```

Both the navbar tag (`#lvlNav`) and the masthead pill (`#lvlMast`) update automatically every day.

### The résumé download button
The masthead has a **Download Résumé** button (`.resume-btn` in the `#top` header). It's a plain
link with the `download` attribute:

```html
<a class="resume-btn" href="resume.pdf" download="Karan-Chauhan-Resume.pdf" aria-label="Download résumé (PDF)">
    <span>Download Résumé</span>
    <span class="rb-ic" aria-hidden="true">↓</span>
</a>
```

- **Put your PDF in this folder named `resume.pdf`** (same folder as `index.html`). That's it — the
  button works with no other change.
- `href="resume.pdf"` is the file it fetches; `download="Karan-Chauhan-Resume.pdf"` is the filename
  the visitor's browser saves it as. Change either to taste (e.g. a different path or saved name).
- If you'd rather link a hosted/Drive copy, change `href` to that URL and **remove** the `download`
  attribute (cross-origin URLs ignore `download` and will open instead of saving).

### Change the masthead intro
Edit `.flavor` (the italic lede about what you do) and `.decoder` (the one-line "this is a portfolio
themed as a character sheet" note) in the `#top` header.

---

## JavaScript behaviors

All JS is inlined at the bottom of `index.html`. Each block is small and self-contained:

| Behavior | What it does |
|---|---|
| `setLevel()` | Computes your age from `BIRTH` vs the browser's date; writes `Lv.N` into `#lvlNav` and `#lvlMast`. Handles the "birthday hasn't happened yet this year" edge case. |
| Nav drawer (`setMenu`) | Hamburger toggles the mobile drawer; manages the scrim, `aria-expanded`, body scroll-lock (`body.nav-open`), closes on link tap / scrim click / Escape / resize > 900px. |
| Scroll-spy | `IntersectionObserver` highlights the nav link for the section in view. Auto-built from the nav links' `href`s. |
| Reveal on scroll | `IntersectionObserver` fades `.reveal` sections in once. |
| d20 roll | Click/Enter on the d20 rolls a **fair 1–20** (`Math.random`), tumbling through faces then landing on a real result with themed flavor text (nat 20 = crit red, nat 1 = grey). |
| Back-to-top | `#toTop` appears after 400px of scroll; smooth-scrolls up (instant if reduced-motion); hides while the mobile drawer is open. |

---

## Responsive breakpoints

| Max width | What changes |
|---|---|
| `1080px` (down to 901) | Navbar link gaps/size tighten so all links still fit |
| `900px` | **Navbar collapses to a hamburger + slide-in drawer**; back-to-top tucks to 16px margins |
| `760px` | Masthead ribbon stacks centered; hero & stat grids go single-column; body font slightly smaller |
| `480px` | Skill score boxes go 2-up; back-to-top shrinks to a 42px tap target |
| `460px` | Project rows stack the title/tech vertically |

---

## Accessibility

Already built in — preserve these if you edit:

- **Skip link** (`.skip-link`) for keyboard users, jumping to content.
- **Visible focus rings** (`:focus-visible` → red outline).
- Hamburger uses `aria-expanded` / `aria-controls` / dynamic `aria-label`.
- The d20 is keyboard-operable (`tabindex`, Enter/Space).
- **`prefers-reduced-motion`** disables animations, reveals, and smooth scroll.
- Decorative SVGs are `aria-hidden`; links that open new tabs use `rel="noopener"`.

---

## Gotchas & notes

- **The age uses the visitor's device clock.** A visitor with a wrong system date sees a wrong
  level. That's the trade-off for a zero-backend single-file site. The hardcoded `Lv.21` in the
  HTML is only a fallback for the instant before JS runs.
- **`color-mix()`** is used once (the navbar's translucent background). Modern browsers support it;
  on very old browsers it degrades. Swap for an `rgba()` if you need to support legacy browsers.
- **One accent only.** The design's discipline comes from using `--rubric` (red) as the *single*
  accent. Adding a second accent color will dilute the look — change `--rubric` instead.
- **Keep plain words prominent.** The big `<h2>`s are plain; D&D terms stay in the small eyebrow /
  tags / nav glosses. New content should follow that so non-gamers still understand the page.
- **Section paddings:** when adding sections, reuse the existing `section` / `.block` rhythm rather
  than adding competing top+bottom margins, to avoid doubled gaps.
- **Legacy files:** `style.css` and `script.js` are unused. Delete them unless you want the old
  version as a backup.

---

_Single-file build. No dependencies beyond Google Fonts. Edit `index.html` and refresh._