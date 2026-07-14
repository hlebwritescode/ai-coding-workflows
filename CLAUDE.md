# ai-coding-workflows

A personal, curated collection of AI coding workflows — primarily [Claude Code](https://claude.com/claude-code) **skills** — used in daily work (both professional and personal development).

Most content here is **imported and adapted from external sources** (starting with [mattpocock/skills](https://github.com/mattpocock/skills)) rather than written from scratch. This file tells an AI agent **how to treat and work with this repo** — its purpose, structure, and the conventions to follow when adding to it.

## What this repo is (and isn't)

- It **is** a library of reusable AI workflows the owner pulls into their real projects.
- It **is not** an application. There's no build, no tests, no runtime. Don't look for or invent them.
- Skills here are meant to be portable — copied or referenced into other repos — so each one should be self-contained.

## Structure

Kept intentionally minimal. Add structure only when it earns its place, not preemptively.

```
CLAUDE.md              # this file — how to work with the repo
skills/
  <skill-name>/
    SKILL.md           # the skill definition
```

- Skills live under `skills/`, one directory per skill, directory name matching the skill's `name`.
- **No category tier** (e.g. no `skills/productivity/`) for now. Introduce one only if the collection grows enough to need it, and only after checking with the owner.

## Skill conventions

Follow the format used by the upstream source so imported skills stay compatible:

- Each `SKILL.md` starts with YAML frontmatter. Required keys:
  - `name` — matches the directory name.
  - `description` — written to be trigger-phrase-rich, since it's what an agent matches on for auto-invocation.
- **Two-tier pattern** (from upstream, preserved here):
  - **Model-invoked** skills are the reusable engines. They can be auto-selected by the agent when a task matches their `description`, or invoked explicitly. No special frontmatter flag.
  - **User-invoked** skills are thin wrappers meant to be called explicitly (e.g. `/grill-me`). They carry `disable-model-invocation: true` and typically just delegate to a model-invoked skill.
  - A user-invoked skill may call a model-invoked one, but never another user-invoked one.

## How to import / add a skill

The owner adds things step by step. When asked to bring in a new skill:

1. **Investigate the source first.** Fetch and read the real upstream file(s). Identify what it does and any dependencies (referenced skills, shared scripts/assets, repo-wide conventions). Don't blind-copy.
2. **Discuss before implementing** when there's a real choice (structure, whether to fold wrappers, adaptation vs. verbatim). Recommend a default; let the owner decide.
3. **Preserve exact text** for skill bodies unless adaptation was agreed — these are carefully worded prompts; paraphrasing degrades them.
4. **Record provenance** in the table below so it's always clear what came from where.
5. Commit with a clear message; push to the working branch.

## Imported skills

| Skill | Type | Source | Notes |
|-------|------|--------|-------|
| `grilling` | model-invoked | [mattpocock/skills](https://github.com/mattpocock/skills/blob/main/skills/productivity/grilling/SKILL.md) | The interview engine. v1.1 text (confirmation gate + facts/decisions split). Local addition: ask in plain text, never the input-options widget. |
| `grill-me` | user-invoked | [mattpocock/skills](https://github.com/mattpocock/skills) | Thin `/grill-me` wrapper that runs a `grilling` session. |
| `setup-hleb-skills` | user-invoked | [mattpocock/skills](https://github.com/mattpocock/skills/tree/main/skills/engineering/setup-matt-pocock-skills) | One-time repo-setup bootstrap that scaffolds per-repo config (issue tracker, triage labels, domain docs) for the skills that need it. Imported ahead of those skills. Adaptations: GitLab option and its template removed; "engineering skills" → "hleb skills". Ships seed templates: `issue-tracker-github.md`, `issue-tracker-local.md`, `triage-labels.md`, `domain.md`. Section B (triage labels) stays dormant until `triage` is imported. |

## The workflow this fits into

Grilling is step one of a larger pipeline (upstream context, for when later stages get imported):

```
(wayfinder) → grilling → to-spec → to-tickets → implement → code-review
```

- **wayfinder** — on-ramp for work too big/foggy for one session; treats grilling as a ticket type.
- **grilling** — sharpen a raw idea via relentless one-question-at-a-time interview.
- **to-spec** — synthesize the discussion into a written spec.
- **to-tickets** — slice the spec into agent-sized tickets.
- **implement** / **code-review** — build each ticket, then review.

Only `grilling` and `grill-me` are imported so far.
