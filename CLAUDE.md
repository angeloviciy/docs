# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## What this is

A Mintlify documentation site for the **Conway frontend**, organized as **task-first job pages** for fraud analysts plus a short **admin onboarding** track. The product source of truth is `/Users/angel/Documents/Github/conway` (branch `rohan/demo-frontend-tweaks`). When writing or updating a page, treat that repo as authoritative: read the actual frontend component and confirm labels/behavior against the running UI before describing it.

Content is MDX with YAML frontmatter; navigation and site config live in `docs.json`. There is no build system, package.json, or test suite here — authoring is the workflow.

## Two-tab structure

The site has exactly two Mintlify tabs:

- **Guides** — one surface, two visibly-grouped zones: **analyst** content first (Getting started → Triage → Investigate → Case work → Knowledge → Field reference), then a visible divider and **admin onboarding** at the bottom.
- **Pilot** — per-deployment template pages. Pilot readers are analyst-only (Response isn't enabled, sources/playbooks are preconfigured). The main `Guides` tab is what they read for job-level instructions; the Pilot tab carries only the per-deployment specifics (what's connected, who to contact, etc.).

## Audiences

- **Primary — fraud analyst.** Non-technical, works cases. Reads `Getting started` through `Knowledge`. Never clicks anything under `Admin onboarding`.
- **Secondary — deployment admin.** Typically a data engineer or ops person at a customer. Reads `Admin onboarding` once during rollout and rarely returns. Does not read analyst jobs.

Pages that target the admin live under `admin/` and appear only in the admin section of the sidebar.

## Voice and depth

- Active, second person, sentence case headings. One idea per sentence.
- Lead with the reader's job ("Triage Positives"), not the screen's name ("Reports tab").
- Definitional openings, not narrative ones. First paragraph states what the subject **is** and what the reader is deciding — "Positive investigations are the ones where Conway concluded the activity was of concern." Never stage a shift or editorialize the pace ("Let's dive in", "Each morning you'll want to…").
- Exhaustive on *what the reader does and sees*; minimal on *how it works*. No endpoint names, DBOS references, backend terms, or "under the hood" asides.
- Primary analyst arc is Detection → Response; everything else is supporting.

### Prose micro-rules

- **Em dash budget.** At most one em-dash pair per paragraph, and don't start a sentence with an em dash. When tempted to reach for a second pair, use a comma + `which` clause, a semicolon, or rephrase. For inline code examples, use `like ` X `` and ``  Y ``, not em-dashes around the list.
- **Word choice.** Prefer **review** over "read", **examine** over "look at", **start with** or **scan** over "skim", **open** over "pop open" / "pull up". Professional register throughout — match the tone of Linear or Stripe docs, not tutorial blog posts.
- **Step titles.** A full imperative verb phrase that states the whole action, not a clipped fragment. "Open and review each investigation" beats "Open each row and read".
- **No editorial padding.** Cut "simply", "just", "easily", "quickly", "in a few clicks", "don't worry", "you'll notice". The action stands on its own.
- **Sentence-case headings everywhere.** Not Title Case. Not ALL CAPS (except when quoting literal UI labels like **DRAFT RUN**).

## Scope

**In scope:**

- Analyst daily work: triage, investigation-reading, verdict override, telemetry audit, case work, RFIs, determinations, entity cross-reference, intelligence management, rule proposals.
- Admin onboarding: identity-provider federation (WorkOS), inviting analysts and assigning roles, connecting Snowflake/Redshift, testing/fixing sources, wiring up integrations (Slack, ticketing).
- The Chat assistant, introduced once in `chat-overview`, and woven through job pages as inline callouts.

**Out of scope:**

- Authoring or editing **playbooks** (the Automations tab). Playbooks are configured for customers by the Conway team in both Pilot and early full deployments. Document the *existence* of the tab and what "Live vs. Draft" means if a reader stumbles onto it; do not write build/generate walkthroughs.
- API reference, CLI (`conway_cli`), deployment/infra, Docker/Kubernetes.
- Developer-only concepts (DBOS queues, run_phase state machine internals, DuckDB workspaces).

## Terminology lock

Enforced across all pages — do not deviate without updating this file and `AGENTS.md`:

- Agent output noun = **Investigation**. The markdown body it produces = **Report**.
- Verdicts = **positive / negative / uncertain**. Never "fraud/legitimate." Use lowercase in prose ("a positive verdict," "flagged uncertain"); capitalize only when quoting a literal UI label, column value, or status chip.
- Case dispositions = **True positive / False positive / Inconclusive** (plus UI-visible subcategories).
- Top-level areas = **Detection / Response / Sources** (sidebar labels). Not "Mission Control," not "Cases," not "Data Sources."
- Automation tab contains **playbooks**. "Playbook" = the artifact; "Automation" = the tab/concept, matching the UI.
- **Never use "alert"** as a product noun — the demo branch is removing that concept. If a UI drawer still uses the label, document around it (e.g. "investigation detail" or the current UI text); flag the inconsistency instead of propagating it.

## Page template

One page per realistic task a reader does end-to-end (`jobs/*.mdx`, `admin/*.mdx`, and Pilot templates all use the same shape). Title is a verb phrase the reader would actually say ("Triage investigations," "Connect Snowflake"), never a screen name.

