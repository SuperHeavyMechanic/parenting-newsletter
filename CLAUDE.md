# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## What this repo is

Josh Weekly: a weekly parenting newsletter for Shan (first-time dad, Jakarta) following his son Josh's development. Each week, one Notion sub-page is generated for that week's theme and Josh's exact age, and a designed HTML email is sent to Shan. The newsletter runs indefinitely, with no end week.

The scheduled task's instructions are the authoritative run spec. `parenting-newsletter-BRIEF.md` is background context from the original 16-week setup. Where the brief conflicts with this file or the task instructions (for example its 16-week cap or Saturday schedule), this file and the task instructions win.

## Running a newsletter issue

Paste this into Claude Code to generate the current week's issue end-to-end:

> Read CLAUDE.md and parenting-newsletter-BRIEF.md and generate this week's parenting newsletter issue end-to-end: resolve the target week, read the spec page, fold in any comments on the latest issue, research the approved sources, choose a topic-specific Unsplash header photo, create the Notion sub-page under the master doc, generate the visual-glossary diagrams into ./assets, link the matching Saturday task, send the email via Resend, and print a summary. Do not mark the task complete.

## Key Notion IDs (do not re-discover)

| Thing | ID |
|---|---|
| Master doc (parent for all issues) | `3729aa0c-f135-80a1-bd8c-ecb29f02e535` |
| Spec / style guide | `3729aa0c-f135-815c-8934-d076ff3bcf57` |
| Tasks data source | `2849aa0c-f135-81f5-b051-000b6827d73f` |

### Issued pages

The source of truth for which weeks exist is the master doc's child pages whose TITLE starts with "Week " followed by a digit. Ignore the Spec page, the CDC Milestone Tracker and any other child pages. The table below is a partial historical record only. Do not rely on it to decide what exists.

| Week | Notion page ID |
|---|---|
| Week 1 — Preparing for a planned C-section | `3729aa0c-f135-81aa-bf3e-ea060800a340` |
| Week 2 — The C-section birth & the first hour | `37c9aa0c-f135-8115-b71f-f7eac53ebbf4` |
| Week 3 — Bringing baby home: the survival kit | `37f9aa0c-f135-819f-bbb5-eb671f108ebd` |
| Week 4 — Postpartum recovery: supporting your wife | `3949aa0c-f135-81be-9014-fba104e26662` |
| Week 5 — Feeding deep-dive | `38d9aa0c-f135-81c2-b574-d94d5bbc6cd3` |
| Week 6 — Breastfeeding support for dads | `38d9aa0c-f135-8161-8f52-fee3f84bae42` |
| Week 7 — Sleep deep-dive | `39b9aa0c-f135-81b3-9586-d3ae4d82da66` |
| Week 8 — Crying & soothing mastery | `3a29aa0c-f135-8192-8dca-f00e5c023f4a` |
| Week 9 — Postpartum mental health | `3a99aa0c-f135-8169-a6a2-c1524df58d69` |

Always read the spec page at the start of every run. Shan may have changed depth dials or added notes.

## Baby and delivery context

- **Baby:** Josh
- **Birth:** planned C-section on 2026-06-18
- Whenever maternal recovery comes up, frame it as C-section recovery. Never mention labor contractions, effacement, dilation, or the 5-1-1 rule.
- Josh's development is the main focus. Cover his mum's recovery only where it fits the timing.

## Week formula (no cap)

- `N = floor((today − 2026-06-06) / 7) + 1`. There is no upper limit; the newsletter continues every week.
- Target weeks = every week from **19** to N that does not exist yet, oldest first. Weeks before 19 are intentionally skipped and must never be created. If none are missing, do nothing and send no email.
- Saturday of week N = 2026-06-06 + (N − 1) × 7 days.
- Josh's age = that Saturday − 2026-06-18.
- CDC milestone checkpoints fall on Josh's monthly birthdays: 2 months = 2026-08-18, 4 months = 2026-10-18, 6 months = 2026-12-18, then 9, 12, 15, 18, 24, 30, 36 months, then yearly. Always use the next checkpoint on or after that week's Saturday.

## Approved sources — only these

BabyCenter, ACOG (`acog.org/womens-health`), CDC (Learn the Signs / Milestone Tracker), Mayo Clinic, Cleveland Clinic. Every claim and reading item needs a working link. Never guess if a fact isn't in these sources.

