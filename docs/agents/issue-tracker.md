# Issue tracker: Forgejo

Issues and PRDs for this repo live as Forgejo issues on `forgejo.burythehammer.com`, owner/repo `burythehammer/death-in-videogames`. Use the `forgejo` MCP tools for all operations — do not use the `gh` CLI (that's the GitHub mirror, used for production deploys only).

## Conventions

- **Create an issue**: `mcp__forgejo__create_issue` with `owner: burythehammer`, `repo: death-in-videogames`.
- **Read an issue**: `mcp__forgejo__get_issue`, plus `mcp__forgejo__list_issue_comments` for the discussion.
- **List issues**: `mcp__forgejo__list_issues` with `state`/`labels` filters as needed.
- **Comment on an issue**: `mcp__forgejo__create_issue_comment`.
- **Apply / remove labels**: `mcp__forgejo__add_issue_labels` / `mcp__forgejo__remove_issue_label` (labels are referenced by numeric ID — look them up with `mcp__forgejo__list_labels` first).
- **Close**: `mcp__forgejo__edit_issue` with `state: closed`.

## Pull requests as a triage surface

**PRs as a request surface: no.** _(Set to `yes` if this repo treats external PRs as feature requests.)_

## When a skill says "publish to the issue tracker"

Create a Forgejo issue via `mcp__forgejo__create_issue`.

## When a skill says "fetch the relevant ticket"

Run `mcp__forgejo__get_issue` (plus `mcp__forgejo__list_issue_comments` for the discussion).

## Repo topology note

This repo has two remotes: `origin` (Forgejo, this tracker, primary dev) and `github` (GitHub, production — GitHub Pages deploys `main` from here). Issues and day-to-day work stay on Forgejo; `main` is pushed to `github` only when ready to ship. See the "Repo topology" section in the root `CLAUDE.md`.
