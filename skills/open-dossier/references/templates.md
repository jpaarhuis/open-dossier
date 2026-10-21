# Open Dossier — file templates

Templates for every maintained file in a dossier. Adapt headings to the dossier language (set in DOSSIER.md), keep the structure. Placeholders in `[brackets]`. Prose inside the code fences (like "Append-only journal…") is real file content — keep it. Table rows with placeholders only illustrate the format: when seeding a new dossier, write the header rows and leave the tables empty.

## Contents

- [AGENTS.md](#agentsmd)
- [DOSSIER.md](#dossiermd)
- [briefing.md](#briefingmd)
- [index.md](#indexmd)
- [log.md](#logmd)
- [Source analysis](#source-analysis-sourcesanalysesslugmd)
- [Wiki page](#wiki-page-wikinamemd)
- [Plan](#plan-plansnn-topicmd)
- [actions.md](#actionsmd)
- [decisions.md](#decisionsmd)
- [backlog.md](#backlogmd)
- [terms.md](#termsmd)
- [Dossier registry (~/.open-dossier/dossiers.md)](#dossier-registry-open-dossierdossiersmd)

---

## AGENTS.md

```markdown
# AGENTS.md

This folder is an open-dossier: a source-driven knowledge dossier, not a codebase.

Read [DOSSIER.md](DOSSIER.md) first — it is the schema (scope, actors,
conventions, way of working). Then [briefing.md](briefing.md) for the current
state and [index.md](index.md) for the catalog.

Work according to the open-dossier skill (`/open-dossier ingest|query|lint|brief`).
Never modify originals in `sources/originals/`.
```

## DOSSIER.md

```markdown
# [Dossier name] — schema

[One paragraph: what this dossier is about and what it is for.]

## Scope and purpose

- Domain: [what subject area]
- Purpose: [knowledge only / knowledge + execution (plans & actions)]
- Language: [language of the dossier; label vocabulary if mapped, e.g. [SOURCE] → [BRON]]

## Actors

| Actor | Role | Notes |
|---|---|---|
| [name/org] | [role] | [context] |

## Recognition

How a tool that routes sources (a meeting recorder, a mail filter) knows a meeting or mail belongs here. Leave out what does not apply.

| Kind | Value | Notes |
|---|---|---|
| Mail domain | [client.example] | [organization] |
| Meeting | [subject or series, e.g. "Weekly Atlas steering"] | [who organizes it] |
| Term | [a word only this dossier uses, e.g. the project name] | |

## Conventions

- Labels: [SOURCE] literal from a source · [INTERPRETATION] our analysis · [HYPOTHESIS] to be confirmed · [DECISION] settled choice
- Meeting status words (State of play): decided · agreed · open · discussed [map once for another language]
- Source IDs S001…, action IDs A001…, sequential, never reused; a point in a meeting is cited as S00X @HH:MM
- Dates absolute (YYYY-MM-DD)
- Originals in sources/originals/ are immutable

## How to work in this dossier

Follow the open-dossier operations: ingest new sources through the full loop
(convert → register → analyze → compound wiki → update plans/actions → update
briefing/index/log). Answer questions from index.md and briefing.md first.
```

## briefing.md

```markdown
# Briefing — [dossier name]

Living synthesis. Last updated: [YYYY-MM-DD].

## Purpose

[2–3 sentences.]

## Current state

[INTERPRETATION] [Where things stand now; what changed recently.]

## Key facts

- [SOURCE] [fact] (S00X)
- [DECISION] [choice made] ([date])

## Open points

- [HYPOTHESIS] [assumption awaiting confirmation]
- [open question → backlog.md]
```

## index.md

```markdown
# Index

Catalog of every page in this dossier. One line each; update on every ingest.

## Source register

| ID | Source | Origin | Type | Converted | Analysis | Status |
|---|---|---|---|---|---|---|
| S001 | [title, date, author] | [who/where from] | [report/transcript/email/…] | [path] | [path] | [new/converted/analyzed/processed] |

## Wiki pages

| Page | About | Updated |
|---|---|---|
| [wiki/name.md] | [one line] | [date] |

## Plans

| Plan | Topic | Status |
|---|---|---|
| [plans/01-topic.md] | [one line] | [draft/active/done] |
```

## log.md

```markdown
# Log

Append-only journal. Newest entries at the bottom; never rewrite old entries.

- [YYYY-MM-DD HH:MM] ingest S001: [one-line gist]. Touched: [pages].
- [YYYY-MM-DD HH:MM] query: "[question]" → filed synthesis in [page].
- [YYYY-MM-DD HH:MM] lint: [n] issues found, [m] fixed, rest in backlog.
```

## Source analysis (sources/analyses/[slug].md)

For a meeting, Summary and State of play come first, before Key points: see [meeting-sources.md](meeting-sources.md).

```markdown
# Analysis: [source title]

## Metadata

- Source: S00X — sources/converted/[file].md (original: sources/originals/[file])
- Origin: [who/where from] · Type: [type] · Date: [document date]

## Key points

- [SOURCE] [point]
- [INTERPRETATION] [what this means for the dossier]

## Decisions and hypotheses found

- [DECISION] [choice stated in the source] → logged in decisions.md
- [HYPOTHESIS] [assumption to confirm]

## Risks and open questions

- [INTERPRETATION] [risk] → backlog.md if action needed

## Follows from this source

- Plans: [created/updated plans/NN-topic.md]
- Actions: [A0XX added]
- Wiki: [pages created/updated]
```

## Wiki page (wiki/[name].md)

```markdown
# [Entity or topic name]

[One-line definition.]

## What we know

- [SOURCE] [claim] (S00X)
- [INTERPRETATION] [synthesis across sources]

## Relations

- [links to other wiki pages / plans]

## Open

- [HYPOTHESIS] / contradictions / gaps (mirror in backlog.md when actionable)
```

## Plan (plans/NN-topic.md)

```markdown
# [NN]. [Plan title]

## Goal

[What done looks like.]

## Origin

[SOURCE]/[DECISION] references: which sources or decisions this plan follows from.

## Scope

- In: …
- Out: …

## Approach

[Steps or phases.]

## People

[Who is involved, what role.]

## Milestones

- [YYYY-MM-DD] [meeting / deadline / go-live]

## Actions from this plan

- A0XX, A0YY (see actions.md)
```

## actions.md

```markdown
# Actions

Central action register. Every action traces to a source, plan or decision.

| ID | Action | Origin | Owner | Priority | Status | Due |
|---|---|---|---|---|---|---|
| A001 | [imperative description] | S001 / plan 01 | [name] | P0–P2 | open | YYYY-MM-DD |

Status: open · in progress · blocked · done · dropped
Priority: P0 = highest. Due = deadline; use — when there is none.
```

## decisions.md

```markdown
# Decisions

Settled choices and open hypotheses. A hypothesis that gets confirmed moves up
as a decision; a killed hypothesis gets status "dropped", never deleted.

| Date | Type | Statement | Rationale / origin | Status |
|---|---|---|---|---|
| YYYY-MM-DD | Decision | [choice] | [why; S00X or discussion] | active |
| YYYY-MM-DD | Hypothesis | [assumption] | [origin] | open |

Date: for a decision, when the choice was made; for a hypothesis, when it was logged.
```

## backlog.md

```markdown
# Backlog

## Sources to process

| Priority | Source | Origin | Why needed | Status |
|---|---|---|---|---|
| P0 | [document] | [where it should come from] | [reason] | to acquire / to ingest |

A document that an open action will deliver stays in actions.md; list it here
only once it arrives (to ingest) or must be chased separately (to acquire).

## Open questions

1. [question — who can answer it?]
```

## terms.md

The dossier's own words, spelled right: for a reader new to the dossier, and for tools that transcribe its meetings (a speech-to-text model gets them as a hint, so "ASR" is not written as "Architektur"). Short on purpose: the words that recur, most used first, at most about 60. People are in the Actors table, not here.

```markdown
# Terms

The names and abbreviations of this dossier as they are spelled, most used first.

| Term | What it is | Heard as |
|---|---|---|
| [ASR] | [insurer, client] | [Architektur, a-s-r] |
```

Term: the spelling as the organization writes it. Heard as: the forms a transcript or mail had wrongly, comma-separated; empty when none.

## Dossier registry (~/.open-dossier/dossiers.md)

One file per user, outside any dossier, so a session can answer across dossiers ("what did we agree with Karel last month?"). `init` adds a row; a tool that finds or creates dossiers may add one too; `lint` reports rows whose folder is gone.

```markdown
# Dossiers

The open-dossiers of this user, for queries across them. One row each.

| Dossier | Path | About |
|---|---|---|
| [name from DOSSIER.md] | [absolute path] | [one line] |
```
