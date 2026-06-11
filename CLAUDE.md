# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## What this repo is

A weekly parenting newsletter for Shan (first-time dad, Jakarta). Each Saturday, one Notion sub-page is generated covering that week's theme and baby's development stage. The full run spec lives in `parenting-newsletter-BRIEF.md` — read it before every run.

## Running a newsletter issue

Paste this into Claude Code to generate the current week's issue end-to-end:

> Read parenting-newsletter-BRIEF.md and generate this week's parenting newsletter issue end-to-end: resolve the current week, read the spec page, fold in any comments on the latest issue, research the approved sources, create the Notion sub-page under the master doc, generate the visual-glossary diagrams into ./assets, link the matching Saturday task, and print a summary. Do not mark the task complete.

## Key Notion IDs (do not re-discover)

| Thing | ID |
|---|---|
| Master doc (parent for all issues) | `3729aa0c-f135-80a1-bd8c-ecb29f02e535` |
| Spec / style guide | `3729aa0c-f135-815c-8934-d076ff3bcf57` |
| Tasks data source | `2849aa0c-f135-81f5-b051-000b6827d73f` |

### Issued pages (do not recreate)

| Week | Notion page ID |
|---|---|
| Week 1 — Preparing for a planned C-section | `3729aa0c-f135-81aa-bf3e-ea060800a340` |
| Week 2 — The C-section birth & the first hour | `37c9aa0c-f135-8115-b71f-f7eac53ebbf4` |

Always read the spec page at the start of every run — Shan may have changed depth dials or added notes.

## Delivery context

- **Method:** Planned C-section (not vaginal birth)
- **Date:** 2026-06-18
- Weeks 1–4 content must reflect C-section prep/OR experience/recovery — not labor contractions, effacement, dilation, or the 5-1-1 rule

## Week formula

`N = clamp(1, 16, floor((today − 2026-06-06) / 7) + 1)`

If week N already exists in the issued pages table above, generate the next missing week instead (idempotent).

## Approved sources — only these

BabyCenter, ACOG (`acog.org/womens-health`), CDC (Learn the Signs / Milestone Tracker), Mayo Clinic, Cleveland Clinic. Every claim and reading item needs a working link. Never guess if a fact isn't in these sources.

**Research tooling note:** Many medical sites (Mayo Clinic, ACOG) return 403/402 to WebFetch. Use `WebSearch` to find and confirm URLs, then `WebFetch` on Cleveland Clinic and BabyCenter which are generally accessible. If a WebFetch fails, fall back to the search result snippet for that source.

## Visual glossary diagrams

SVGs live in `./assets/week-N/`. Embed in Notion via GitHub raw URLs:

```
https://raw.githubusercontent.com/SuperHeavyMechanic/parenting-newsletter/main/assets/week-N/<filename>.svg
```

Workflow per issue:
1. Generate original SVG diagrams (never copy from medical sites — copyright)
2. Save to `./assets/week-N/`
3. `git add` each SVG by name, commit, `git push origin main`
4. Embed the raw GitHub URLs in the Notion page using `![alt](url)` in the visual glossary section
5. Push must complete before creating the Notion page so the raw URLs are live

## Linking the weekly task

Find the task titled `Read Week N newsletter…` in the tasks data source and insert the newsletter page URL into the task's page content using `notion-update-page` with `command: insert_content`. Do not update any task status property — Shan marks tasks complete himself.

## Git workflow

- Commit after every run with the new SVG assets
- Push to `origin main` immediately after commit (`github.com/SuperHeavyMechanic/parenting-newsletter`)
- Stage files explicitly by name — never `git add .`

## Automated schedule

A cloud routine runs every **Sunday at 08:00 Asia/Jakarta (01:00 UTC)** and generates the next missing week automatically. Routine ID: `trig_01YHpe2YF6xcNXYECcmbbZAn`. Manage at https://claude.ai/code/routines/trig_01YHpe2YF6xcNXYECcmbbZAn.
