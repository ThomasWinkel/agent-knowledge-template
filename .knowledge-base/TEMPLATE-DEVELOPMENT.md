# Developing the template

Applies only to the template repository (`is_template = true`), not to knowledge bases derived from it. Read [DESIGN.md](DESIGN.md) first: it records the decisions, rejected alternatives and invariants. Update it when a decision changes.

- **Ownership.** Files listed under `owned` in `template.toml` belong to the template and are overwritten in every knowledge base on upgrade. Everything else (`knowledge-base.toml`, `README.md`, `LICENSE`, `index.md`, `topics/`, `.gitignore`, `.gitattributes`) belongs to the knowledge base; changes there reach existing knowledge bases only through migration steps.
- **Example topic.** `topics/example/` is fictional, shows the format and lets CI run in the template. `manage.py init` deletes it.
- **Releasing a change** that affects knowledge bases:
  1. Bump `version` in `template.toml` (semver: major = manual content migration needed, minor = new features, patch = fixes).
  2. Add a `## <version>` section to `MIGRATIONS.md` with exact, executable steps — or "None.". Prefer making `manage.py` perform mechanical migrations and keep the agent steps short.
  3. Commit, push, then tag the commit and push the tag: `git tag v<version> && git push origin v<version>`. The tag is the release: `manage.py check-template` in knowledge bases finds it via `git ls-remote --tags`, and upgrades clone it.
- **Constraints.** `manage.py` uses the Python ≥ 3.11 standard library only. Frontmatter stays a single-line `key: value` subset. Keep `AGENTS.md` short: every agent reads it on every use.
- **Testing.** `python .knowledge-base/manage.py lint` must pass. Test `init` and `upgrade` on a copy in a temp directory, never on this repository.
