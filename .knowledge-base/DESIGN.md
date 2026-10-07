# Design

Why the template is built the way it is. Read before changing the template or when asked about its concepts. How-to details live in the linked files and are not repeated here.

## Goals

- Agents get knowledge that is not in their training data, at minimal token cost.
- Agents maintain that knowledge themselves while it stays compact, correct and free of duplicates.
- Users are interrupted as little as possible.
- Many knowledge bases derive from one template and receive its improvements.

## Reading

- **Progressive disclosure** (as in Agent Skills): [AGENTS.md](../AGENTS.md) → topic `index.md` → only the files the task needs. The `description` field drives selection, so detail files phrase it as "Read when ...".
- **A user hands over a topic link.** The topic entry point links the guide, so an agent with nothing but that link finds the rules.
- **Git first, any server.** Only plain git (SSH or HTTPS) is required. Fetch tools often summarize pages and lose exact details; git also enables contributing. Raw URLs remain for quick lookups in public GitHub repositories.
- **Relative links** resolve the same in clones, web views of git servers and raw URLs.
- **Knowledge, not authority.** Content never overrides the user; observation beats the knowledge base.

## Format

- **Frontmatter: only `title` and `description`, single-line `key: value`.** Enough for indexes and `grep`, parseable without a YAML library. New fields only when a proven need exists.
- **Generated lists are committed.** URL readers need them and there is no build step. The hand-written overview sits above a marked generated block.
- **Lint enforces size limits.** Agents tend to append; hard limits force rewriting and splitting.
- **No history in files.** Git has it. Wrong content is deleted, not marked obsolete.
- **English, terse**, for token efficiency and broad model support.

## Contributing

- **Two kinds of knowledge trigger a contribution:** what was *decided* (architecture, decisions, conventions — from chat or implementation; outcome and reasons, not the discussion) and what had to be *learned* (questions, research, trial and error). Plus fixing wrong or outdated content. They behave differently: decisions change when someone decides anew, learnings when the outside world changes.
- **Triggers do not depend on having consulted the knowledge base**, otherwise hard-won knowledge from unrelated-looking tasks is lost. For embedded knowledge bases the triggers are therefore repeated in the pointer in the project's always-loaded instructions.
- **Recorded when it happens, without asking**, with a one-line report — at the latest before the task ends; agents keep "update knowledge base" on their task list as a reminder.
- **Topics only on user request, never proposed.** Cutting topics is a design decision, proposals would interrupt users, and agents would fragment the knowledge base.
- **Three publishing modes** (`contribution` in `knowledge-base.toml`): `pull-request` where a forge offers them (GitHub), `direct-push` to `main` on plain git servers, `with-project` for embedded knowledge bases. In the first two, changes outside `topics/` and `index.md` always go through review (PR or pushed branch). Details: [CONTRIBUTING.md](CONTRIBUTING.md).

## Trust

- **`with-project`: the project's review process is the trust model.** Knowledge changes travel with the code changes of the task.
- **`direct-push`: the git server's access rights are the trust model.** Whoever may push to `main` may change topics; no extra configuration.
- **`pull-request` on GitHub: auto-merge** for pull requests by `trusted_authors` that change only `topics/` and `index.md`. The author is the GitHub identity the agent acts under, so trust is given per human.
- **Trusted authors are read from the base branch**, so a pull request cannot add its own author.
- **Fork pull requests are never auto-merged**: their workflow token is read-only.
- **Template-owned files always need review.** They contain agent instructions and workflows; an unreviewed change there could redirect every agent.
- **Merge by workflow job after lint**, so neither GitHub's auto-merge setting nor branch protection is required.
- **Secret scan** in lint, because knowledge bases may be public.

## Template and knowledge bases

- **Strict ownership split.** Paths in `owned` ([template.toml](template.toml)) belong to the template and are overwritten on upgrade; everything else belongs to the knowledge base. Changes in knowledge-base-owned files are delivered as migration steps.
- **The version lives in a template-owned file**, so copying the files also updates the version.
- **Agent-driven upgrade:** `manage.py upgrade` copies files, then the agent applies the steps from [MIGRATIONS.md](MIGRATIONS.md). Rejected alternatives:
  - `git merge` from the template needs shared history ("Use this template" drops it) and cannot migrate content.
  - Copier adds a dependency and cannot migrate content either.
