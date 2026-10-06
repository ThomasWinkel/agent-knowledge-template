# Agent Knowledge Template

Template for knowledge bases that AI agents (Claude Code, GitHub Copilot) read **and maintain themselves**. A knowledge base holds condensed knowledge that is not in the models' training data — internal processes, private APIs, conventions, verified recipes — organized in independent topics.

## How it works

- **Topics** live in `topics/<topic>/`. Each has an entry point `index.md` (overview, key facts, generated list of files with "read when" descriptions) and compact Markdown files with a small YAML frontmatter (`title`, `description`).
- **Usage:** give an agent the link to a topic's `index.md`. It reads [AGENTS.md](AGENTS.md) first, then the entry point, then only the files the task needs.
- **Maintenance:** after a successful task, an agent fixes wrong or outdated knowledge and adds what was missing, then opens a pull request. Topics are only created when a user asks. Rules: [.knowledge-base/CONTRIBUTING.md](.knowledge-base/CONTRIBUTING.md).
- **Quality gates:** `manage.py lint` (Python standard library) checks frontmatter, links, sizes, generated indexes and common secret formats. Pull requests by trusted authors that only change topics are merged automatically; everything else is reviewed.
- **Template updates:** each knowledge base knows its template version. A weekly workflow opens an issue when a newer version exists; an agent performs the upgrade by following [.knowledge-base/UPGRADE.md](.knowledge-base/UPGRADE.md).

## Create a knowledge base

1. On GitHub: **Use this template** → create a repository.
2. Clone it and tell an agent (Claude Code or Copilot) in the clone: "Set up this knowledge base." It follows [.knowledge-base/INIT.md](.knowledge-base/INIT.md).
3. Create topics by asking an agent; afterwards point agents at the topic links.

## Repository layout

| Path | Owner | Purpose |
|---|---|---|
| `AGENTS.md`, `CLAUDE.md` | template | Guide for agents (`CLAUDE.md` imports `AGENTS.md` for Claude Code) |
| `.knowledge-base/` | template | `manage.py`, contribution, setup and upgrade guides, migrations |
| `.github/workflows/knowledge-base-*.yml` | template | Lint and auto-merge, weekly template check |
| `knowledge-base.toml` | knowledge base | Name, description, repository, trusted authors |
| `README.md`, `index.md`, `topics/` | knowledge base | Content (`index.md` is generated) |

Template-owned files are replaced on upgrade; do not edit them in a knowledge base.

## Developing the template

Design decisions and their reasons: [.knowledge-base/DESIGN.md](.knowledge-base/DESIGN.md). Working on the template: [.knowledge-base/TEMPLATE-DEVELOPMENT.md](.knowledge-base/TEMPLATE-DEVELOPMENT.md).
