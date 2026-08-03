# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## What this is

"Death in Videogames" — a Jekyll static blog (Hyde/Poole theme) examining how videogames represent death, one game analysis per post. Deployed to https://www.deathinvideogames.com (see `CNAME`).

## Commands

Requires Ruby 3.0.0 and Bundler.

```sh
bundle install          # install gems (jekyll, jekyll-paginate, webrick, etc.)
bundle exec jekyll serve   # run local dev server with live rebuild (http://localhost:4000)
bundle exec jekyll build   # build static site to _site/
```

There is no test suite or linter configured for this repo.

## Architecture

Standard Jekyll layout, configured via `_config.yml` (markdown: kramdown, highlighter: pygments, pagination via `jekyll-paginate`, pretty permalinks).

- `_posts/` — one Markdown file per game analysis, named `YYYY-MM-DD-game-name.md`. Each post follows a consistent front matter + section structure (see below).
- `_layouts/` — `default.html` (base HTML shell, includes sidebar), `page.html` (standalone pages like About), `post.html` (blog posts, includes a "Related Posts" block using `site.related_posts`).
- `_includes/` — `head.html` (meta/CSS/favicon/RSS links), `sidebar.html` (site title, nav auto-generated from any page with `layout: page`, footer).
- `index.html` — paginated home page listing full post content, one post per page (`paginate: 1` in `_config.yml`).
- `assets/images/` — post header images, referenced from post Markdown as `/assets/images/NN-slug.jpg`.
- `public/css/` — theme stylesheets (`poole.css`, `hyde.css`, `syntax.css`) plus favicon/touch-icon assets.
- `about.md` — standalone page (`layout: page`), auto-appears in sidebar nav.
- `atom.xml` — RSS/Atom feed template (`layout: null`), iterates `site.posts`.
- `404.html` — custom 404 page.

## Post conventions

Each post in `_posts/` follows this front matter shape:

```yaml
---
layout: post
title: "Game Name"
date: YYYY-MM-DDTHH:MM:SSZ
tags:
- game-slug
---
```

Body structure (as headed sections, using `---`-underlined H2s via `------`):
1. Header image: `![Game Name](/assets/images/NN-slug.jpg)`
2. `Game Setup` — brief premise/genre summary
3. `How death is represented` — bulleted list, each item prefixed with a category: `Narrative:`, `Systematic:`, or `Thematic:`
4. `Tone:` — short bulleted list of tone descriptors
5. `Analysis` — prose analysis of the game's treatment of death; spoiler-heavy sections are flagged with `**_Spoilers follow._**`
6. Trailing platform list: `*Game is available on the following platforms:*` + bullets

New posts should match this structure and add a corresponding image to `assets/images/`.

## Agent skills

### Issue tracker

Issues live as Forgejo Issues on `burythehammer/death-in-videogames` (forgejo.burythehammer.com), managed via the Forgejo MCP tools. See `docs/agents/issue-tracker.md`.

### Repo topology

`origin` (Forgejo) is the primary dev remote — day-to-day work happens here. `github` is the production remote: GitHub Pages deploys the site from `main` on GitHub, so only push there when `main` is ready to ship.

Feature workflow (issue → branch → PR → merge/close) follows the global default Forgejo workflow.

### Domain docs

Single-context layout: `CONTEXT.md` + `docs/adr/` at the repo root. See `docs/agents/domain.md`.
