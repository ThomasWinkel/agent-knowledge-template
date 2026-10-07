# Upgrade to a newer template version

Triggered by the user, by an agent's hint after contributing, or on GitHub by the issue "Template update available" from the weekly workflow.

1. In the knowledge base clone: `git fetch origin && git switch -c knowledge/template-upgrade origin/main`.
2. Find the latest version: `python .knowledge-base/manage.py check-template`.
3. Clone that release of the template (repository from `.knowledge-base/template.toml`), e.g. into your scratchpad:
   `git clone --depth 1 --branch v<version> <template-repository> <scratchpad>/template`
4. Run the **new** template's script against the knowledge base:
   ```sh
   python <scratchpad>/template/.knowledge-base/manage.py upgrade --target <knowledge-base-path>
   ```
   It replaces all template-owned files (listed under `owned` in `template.toml`) and prints the migration steps from `MIGRATIONS.md` for every version in between.
5. Apply the printed migration steps in order.
6. In the knowledge base: `python .knowledge-base/manage.py index`, then `python .knowledge-base/manage.py lint`; fix all errors.
7. Review `git diff`. Local edits to template-owned files are overwritten by design; if any were lost, tell the user.
8. Commit ("Upgrade template to <version>") and publish for review — never directly to `main`, because upgrades change agent instructions and tooling:
   - `pull-request`: open a PR (on GitHub referencing the issue: `Closes #<number>`); it is never merged automatically.
   - `direct-push`: push the branch (`git push -u origin HEAD`) and ask the user to review and merge it.
