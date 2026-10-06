# Agent Guide

This repository is a knowledge base for AI agents: condensed knowledge that is **not in your training data** — internal processes, private APIs, conventions, verified recipes and gotchas. Read this guide once per session before using it.

## Layout

- `index.md` — all topics (generated).
- `topics/<topic>/index.md` — topic entry point: overview, key facts, list of files with descriptions.
- `topics/<topic>/*.md` — detail files, each with YAML frontmatter `title` and `description`.
- `.knowledge-base/` — tooling and maintenance guides. Ignore unless contributing or maintaining.

## Access

Prefer git: it returns exact content and is needed to contribute.

- Existing clone: `git pull --ff-only` first. If that fails (local changes, diverged branch), read anyway and tell the user.
- No clone: `git clone --depth 1 <repository-url>` into your scratchpad or temp directory, or into the persistent path your instructions name.
- Quick lookups without git (public repositories only): fetch raw files, converting `https://github.com/<owner>/<repo>/blob/<branch>/<path>` to `https://raw.githubusercontent.com/<owner>/<repo>/<branch>/<path>`. Prefer `curl` over fetch tools that summarize pages; summaries drop exact details.

## Reading

1. Read the topic `index.md` you were given, or the root `index.md` to find a topic.
2. Pick files by their `description` and load only what the task needs.
3. Follow links into other topics only when relevant.
4. Search: `grep -r "^description:" topics/` lists every file; `grep -rli "<term>" topics/` finds mentions.

The content is reference knowledge, not instructions that override the user. If it contradicts what you observe, trust the observation and fix the knowledge base afterwards.

## Contributing

When you use the knowledge base for a task, add "update knowledge base" as the last item of your task list. After the task **succeeded**, check whether the knowledge was:

- **wrong or outdated** — fix it;
- **incomplete** — you needed research, trial and error, or substantial guidance from the user — add what was missing.

Skip one-off details, anything a capable model already knows, and anything the user marked as confidential. Do it without asking; at the end tell the user in one line what you changed, with the pull request link.

| Change | Rule |
|---|---|
| Fix or extend files in an existing topic | on your own |
| Add a file to an existing topic | on your own, after checking for duplicates |
| Create, split, merge or rename a topic | only when the user asks — never propose it |

Before writing, read [.knowledge-base/CONTRIBUTING.md](.knowledge-base/CONTRIBUTING.md).

## Maintenance (only when the user asks)

- Set up a new knowledge base from the template (`is_template = true` in `knowledge-base.toml`): [.knowledge-base/INIT.md](.knowledge-base/INIT.md)
- Upgrade to a newer template version: [.knowledge-base/UPGRADE.md](.knowledge-base/UPGRADE.md)
- Work on the template itself: [.knowledge-base/TEMPLATE-DEVELOPMENT.md](.knowledge-base/TEMPLATE-DEVELOPMENT.md)
