# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project

**BuzzFam Advertising — official website.**

A marketing site for BuzzFam Advertising, a full-service advertising studio
(brand strategy, creative campaigns, media buying, social). Built as a fast,
SEO-friendly static site.

## Tech stack

- **Framework:** [Astro](https://astro.build) (v4) — static output, minimal
  client JS.
- **Language:** TypeScript (strict) for component frontmatter; `.astro`
  component files with scoped `<style>` blocks.
- **Styling:** plain CSS. Global tokens and base styles live in
  `src/layouts/Layout.astro` (`:root` custom properties like `--bf-accent`);
  component-specific styles are scoped inside each `.astro` file.
- **Package manager:** npm (`package-lock.json` is committed).
- **Node:** v22 (pinned for Netlify builds via `NODE_VERSION` in
  `netlify.toml`; matches the dev environment).

There is **no CSS framework, no React/Vue/Svelte integration, and no test
runner** yet. Don't assume any are present — add and document them here in the
same change if a task calls for one.

## Project structure

```
.
├── astro.config.mjs        # Astro config; `site` URL drives canonical links
├── netlify.toml            # Netlify build command, publish dir, Node version
├── tsconfig.json           # extends astro/tsconfigs/strict
├── package.json            # scripts + deps
├── public/                 # static assets served as-is (favicon, etc.)
│   └── favicon.svg
└── src/
    ├── env.d.ts             # references Astro's generated types
    ├── layouts/
    │   └── Layout.astro     # HTML shell, <head> meta/OG tags, global styles
    ├── components/
    │   ├── Header.astro     # sticky nav
    │   └── Footer.astro
    └── pages/
        └── index.astro      # homepage (file-based routing)
```

Astro uses **file-based routing**: a `.astro` file in `src/pages/` becomes a
route (`src/pages/about.astro` → `/about`). Anything in `public/` is served
from the site root unchanged.

The site is currently a **single page**: `index.astro` renders hero, services,
work, about, and contact sections, and the header nav links are hash anchors
(`#services`, `#work`, `#about`, `#contact`) into it. If you add real routes,
update the nav links in `Header.astro` accordingly.

## Development

```bash
npm install        # install dependencies
npm run dev        # local dev server with HMR at http://localhost:4321
npm run build      # production build to dist/
npm run preview    # serve the built dist/ locally
npm run check      # astro check — TypeScript/template diagnostics
```

Before committing structural or component changes, run `npm run build` and
`npm run check` and make sure both pass (the build is static, so a passing
build catches most breakage).

## Conventions

- **Components** are `PascalCase.astro` in `src/components/`; layouts in
  `src/layouts/`; routes in `src/pages/`.
- **Styling:** prefer scoped `<style>` in the component. Reach for the global
  `:root` tokens (`--bf-bg`, `--bf-accent`, `--bf-accent-2`, `--bf-text`,
  `--bf-text-muted`, `--bf-radius`, `--bf-max`) instead of hard-coding the
  brand palette, so the look stays consistent. Shared utility classes
  (`.container`, `.btn`, `.btn-primary`, `.btn-ghost`) are defined globally in
  `Layout.astro`.
- **Content data** for repeated UI (services, stats, nav links) is defined as
  arrays in component frontmatter and rendered with `.map()` — keep that
  pattern rather than copy-pasting markup.
- **SEO:** every page renders through `Layout.astro`, which sets `<title>`,
  meta description, canonical, and Open Graph tags from `title`/`description`
  props. Pass those props when adding a page.
- **Production domain:** `buzzfam.biz`. `astro.config.mjs` `site` and the
  contact email (`hello@buzzfam.biz`) point at it. Update both together if the
  domain ever changes.

## Deployment

- **Host:** Netlify (static). `netlify.toml` declares the build command
  (`npm run build`) and publish directory (`dist`); no Astro adapter is needed
  since the site is fully prerendered.
- **Build settings** (Netlify → Add new site → Import from Git): build command
  `npm run build`, publish directory `dist`. These are read from `netlify.toml`,
  so the dashboard defaults can be left as-is. Every push to `main` triggers a
  production deploy once the repo is connected; deploy previews are built for
  pull requests.
- **Domain:** add `buzzfam.biz` as a custom domain under Site configuration →
  Domain management and point DNS at Netlify (either via Netlify DNS or the
  provided `CNAME`/`A` records).

## Git workflow

- **Default branch:** `main`.
- **Feature branches:** `claude/<short-kebab-description>`
  (e.g. `claude/claude-md-docs-2014kj`). Develop on a feature branch; never
  commit directly to `main`.
- **Commits:** clear, imperative-mood messages; keep them focused.
- **Pushing:** `git push -u origin <branch-name>`. Don't open a pull request
  unless explicitly asked.
- **Merged PRs are final:** if a branch's PR has merged, start follow-up work
  fresh from the latest `main` rather than stacking onto merged history.

## Working agreements for assistants

- **Be honest about state.** Describe only what exists. There's no test suite
  today — say so rather than inventing one.
- **Verify before claiming done.** Run `npm run build` / `npm run check` and
  report real results.
- **Keep secrets out of git.** No API keys or `.env` files committed
  (`.gitignore` already excludes `.env*`).
- **Keep this file current.** When you change the stack, structure, scripts,
  conventions, or add deployment config, update the matching section here in
  the same change.

## Placeholder content

- The **"Selected work" section** on the homepage is a "case studies coming
  soon" placeholder — no real case studies exist yet.
- The **hero stats** (campaigns launched, ROAS, impressions) in
  `index.astro` are marketing copy, not sourced figures; confirm with the
  client before treating them as facts.
