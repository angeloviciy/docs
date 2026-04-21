# Conway docs — authoring instructions

## About this project

Mintlify documentation site for the Conway frontend, aimed at **non-technical fraud analysts**. Product source of truth: `~/Documents/Github/conway` (branch `rohan/demo-frontend-tweaks`). Two tabs: **Guides** (product walkthrough) and **Pilot** (per-deployment templates).

- Pages are MDX with YAML frontmatter.
- Navigation lives in `docs.json`.
- `mint dev` to preview locally; `mint broken-links` to check links.
- See `CLAUDE.md` for the per-page template and the terminology lock.

## Terminology

These terms are locked. Any PR that introduces a synonym should be rejected.

| Concept | Correct term | Do not use |
|---|---|---|
| Agent analysis output (the thing) | **Investigation** | Alert, anomaly report, finding |
| The markdown body an investigation produces | **Report** | Write-up, summary, narrative |
| Verdict on an investigation | **Positive / Negative / Uncertain** | Fraud / Legitimate, Confirmed / Rejected |
| Case outcome | **True positive / False positive / Inconclusive** | Confirmed / Denied, Real / Fake |
| Subcategory under a disposition | Use the exact label shown in the UI (e.g. "ATO via credential stuffing") | Paraphrases |
| Top-level section (sidebar item) | **Detection / Response / Sources** | Mission Control, Cases, Data Sources |
| Automation artifact | **Playbook** | Workflow, rule set, automation (as a noun for the artifact) |
| Automation surface | **Automations tab** (inside Response) | Playbooks page |
| Agent-saved observation | **Intelligence** (item) | Memory, note, insight, learning |
| Extracted actor (person / account / device / IP) | **Entity** | Subject, record |
| Detection execution | **Run** | Job, scan, batch |

**Never use "alert"** as a product noun. The current demo branch is removing alerts as a first-class UI concept. If a drawer or panel still carries the label, document it using whatever text appears on screen and flag the inconsistency; do not propagate the term into prose.

## Style preferences

- Active voice, second person ("you").
- One idea per sentence.
- Sentence case for headings.
- Bold for UI elements: `Click **Create run**`.
- Code formatting for file names, commands, paths. **No backend path names** (`/api/v1/...`) in Guides content — those belong only in internal `CLAUDE.md`.
- Prefer `<Steps>` walkthroughs + `<Frame>` screenshots over prose dumps.
- Lead with what the reader wants to accomplish, then the steps.

## Content boundaries

**In scope:** everything a fraud analyst sees on `/detection/*`, `/response/*`, `/sources`, `/runs/:id`, and the Chat panel. Exhaustive per-page references.

**Out of scope:**
- Authentication, SSO, magic links, invite-accept, logout.
- The Settings modal's account contents (profile, org, user ID, log out).
- API reference / endpoint names.
- CLI (`conway_cli`).
- Deployment, infra, Docker, Kubernetes.
- Any developer-only concept (DBOS queues, run_phase state machine internals, DuckDB workspaces, etc.).

Admin-only surfaces (e.g. the Integrations tab inside Sources) get one page each and are explicitly flagged as "visible to admins only" — no deeper admin guide.

## Pilot tab rules

Pages under `pilot/` are **templates filled in per deployment**. A Pilot page in `main` should always contain:
1. A short header describing what goes there.
2. A `<Note>` callout reading "This page is customized for your deployment. A Conway team member will populate it for you."
3. Placeholder headings only — no client-specific details committed to `main`.

Customization happens on a per-deployment branch or fork. Never commit client-specific names, credentials, or playbook content to `main`.
