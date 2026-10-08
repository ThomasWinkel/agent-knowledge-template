# Agent Knowledge Template

> **Agents:** to set up a knowledge base from this template — as its own repository or embedded in a project — follow [.knowledge-base/INIT.md](.knowledge-base/INIT.md).

Template for knowledge bases that AI agents (Claude Code, GitHub Copilot) read **and maintain themselves**. A knowledge base holds condensed knowledge that is not in the models' training data — internal processes, private APIs, conventions, verified recipes — organized in independent topics.

## How it works

- **Own repository or embedded:** a knowledge base is a repository of its own (shared knowledge, e.g. internal APIs and processes) or a folder such as `agent-knowledge/` inside a project repository (project knowledge, versioned and reviewed with the code).
- **Topics** live in `topics/<topic>/`. Each has an entry point `index.md` (overview, key facts, generated list of files with "read when" descriptions) and compact Markdown files with a small YAML frontmatter (`title`, `description`).
- **Usage:** give an agent the link to a topic's `index.md`, or the repository URL plus a topic name. It reads [AGENTS.md](AGENTS.md) first, then the entry point, then only the files the task needs. A link is for the task at hand; on request the agent includes a knowledge base or topics permanently in a project: an entry in an "## Agent knowledge" section of the project's agent instructions ([.knowledge-base/CONNECT.md](.knowledge-base/CONNECT.md)).
- **Maintenance:** after a successful task, an agent fixes wrong or outdated knowledge and adds what was missing, then opens a pull request, pushes to `main`, or (embedded) includes the change in the task's own commit, depending on the `contribution` setting. Topics are only created when a user asks. Rules: [.knowledge-base/CONTRIBUTING.md](.knowledge-base/CONTRIBUTING.md).
- **Any git server:** the core needs only git (SSH or HTTPS) and Python. On GitHub, workflows add lint checks, auto-merge of pull requests by trusted authors and update issues.
- **Quality gates:** `manage.py lint` (Python standard library) checks frontmatter, links, sizes, generated indexes and common secret formats. Only topic changes are published without review.
- **Template updates:** each knowledge base knows its template version and finds newer release tags via git. Agents mention updates after contributing (on GitHub a weekly workflow also opens an issue); an agent performs the upgrade by following [.knowledge-base/UPGRADE.md](.knowledge-base/UPGRADE.md).

## Create a knowledge base

Tell an agent (Claude Code or Copilot), for example:

- in a project: "Add agent knowledge from https://github.com/ThomasWinkel/agent-knowledge-template to this project."
- in a clone of a new, empty repository: "Set up a knowledge base here from https://github.com/ThomasWinkel/agent-knowledge-template."

The agent follows [.knowledge-base/INIT.md](.knowledge-base/INIT.md). On GitHub you can also click **Use this template** and then ask an agent in the clone to "set up this knowledge base". Afterwards, ask agents to create topics and point them at the topic links.

## Use a knowledge base in a project

- For one task: give the agent the link to a topic or the knowledge base.
- Permanently: "Include topic billing-api from https://github.com/acme/knowledge in this project." The agent adds an entry to the project's `AGENTS.md`/`CLAUDE.md` ([.knowledge-base/CONNECT.md](.knowledge-base/CONNECT.md)). Rules stay in the knowledge base, so its template upgrades need no change in the project.

## Repository layout

| Path | Owner | Purpose |
|---|---|---|
| `AGENTS.md`, `CLAUDE.md` | template | Guide for agents (`CLAUDE.md` imports `AGENTS.md` for Claude Code) |
| `.knowledge-base/` | template | `manage.py`, contribution, setup and upgrade guides, migrations |
| `.github/workflows/knowledge-base-*.yml` | template | GitHub only, not installed when embedded: lint and auto-merge, weekly template check |
| `knowledge-base.toml` | knowledge base | Name, description, repository, contribution mode, trusted authors |
| `README.md`, `index.md`, `topics/` | knowledge base | Content (`index.md` is generated) |

Embedded in a project, all of this lives in the knowledge base folder. Template-owned files are replaced on upgrade; do not edit them in a knowledge base.

## Developing the template

Design decisions and their reasons: [.knowledge-base/DESIGN.md](.knowledge-base/DESIGN.md). Working on the template: [.knowledge-base/TEMPLATE-DEVELOPMENT.md](.knowledge-base/TEMPLATE-DEVELOPMENT.md).