**Research tooling note:** Medical sites (Mayo Clinic, ACOG, Cleveland Clinic, CDC) frequently return 403 to WebFetch, and babycenter.com is not crawlable by WebSearch at all (blocked domain). Use `WebSearch` scoped to `allowed_domains: ["mayoclinic.org", "acog.org", "my.clevelandclinic.org", "cdc.gov"]` to find URLs and get enough summary detail from the search snippet itself — don't rely on WebFetch succeeding on the actual page.

## Header photo (Unsplash)

- Every issue gets its own header photo, chosen via the Unsplash connector for **that week's specific topic** (e.g. "baby reaching for toy", "baby first solid food"), never a generic "baby" search.
- Use free photos only (not Unsplash+), landscape orientation, with soft, warm or neutral tones that suit the sage-and-caramel design. Never choose anything clinical, distressing, showing unsafe sleep or handling, medical procedures, or nudity.
- Never reuse a photo. Before choosing, read every `assets/week-*/hero.json` and skip IDs already used.
- Save the choice to `assets/week-N/hero.json` with `id`, `slug`, `raw_url`, `alt`, `photographer_name`, `photographer_username`, `unsplash_page`, `query_used`.
- Header URL = `raw_url + "&w=1200&h=480&fit=crop&crop=faces,entropy&q=80&fm=jpg"`. Always use exactly this fixed 5:2 crop so the header height is constant.
- Credit "Photo by {name} on Unsplash" in both the email and the Notion page. Link Unsplash's own image URL; do not re-host the photo.
- Set the photo as the Notion page cover.

## Email (Josh Weekly)

- **Template:** `templates/email.html`; icons in `templates/icons/`. Do not edit these during a run; only fill them in.
- **Send via:** Resend connector `send-email`
  - From: `Josh Weekly <onboarding@resend.dev>`
  - To: `shansebastian@gmail.com`
  - Idempotency key: `josh-weekly-week-{N}`
- **Subject:** `[Josh weekly newsletter] Week {N} · {W}w {D}d — {Short theme}`
- **Filling the template:** Remove all HTML comments first. Then fill every `{{TOKEN}}` and repeat each REPEAT block once per item, as its marker describes. Keep all styles, colours and icon URLs unchanged. Confirm no `{{` remains before sending, and always include a plain-text version.
- **Fallback:** If Resend fails, send a text-only version through the Gmail connector's `send_message` with the same subject. The Gmail connector strips all images and the CSS `background:` shorthand, so use `background-color` / `bgcolor` and no images there.
- Send at most one email per week number, and only after the Notion page exists.
- **Sender limits:** Resend's `resend.dev` test sender only delivers to the Resend account owner's address (shansebastian@gmail.com). Sending to anyone else, or from a custom address, needs a verified domain in Resend.

## Visual glossary diagrams

SVGs live in `./assets/week-N/`. Embed in Notion via GitHub raw URLs:

```
https://raw.githubusercontent.com/SuperHeavyMechanic/parenting-newsletter/main/assets/week-N/<filename>.svg
```

Workflow per issue:
1. Generate original SVG diagrams (never copy from medical sites — copyright)
2. Save to `./assets/week-N/` (together with `hero.json`)
3. `git add` each file by name, commit, `git push origin main`
4. Embed the raw GitHub URLs in the Notion page using `![alt](url)` in the visual glossary section
5. Push must complete before the raw URLs are embedded, so they are live

## Linking the weekly task

Find the task titled `Read Week N newsletter…` in the tasks data source and insert the newsletter page URL into the task's page content using `notion-update-page` with `command: insert_content`. If no task exists for that Saturday, skip this and note it in the summary. Do not update any task status property — Shan marks tasks complete himself.

## Git workflow

- Commit after every run with the new SVG assets and `hero.json`
- Push to `origin main` immediately after commit (`github.com/SuperHeavyMechanic/parenting-newsletter`)
- Stage files explicitly by name — never `git add .`
- The repo is public. Never commit anything private (tokens, personal documents, photos of Josh).

## Automated schedule

A cloud scheduled task named "Weekly Parenting Newsletter" runs every **Sunday around 08:00 Asia/Jakarta (01:00 UTC)** and generates any missing weeks automatically.

- **ID:** `trig_016j2S8j2vpQV9t445KR1gEr`
- **Manage it at:** https://claude.ai/code/routines/trig_016j2S8j2vpQV9t445KR1gEr
- **Connectors it needs:** Notion, Resend, Unsplash, Gmail (fallback)
