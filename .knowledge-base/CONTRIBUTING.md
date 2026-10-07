# Contributing

Rules for writing to the knowledge base. Run commands from the repository root; use `python`, `python3` or `py` (Python ≥ 3.11).

## Principles

- **Only what an agent cannot know.** Not in training data, not trivially derivable from code or docs at hand. Link authoritative sources instead of copying them; record what they lack: undocumented behavior, the working recipe, gotchas.
- **One place per fact.** Search before writing (`grep -rli "<term>" topics/`). Extend the existing place or link to it; never repeat.
- **Edit, don't append.** Rewrite so the file reads as if written today. Delete what is wrong or obsolete. No changelogs, "updated on" notes or history — git has that.
- **Compact.** English, terse, imperative, bullets. Commands and code over prose. No introductions, no filler, no general knowledge.
- **Precise.** Exact names, paths, versions ("applies to API v2"). Prefer facts you verified during the task; mark unverified ones.
- **Durable.** Volatile values (current IDs, prices, people) go stale — describe where to look them up instead.
- **No secrets, no personal data.** Public repositories are public; lint scans for common token formats.

## File format

```markdown
---
title: Authentication
description: Read when obtaining or refreshing tokens, or debugging 401/403 responses.
---

# Authentication

Short summary line, then steps or reference.

## Gotchas

- ...
```

- Frontmatter: exactly `title` and `description`, single line each. Quote values containing `: ` or starting with YAML special characters.
- `description` is what agents choose files by. Detail files start with "Read when ...". Topic indexes state what the topic covers. Max ~200 characters.
- One concern per file. File and folder names: lowercase kebab-case.
- Size: warning above 300 lines, error above 500 — split into files or a subfolder.
- Links: relative paths (`../other-topic/file.md`). Other repositories: absolute URLs.
- Other files (examples, schemas, images) may live next to the Markdown files; link them.

## Topic entry point (`index.md`)

```markdown
---
title: Billing API
description: Internal billing REST API - authentication, endpoints, error semantics.
---

> **Agents:** read [the knowledge base guide](../../AGENTS.md) first.

# Billing API

2-5 sentences: what it is, when an agent needs it.

## Key facts

- The few facts needed for almost every task, with links to details.

<!-- BEGIN GENERATED CONTENTS ... -->
<!-- END GENERATED CONTENTS -->
```

- Hand-written part under ~80 lines. Never edit the generated block; `manage.py index` rewrites it from the frontmatter of the files.
- Large topics may have subfolders; each needs its own `index.md` of the same shape.

## New topic (only when the user asks)

A topic is an area an agent is pointed to as a whole: a system, an API, a process. Agree on slug and scope with the user, then:

```sh
python .knowledge-base/manage.py new-topic <slug> --title "<Title>" --description "<what it covers>"
```

Use `<slug>/<sub>` for a subtopic. Replace the TODO in the created `index.md`.

## Workflow

```sh
git fetch origin
git switch -c knowledge/<topic>-<short-description> origin/main
# edit files
python .knowledge-base/manage.py index
python .knowledge-base/manage.py lint
git add -A
git commit -m "<topic>: <what changed>"
git fetch origin && git rebase origin/main
```

- Fix all lint errors; address warnings where reasonable.
- Conflicts in generated blocks or `index.md`: take either side, then rerun `manage.py index`.
- Renamed or deleted a file: fix the links lint reports.

Publish according to `contribution` in `knowledge-base.toml`:

- **`pull-request`:** `git push -u origin HEAD`, then `gh pr create --fill` (without `gh`: give the user the link `git push` prints). Describe what was wrong or missing and how you verified it. PRs by `trusted_authors` that only change `topics/` and `index.md` are merged automatically once checks pass; all others wait for review. No push access: `gh repo fork --remote` and open the PR from the fork.
- **`direct-push`:** `git push origin HEAD:main`. If rejected because `main` moved: fetch, rebase, rerun `index` and `lint`, push again.
- **Either mode, changes outside `topics/` and `index.md`** (tooling, guides, configuration) always need a review: open a PR, or in `direct-push` mode push the branch (`git push -u origin HEAD`) and ask the user to review and merge it.

Finally run `python .knowledge-base/manage.py check-template`. If it reports an update, mention it in your final line to the user; ignore failures.
