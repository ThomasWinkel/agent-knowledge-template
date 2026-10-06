# Upgrade to a newer template version

Usually triggered by an issue "Template update available" opened by the template check workflow, or by the user.

1. In the knowledge base clone: `git fetch origin && git switch -c knowledge/template-upgrade origin/main`.
2. Clone the template (repository from `.knowledge-base/template.toml`) next to it, e.g. into your scratchpad:
   `git clone --depth 1 <template-repository> <scratchpad>/template`
3. Run the **new** template's script against the knowledge base:
   ```sh
   python <scratchpad>/template/.knowledge-base/manage.py upgrade --target <knowledge-base-path>
   ```
   It replaces all template-owned files (listed under `owned` in `template.toml`) and prints the migration steps from `MIGRATIONS.md` for every version in between.
4. Apply the printed migration steps in order.
5. In the knowledge base: `python .knowledge-base/manage.py index`, then `python .knowledge-base/manage.py lint`; fix all errors.
6. Review `git diff`. Local edits to template-owned files are overwritten by design; if any were lost, tell the user.
7. Commit ("Upgrade template to <version>"), push, open a PR referencing the issue (`Closes #<number>`). Template upgrades change workflows and agent instructions, so they are never merged automatically.
