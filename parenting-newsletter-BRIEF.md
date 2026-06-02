# Parenting Newsletter — Claude Code Execution Brief

**Purpose:** Generate Shan's weekly first-time-dad parenting newsletter into Notion, on a schedule, to a defined spec. Each run produces one weekly issue as a Notion sub-page, illustrated and sourced, ready for Shan to read against his Saturday task.

Hand this whole file to Claude Code. Two usage modes:
- **One-off:** paste a run instruction (see §6) and let it generate this week's issue now.
- **Scheduled:** save this as `BRIEF.md` in a repo and register the weekly schedule in §7 so it runs every Saturday.

---

## 1. Prerequisites

- **Notion connector enabled** in Claude Code (MCP). The routine reads/writes the workspace below.
- **Web access** for source research (the approved medical sources in §4).
- (Optional, for embedding diagrams) a way to host images at a public URL — see §6 step 7.

---

## 2. Constants (already-resolved IDs — do not re-discover)

| Thing | Value |
| --- | --- |
| Master doc (parent for all issues) | page_id `3729aa0c-f135-80a1-bd8c-ecb29f02e535` |
| Spec / style guide (source of truth) | page_id `3729aa0c-f135-815c-8934-d076ff3bcf57` |
| Week 1 issue (reference example) | page_id `3729aa0c-f135-81aa-bf3e-ea060800a340` |
| Tasks data source | `2849aa0c-f135-81f5-b051-000b6827d73f` |
| "Parenting" area page (for task relation) | `https://www.notion.so/3729aa0cf1358081a436d93bd4a3cb04` |
| Task "not started" status value | `🍵` |
| Timezone | Asia/Jakarta |
| Baby due date (scheduled) | 2026-06-18 |
| Delivery method | Planned C-section |
| Baby date of birth (fill once born) | `BABY_DOB = 2026-06-18` (scheduled; update if changed) |

The 16 weekly "Read Week N newsletter…" tasks already exist in the tasks DB, dated each Saturday from 2026-06-06 to 2026-09-19, area = Parenting, no project.

---

## 3. Week map (theme + age)

Week 1 anchors to Saturday 2026-06-06; each subsequent week is +7 days.

| Wk | Saturday | Theme | Baby age* |
| --- | --- | --- | --- |
| 1 | Jun 6 | Preparing for a planned C-section | ~37 wk pregnant |
| 2 | Jun 13 | The C-section birth & the first hour | ~38 wk pregnant |
| 3 | Jun 20 | Bringing baby home: the survival kit | Birth Jun 18 / ~0–2 days old |
| 4 | Jun 27 | Postpartum recovery — supporting your wife | 0–1 wk |
| 5 | Jul 4 | Feeding deep-dive | 1–2 wk |
| 6 | Jul 11 | Breastfeeding support for dads | 2–3 wk |
| 7 | Jul 18 | Sleep deep-dive | 3–4 wk |
| 8 | Jul 25 | Crying & soothing mastery | 4–5 wk |
| 9 | Aug 1 | Postpartum mental health | 5–6 wk |
| 10 | Aug 8 | Bonding with baby | 6–7 wk |
| 11 | Aug 15 | Baby care mastery | 7–8 wk (2 mo) |
| 12 | Aug 22 | Doctor visits & what to track | 8–9 wk |
| 13 | Aug 29 | Baby development — a deeper look | 9–10 wk |
| 14 | Sep 5 | Building gentle sleep habits | 10–11 wk |
| 15 | Sep 12 | Your marriage & partnership | 11–12 wk (3 mo) |
| 16 | Sep 19 | Looking ahead — 3 months & beyond | 12–13 wk |

*Age column is an estimate. Once `BABY_DOB` is set, compute the baby's real age for that week and use it for the development content. Before birth, use the gestational estimate.

**Determining the current week:** `N = clamp(1, 16, floor((today − 2026-06-06) / 7) + 1)`. If a page for week N already exists under the master doc, generate the next missing week instead (idempotent).

---

## 4. Approved sources (only these)

BabyCenter, ACOG (acog.org/womens-health), CDC (Learn the Signs / Milestone Tracker), Mayo Clinic, Cleveland Clinic.

- Always include a working link beside each reading item and key claim.
- If a fact isn't in these sources, say "This information is not available in the referenced sources" rather than guessing.
- If a source page can't be reached, substitute another approved source and note it.

---

## 5. Content rules (the spec is the source of truth — read it every run)

Fetch the spec page (`3729aa0c-f135-815c-8934-d076ff3bcf57`) and follow it. Summary of what it currently says:

**Tone:** warm, practical, beginner-friendly, plain language. Skimmable. Tables where useful. Not too long.

