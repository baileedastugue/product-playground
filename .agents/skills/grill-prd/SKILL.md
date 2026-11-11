---
name: grill-prd
description: Turn a product concept and its in-scope list, plus any supporting spreadsheets, notes, Figma files, or links, into a finished PRD by interviewing the user in rounds of multiple-choice questions, then filling in the project's PRD template. Use when the user wants to write, draft, or spec a PRD or product requirements doc, or types /grill-prd.
---

# Grill PRD

The user brings the **concept** and the **in-scope** list. Your job is to interview them until every other section of the PRD template is settled, then write the PRD.

## 1. Intake

You need two inputs before asking anything else:

- **Concept**: what the product or feature is, and roughly who it's for.
- **In scope**: what this release will deliver.

If either is missing from the user's message, ask for it in plain text (no multiple choice; these are the user's own ideas). In the same message, invite optional supporting material: spreadsheets, notes, research, Figma files, links, screenshots. Then read `templates/prd-template.md`. It is the source of truth for sections, order, and table columns, so reread it each run rather than relying on memory. If it's missing, tell the user and stop.

Before the first round, check the repo for anything that already answers questions (existing code, docs, or earlier PRDs in `docs/prds/`). Finding facts is your job; never ask the user for something you can look up.

### Supporting material

The user may attach or point to extra context at intake or at any point during the interview. Read all of it before the next round:

- **Spreadsheets** (`.xlsx`, `.xls`, `.csv`): read CSVs directly. For Excel files, extract every sheet with a short Python script (`pandas` or `openpyxl`); install the package if it's missing.
- **Documents** (`.docx`, `.pdf`, `.md`, `.txt`, pasted notes): read directly; for `.docx` on macOS, convert with `textutil -convert txt`.
- **Figma links**: use the Figma MCP server if it's connected. Pull the linked frame's screenshot and design context to see the screens, flows, states, and copy. If the server isn't connected or errors, say so and ask the user to share screenshots or exports of the key frames instead. Put the first Figma link in the template's Designs link unless the user says otherwise.
- **Other links**: fetch the page. If it needs a login or won't load, ask the user to paste the content or export a file.
- **Images and screenshots**: read them for what they show (designs, dashboards, user feedback).

Use what you find to:

- **Fill sections directly** when the material settles them (e.g. evidence from survey data, a metric baseline from a spreadsheet). Don't ask questions the material already answers.
- **Sharpen options** for the questions that remain, using real numbers, quotes, and user segments from the material.
- **Check designs against scope**: compare what the Figma screens show with the in-scope list. Raise screens or states with no matching requirement, and in-scope items with no design, as questions. Designs often reveal functional requirements and acceptance criteria (empty, error, and loading states).
- **Surface conflicts** as questions when material disagrees with itself or with what the user said (e.g. the notes name a different primary user than the concept does).

When you show the confirmation summary, note which decisions came from supporting material rather than from the user's answers, so they can catch misreadings.

## 2. Interview in rounds

Map every template section that the intake doesn't already answer. Each round asks only questions whose prerequisites are settled. For example, personas come before requirements, and goals come before metric targets.

Round rules:

- At most **5 questions** per round, answered **one at a time**. If more are ready, ask the most foundational 5 first.
- Every question is multiple choice with 2–4 options **specific to this concept**, never generic. Put your recommendation first and append "(Recommended)" to its label. The user can always give their own answer.
- Use `AskQuestion` when available: one call per round, one entry per question. Otherwise ask in plain text, one question at a time, as lettered options with a final "Something else" option.
- Put the context needed to decide in the question body, and keep option labels short.

What to cover, roughly in this order:

1. **Problem statement**: who has the problem, what it is, evidence, why now.
2. **Personas**: 1–3, with context, key need, pain point.
3. **Goals**: 2–4 goals, each with a success metric and target.
4. **Scope pressure-test**: challenge the user's in-scope list. Flag items that look too large for one release or don't serve a goal, and propose moving them to out of scope. Suggest out-of-scope items the concept implies (adjacent features users will expect) with a one-line reason each.
5. **Functional requirements**: derive from the in-scope list, then ask about priority (P0/P1/P2) and acceptance criteria where they aren't obvious.
6. **Non-functional requirements**: offer the 2–4 that matter most for this concept (e.g. performance, accessibility, privacy).

Skip anything the template marks as post-launch (such as Results & learnings). Leave its prompt in place.

## 3. Unknowns

When something can't be known yet, don't invent it and don't keep pressing. Write `TKTK` with a few words on what's needed (e.g. `TKTK: baseline onboarding time from analytics`). Use an em dash (`—`) for cells that don't apply. Links in the header default to `TKTK` unless the user provides them.

## 4. Confirm, then write

When nothing is left to ask, show a short summary of the decisions grouped by section, and ask the user to confirm or correct it. Don't write the file until they confirm.

Then write the PRD to `docs/prds/<feature-name>.md`, where `<feature-name>` is short and kebab-case:

- Keep the template's sections, order, and table columns exactly.
- Replace every italic prompt and example row with real content. Delete the "How to use" line.
- Set Status to `Draft` and Last updated to today's date.
- Keep it to 1–2 pages. Prefer one tight row over several vague ones.

Finish by listing every remaining `TKTK` so the user knows what to fill in.