```mdx
---
title: '<Verb-phrase job title>'
description: '<One sentence: what this page covers, in definitional tone>'
---

<Two-to-three sentences: what this page covers. Definitional, not narrative —
state the subject and the decision the reader is making. Do not stage the
reader's shift or editorialize the pace.>

<Frame>
  <img src="/images/jobs/<slug>/overview.png" alt="<Where in the UI this job happens>" />
</Frame>

<Steps>
  <Step title="<Imperative verb phrase>">
    One-to-three sentences. Cite UI labels in **bold**. Describe action + result
    in one breath, no editorializing. Add a per-step `<Frame>` only when the
    overview screenshot isn't enough.
  </Step>
  …
</Steps>

## What to do next <!-- or similar; only when the job ends in a branch -->

<When a job ends in a decision branch, list the outcomes as a short bulleted
list and link into the downstream job pages inline. Skip this section if the
job terminates cleanly without branching.>

<Tip>
**Shortcut.** Inline tip for a power-user path or a chat prompt that's
genuinely faster. Optional. Label it ("Shortcut," "Tip," "Heads up") so the
reader knows why it's there. See the Chat pattern section below for when to
include a chat prompt.
</Tip>
```

**Sections to avoid by default.** These existed in the previous template and are out:

- No `## Steps` H2 — the `<Steps>` component delimits itself.
- No `## Before you start` unless there's a *real* precondition (required role, prior setup). Empty-grid and troubleshooting don't count — handle those inline or in chat.
- No `## Where this lives in the UI` — weave field detail inline at the step that uses the field, not in a dedicated section.
- No `## Related jobs` header block — link related jobs inline in the step or branch where they're mentioned.
- No `## What you can do here` bullet list — if the title is a job, the list is redundant.

**Length target.** A three-step job page renders in ~30-40 lines. If a page runs longer, interrogate whether every section is earning its place before adding more.

## Chat pattern

The Chat assistant is introduced once, in `chat-overview.mdx` (the single orientation page under Getting started). It appears everywhere else as **inline `<Tip>` callouts** inside job pages — never as its own H2 section, never as a top-of-page reminder.

```mdx
<Tip>
**Shortcut.** Before opening rows one at a time, ask the chat *"what's the shape of today's Positives — any clusters, repeated entities, unusual volume?"* A birds-eye read often catches patterns that don't surface row by row.
</Tip>
```

Rules:

- Only write a callout when chat is **genuinely the faster path** than the manual flow. Not for decoration.
- Lead the tip with a short bolded label (**Shortcut**, **Tip**, **Heads up**) so the reader knows why the callout is there.
- Put the actual prompt in italics and use the phrasing an analyst would actually type — not a formal description.
- Place the tip where it's relevant: near the decision moment in a step, or as a terminal tip when it's a cross-cutting shortcut over the whole job.
- Do not stack more than one tip per job page without interrogating whether both earn their place.
- The old top-of-page `<ChatReminder />` snippet has been deleted. Don't recreate it; the chat pattern is inline `<Tip>` callouts only.

Good placements: a second-opinion prompt before a verdict override, a summarize prompt when opening a dense investigation, a "has this entity come up before?" prompt at cross-reference moments, a drafting prompt when composing an RFI.

## Sidebar conventions

The Guides sidebar groups analyst content first, admin content last, with a visual break between them. Current group order:

1. Getting started (welcome, core-concepts, chat-overview)
2. Triage
3. Investigate
4. Case work
5. Knowledge
6. — (visual divider) —
7. Admin onboarding

Pilot tab carries only per-deployment templates: `index` (what a Pilot is + limitations merged), `contacts`, `sources-configured`, `playbooks-configured`. No job pages — those live in Guides.

## Common commands

- `mint dev` — local preview at `http://localhost:3000` (run from repo root). Requires Node 19+ and `npm i -g mint`.
- `mint dev --port 3333` — override port.
- `mint broken-links` — validate all internal links; run before PRs that move or rename pages.
- `mint update` — update CLI if the local preview drifts from production.

Deployment is automatic: pushing to `main` triggers the Mintlify GitHub app.

## Authoring workflow

- **Adding a page**: create the `.mdx` file, then add its slug to the appropriate `groups[].pages` array in `docs.json`. A page on disk but not referenced in `docs.json` won't appear in nav.
- **Page slug** = path without `.mdx`, relative to repo root (e.g. `jobs/triage`).
- **Screenshots**: the user captures these. Reference predictable paths like `/images/jobs/<slug>/overview.png` or `/images/jobs/<slug>/step-N.png` so shots can be dropped in without editing MDX.
- **Reusable snippets** live in `snippets/`. The canonical verdict phrasing is in `snippets/verdict-values.mdx` — import it when a page needs the three-verdict definition, don't re-type the labels.

## Migration state

The old per-screen pages (`detection/*`, `run-detail/*`, `response/*`, `case-detail/*`, `automations/*`, `chat/*`, `sources/*`, `navigating.mdx`, `snippets/chat-reminder.mdx`, `pilot/limitations.mdx`, `pilot/terminology.mdx`) have been removed. All analyst content now lives under `jobs/*.mdx`, admin content under `admin/*.mdx`, dense field detail under `reference/*.mdx`, and per-deployment Pilot content under `pilot/*.mdx`. Don't recreate the old per-screen layout — every new page is either a job, a field reference, or a Pilot template.

## Mintlify component reference

Mintlify-specific component syntax (`<Steps>`, `<AccordionGroup>`, `<Frame>`, etc.), `docs.json` schema, and writing standards are covered by the `mintlify` / `mintlify-docs` skills (see `skills-lock.json`). Invoke them rather than guessing component names.
