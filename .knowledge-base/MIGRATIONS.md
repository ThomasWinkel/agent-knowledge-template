# Migrations

Steps an agent applies to a knowledge base during an upgrade, in addition to the file replacement done by `manage.py upgrade`. One `## <version>` section per template version (headings are parsed by the script). Write "None." when a version needs no steps.

## 0.3.0

No steps. New: knowledge bases embedded in a project (`manage.py install`, `contribution = "with-project"`).

## 0.2.0

Works with any git server; template updates are detected via version tags.

1. In `knowledge-base.toml`, add below `repository` (without it, `pull-request` is assumed):
   ```toml
   # How agents publish changes:
   #   "pull-request" - pull requests; auto-merged for trusted_authors (GitHub), otherwise reviewed
   #   "direct-push"  - push to main; the git server's access rights decide who may write
   contribution = "pull-request"
   ```
   Use `"direct-push"` if the knowledge base is not on GitHub or the user prefers pushing to `main`.

## 0.1.0

Initial version. None.