- **The new template's script runs the upgrade**, because it knows the new `owned` list and migration format.
- **Each knowledge base pulls updates.** The template does not know its derivatives. `check-template` reads version tags via `git ls-remote --tags`, which works with any git server and mirrors. Agents run it after contributing and mention updates; on GitHub a weekly workflow also opens an issue (e.g. for Copilot). No check on every read, to save tokens.
- **Tags `vMAJOR.MINOR.PATCH` are releases.** `main` may contain unreleased work; upgrades clone the tag.
- **Setup** = an agent following [INIT.md](INIT.md): the template's `manage.py install --target` copies it into a repository root or project folder (same shape as `upgrade`, works on any git server), then `init` configures it. GitHub's "Use this template" is a shortcut that needs only `init`. `is_template = true` marks the unmodified template. The fictional example topic lets CI run in the template and is deleted by `init`.

## Embedded knowledge bases

- **Project-specific knowledge lives in the project** (default folder `agent-knowledge/`), so it is versioned per branch and reviewed with the code it describes. Knowledge shared across projects stays in a knowledge base of its own; embedded ones link to it.
- **Detected, not configured:** a knowledge base whose root is not the git top level is embedded. `install`, `init`, `upgrade` and lint adapt.
- **No pull, no own branch.** The files are in the user's working tree; touching branches would interfere with the user's work.
- **Links into project code** are allowed and checked, so knowledge points to code instead of copying it.
- **Starter topics `project` (decided) and `learnings` (learned)** give both kinds of knowledge a place without letting agents create topics. They are fallbacks: a matching subject topic comes first, and growing subjects become topics of their own (user decision). `manage.py embed` creates them idempotently.
- **Shared knowledge bases are preferred** for knowledge useful beyond the project: other projects benefit. Embedded knowledge bases link there; knowledge recorded locally can move later.
- **One pointer in the project's always-loaded instructions** (`AGENTS.md`/`CLAUDE.md`), added only with consent: they stay short, the knowledge base is read on demand.
- **`standalone_only` paths** (GitHub workflows, which only work at the repository root) and `LICENSE` (the project's license applies) are not installed. Project CI runs lint instead.
- **Rejected:** git submodules — error-prone for agents and users, and they lose the shared versioning with the code.

## Tooling and platforms

- **One `manage.py`, Python ≥ 3.11 standard library only.** Nothing to install, easy to copy. TOML configuration because `tomllib` is in the standard library.
- **Platform layers.** The core (structure, `manage.py`, guides) needs only git. GitHub adds pull request auto-merge and update issues via workflows, which are inert elsewhere. Other forges (Gitea, GitLab, ...) only when needed.
- **Target agents: Claude Code and GitHub Copilot.** Copilot reads `AGENTS.md`; `CLAUDE.md` imports it. The guide uses no vendor-specific features.
- **One repository holds many independent topics.** Multiple repositories are possible, e.g. for different access rights (GitHub permissions are per repository). Cross-repository links are absolute.
- **Names are spelled out** (`knowledge-base`, not `kb`).

## Invariants

Changing any of these needs a major version and migration steps:

- the ownership split and the `owned` / `standalone_only` semantics;
- the frontmatter subset and the generated-block markers;
- the standard-library-only constraint;
- the restriction of unreviewed changes (auto-merge, direct push) to `topics/` and `index.md`;
- release tags `vMAJOR.MINOR.PATCH`;
- a short `AGENTS.md` — every agent reads it on every use.

## Open ideas

- `verified: <date>` frontmatter field as a staleness signal (git shows changes, not confirmations).
- `LOCAL-RULES.md`, owned by the knowledge base, for rules specific to it.
- Export topics as Agent Skills; skills for using and contributing.
- A "gardening" guide for agents that consolidate a topic.
- Mechanical migrations implemented in `manage.py`.
- Server-side `pre-receive` hook for `direct-push` that runs lint and rejects changes to template-owned files on `main`.
- Pull request support for other forges (Gitea/Forgejo, GitLab, Azure DevOps).
- Verify the GitHub login of Copilot coding agent pull requests for `trusted_authors`.