**10-section template (in order):**
1. Header — week number, baby's age, theme
2. This week's theme — 1–2 sentence framing
3. What to read — 3–5 items (title · source · why it matters · link)
4. Simple summary — What's happening / Why it matters / What to watch for / What dad can do
5. Development focus (recurring) — what's typical at this age, what dad can do, one red flag worth raising with the pediatrician
6. Dad checklist — 5–10 concrete actions
7. Questions for our doctor — 5
8. Warning signs — split "for my wife" / "for the baby"; not a diagnosis, when to call
9. Dad reflection — 3 questions
10. Visual glossary (recurring) — 2–4 hardest-to-picture terms, each a simple original diagram + one-line everyday analogy

**Depth dials (current):** Development focus = **Deep**; all other sections = **Standard**. Re-read the dials each run in case Shan changed them.

**Guardrails:**
- Never diagnose. Anything needing medical judgment → "check with your doctor."
- Milestones have wide normal ranges — signposts, not deadlines.
- Copyright: paraphrase; at most one short (<15-word) quote per source; link out for the rest.

---

## 6. The weekly routine (what each run does)

1. **Resolve week N** (see §3) and its theme + baby age.
2. **Read the spec page** and apply the template + depth dials.
3. **Read feedback:** fetch the most recent existing issue page and run get-comments on it. Fold issue-specific feedback into this week's issue; if a comment implies a structural change, also update the spec page and add a changelog row.
4. **Research:** search the approved sources for the theme and the age-appropriate development milestones / red flags. Gather 3–5 reading links. Verify facts. Cite links inline.
5. **Compose** the issue against the 10-section template and dials.
6. **Create the Notion sub-page** under the master doc (`3729aa0c-f135-80a1-bd8c-ecb29f02e535`), titled `Week N — <Theme>`, with a relevant emoji icon and the full content. Put a clear note at the very top: *"Draft for review — verify anything medical with your doctor."*
7. **Visual glossary diagrams:** identify the 2–4 hardest terms, generate **original** SVG diagrams (not copied medical images), each with a plain-language analogy. Save them to `./assets/week-N/`. To embed in Notion you need a public image URL — pick one:
   - commit the SVGs to the repo and embed via their `raw.githubusercontent.com` URLs, **or**
   - upload to a public bucket and embed those URLs, **or**
   - if no host is configured, skip embedding and leave the plain-language analogies in the page (Week 1 pattern), noting the files are in `./assets/week-N/`.
8. **Link the task:** find the existing task titled `Read Week N newsletter: …` in the tasks data source and add the new issue page URL to it (do not mark it complete — Shan does that after reading).
9. **Changelog:** if the spec changed this run, append a version row.
10. **Print a summary** to stdout: week number, page URL, sources used, and any spec changes.

**One-off run instruction (paste into Claude Code):**
> Read BRIEF.md and generate this week's parenting newsletter issue end-to-end: resolve the current week, read the spec page, fold in any comments on the latest issue, research the approved sources, create the Notion sub-page under the master doc, generate the visual-glossary diagrams into ./assets, link the matching Saturday task, and print a summary. Do not mark the task complete.

---

## 7. Scheduling (one-time setup)

Recommended: a **`/schedule` Cloud task** (the "Routines" feature) so it fires every Saturday whether the laptop is on or not, with access to the Notion connector.

In a Claude Code session, run:
```
/schedule
```
and configure: **weekly, Saturdays, 07:00 Asia/Jakarta**, with the run instruction from §6. Equivalent cron expression: `0 7 * * 6`.

Alternatives:
- **Desktop scheduled task** — same `/schedule`, but only runs while the machine is on (macOS/Windows; not Linux).
- **System cron + headless:** `0 7 * * 6  cd /path/to/repo && claude -p "$(cat BRIEF.md) — run this week's issue now" --allowedTools "Read" "Write" "Bash"` (machine must be on).
- **GitHub Actions:** `anthropics/claude-code-action@v1` on a `schedule: cron: '0 0 * * 6'` trigger (runs on GitHub's infra). Run `/install-github-app` once to set it up.

Note: each scheduled run is a full Claude Code session and counts toward usage limits — negligible for once a week.

---

## 8. Optional: package as a reusable command

Consider turning §6 into a custom slash-command or skill (e.g. `/newsletter`) in the repo so the schedule just calls `/newsletter`, and the logic lives in one versioned place. Mirrors the existing `ai-briefing` skill pattern in the workspace.

---

## 9. Definition of done (per run)

- A new `Week N — <Theme>` page exists under the master doc, following the 10-section template, marked draft-for-review.
- Every reading item and key claim has an approved-source link.
- 2–4 visual-glossary diagrams generated (embedded if a host is configured, else saved to ./assets with analogies in-page).
- The matching Saturday task has the page URL attached (left incomplete).
- Spec changelog updated if the spec changed.
- No diagnoses; medical-judgment items defer to the doctor.
