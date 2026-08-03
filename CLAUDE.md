# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## What this is

"Death in Videogames" — a Hugo static blog (Hyde/Poole-style theme, hand-written templates) examining how videogames represent death, one game analysis per post. Deployed via GitHub Actions to GitHub Pages at https://www.burythehammer.com/death-in-videogames/ (the custom `deathinvideogames.com` domain has lapsed; update `baseURL` in `hugo.toml` if/when it's restored).

## Commands

Requires the Hugo extended binary (no Ruby/Bundler).

```sh
hugo server   # local dev server with live rebuild (http://localhost:1313/death-in-videogames/)
hugo build --gc --minify   # build static site to public/
```

There is no test suite or linter configured for this repo.

## Architecture

Standard Hugo layout, configured via `hugo.toml` (custom `[permalinks.page]` preserving `/YYYY/MM/DD/slug/` post URLs, `[pagination] pagerSize = 1` for one post per home page, explicit `[related]` config so posts still surface as related on a small site with few overlapping tags, `disableKinds` for section/taxonomy/term since tags aren't browsable here).

- `content/posts/` — one Markdown file per game analysis. Each post follows a consistent front matter + section structure (see below).
- `content/about.md` — standalone About page, auto-appears in sidebar nav.
- `layouts/_default/baseof.html` — base HTML shell (wraps `head`/`sidebar` partials + a `main` block).
- `layouts/_default/single.html` — generic standalone page template (used for About).
- `layouts/posts/single.html` — post template, includes a "Related Posts" block via Hugo's built-in related-content feature.
- `layouts/index.html` — paginated home page listing full post content, one post per page.
- `layouts/404.html` — custom 404 page.
- `layouts/partials/head.html` — meta/CSS/favicon/RSS links.
- `layouts/partials/sidebar.html` — site title, nav auto-generated from standalone pages (`Type: "page"`), footer.
- `static/assets/images/` — post header images, referenced from post Markdown as `/assets/images/NN-slug.jpg`.
- `static/css/` — theme stylesheets (`poole.css`, `hyde.css`, `syntax.css`) plus favicon/touch-icon assets at `static/` root.
- `.github/workflows/hugo.yaml` — builds and deploys to GitHub Pages on push to `main`.

## Post conventions

Each post in `content/posts/` follows this front matter shape:

```yaml
---
title: "Game Name"
date: YYYY-MM-DDTHH:MM:SSZ
tags:
- game-slug
---
```

(Add an explicit `slug:` field if the title contains characters — like a trailing period — that would otherwise mangle the auto-derived URL slug.)

Body structure (as headed sections, using `---`-underlined H2s via `------`):
1. Header image: `![Game Name](/assets/images/NN-slug.jpg)`
2. `Game Setup` — brief premise/genre summary
3. `How death is represented` — bulleted list, each item prefixed with a category: `Narrative:`, `Systematic:`, or `Thematic:`
4. `Tone:` — short bulleted list of tone descriptors
5. `Analysis` — prose analysis of the game's treatment of death; spoiler-heavy sections are flagged with `**_Spoilers follow._**`
6. Trailing platform list: `*Game is available on the following platforms:*` + bullets

New posts should match this structure and add a corresponding image to `static/assets/images/`.

## Agent skills

### Issue tracker

Issues live as Forgejo Issues on `burythehammer/death-in-videogames` (forgejo.burythehammer.com), managed via the Forgejo MCP tools. See `docs/agents/issue-tracker.md`.

### Repo topology

`origin` (Forgejo) is the primary dev remote — day-to-day work happens here. `github` is the production remote: GitHub Pages deploys the site from `main` on GitHub, so only push there when `main` is ready to ship.

Feature workflow (issue → branch → PR → merge/close) follows the global default Forgejo workflow.

### Domain docs

Single-context layout: `CONTEXT.md` + `docs/adr/` at the repo root. See `docs/agents/domain.md`.
