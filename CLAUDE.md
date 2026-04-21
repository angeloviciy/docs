# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## What this is

A Mintlify documentation site that serves as an **in-depth reference guide to the Conway frontend** for non-technical fraud analysts. The product source of truth is `/Users/angel/Documents/Github/conway` (branch `rohan/demo-frontend-tweaks`). When writing or updating a page, treat that repo as authoritative: read the actual frontend component and confirm labels/behavior against the running UI before describing it.

Content is MDX with YAML frontmatter; navigation and site config live in `docs.json`. There is no build system, package.json, or test suite here — authoring is the workflow.

## Two-tab structure

The site has exactly two Mintlify tabs:
- **Guides** — the product walkthrough. Ordered to mirror the Conway sidebar (Detection → Response → Sources) plus deep-detail sections for Run Detail and Case Detail, the Automations editor, and the Chat assistant.
- **Pilot** — a dedicated tab whose pages are **templates filled in per deployment**. Different pilots see different sidebar nav via `useDomain().pages` in the Conway frontend, and different preset playbooks/sources — the Pilot tab is where that deployment-specific content goes.

## Audience, voice, scope

- **Audience**: non-technical fraud analysts. Not engineers. Not admins.
- **Voice**: active, second person, sentence case headings. One idea per sentence.
- **Depth**: exhaustive on *what the UI shows and lets you do*; minimal on *how it works*. No endpoint names, DBOS references, backend terms, or "under the hood" asides.
- **Primary arc**: Detection → Response. Everything else is supporting.
- **Explicitly out of scope**: authentication / login / SSO / invite-accept / logout surfaces; the Settings modal's account contents; API reference; CLI; deployment/infra.

## Terminology lock

These are enforced across all pages — do not deviate without updating this file and `AGENTS.md`:

- Agent output noun = **Investigation**. The markdown body it produces = **Report**.
- Verdicts = **Positive / Negative / Uncertain**. Never "fraud/legitimate."
- Case dispositions = **True positive / False positive / Inconclusive** (plus UI-visible subcategories).
- Top-level areas = **Detection / Response / Sources** (sidebar labels). Not "Mission Control," not "Cases," not "Data Sources."
- Automation tab contains **playbooks**. "Playbook" = the artifact; "Automation" = the tab/concept, matching the UI.
- **Never use "alert"** as a product noun — the demo branch is removing that concept. If a UI drawer still uses the label, document around it (e.g. "investigation detail" or the current UI text); flag the inconsistency instead of propagating it.

## Chat assistant cross-references

The Chat assistant is a primary analyst tool and is the standard escape hatch for "I'm stuck." When authoring a feature page that involves interpretation, decisions, or dense UI (e.g. reading an investigation, choosing a disposition, connecting a source, navigating the case workspace), **include a chat reminder** — import the reusable snippet at `snippets/chat-reminder.mdx` rather than re-typing it:

```mdx
import ChatReminder from '/snippets/chat-reminder.mdx'

<ChatReminder />
```

Place the reminder at the very top of the page, right after the frontmatter — so users see it before they start reading. Skip it on purely introductory/conceptual pages (welcome, core-concepts, navigating) and on the Chat assistant pages themselves.

## Per-page content template

Every Guides page follows this structure:

```mdx
---
title: '<Page title>'
description: '<One sentence>'
---

<One-sentence "what this screen is for">

<Frame>
  <img src="/images/<slug>.png" alt="<Page name>" />
</Frame>

## What you can do here

- Bullet of every meaningful action
- …

## Walkthrough: <common flow>

<Steps>
  <Step title="…"> … </Step>
</Steps>

## Reference

Every interactive element on the screen as a bolded-name + plain-English definition.

## Tips (optional)
```

No "Under the hood" section. No endpoint names.

## Common commands

- `mint dev` — local preview at `http://localhost:3000` (run from repo root). Requires Node 19+ and `npm i -g mint`.
- `mint dev --port 3333` — override port.
- `mint broken-links` — validate all internal links; run before PRs that move or rename pages.
- `mint update` — update CLI if the local preview drifts from production.

Deployment is automatic: pushing to `main` triggers the Mintlify GitHub app.

## Authoring workflow

- **Adding a page**: create the `.mdx` file, then add its slug to the appropriate `groups[].pages` array in `docs.json`. A page on disk but not referenced in `docs.json` won't appear in nav.
- **Page slug** = path without `.mdx`, relative to repo root (e.g. `detection/runs`).
- **Drafts**: placeholder pages currently carry a `<Note>Draft — content pending</Note>` callout; replace the callout when authoring.
- **Screenshots**: the user captures these. Reference predictable paths like `/images/<slug>.png` so shots can be dropped in without editing MDX.
- **Reusable snippets** live in `snippets/`. The canonical verdict/disposition phrasing is in `snippets/verdict-values.mdx` — import it rather than re-typing the labels.

## Mintlify component reference

Mintlify-specific component syntax (`<Steps>`, `<AccordionGroup>`, `<Frame>`, etc.), `docs.json` schema, and writing standards are covered by the `mintlify` / `mintlify-docs` skills (see `skills-lock.json`). Invoke them rather than guessing component names.
