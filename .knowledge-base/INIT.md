# Set up a new knowledge base

Precondition: the user created a repository from the template and you work in a clone of it; `knowledge-base.toml` contains `is_template = true`. On GitHub, the repository is created with "Use this template". On other git servers: clone the template, remove its `.git` folder, `git init`, and add the new repository as `origin`.

1. Ask the user in one message for:
   - name of the knowledge base,
   - one sentence describing its purpose,
   - only on GitHub with pull requests: GitHub logins whose pull requests may be merged automatically (default: the repository owner; add `Copilot` to auto-merge PRs of the Copilot coding agent).
2. Run:
   ```sh
   python .knowledge-base/manage.py init --name "<name>" --description "<purpose>" [--trusted-author <login> ...]
   ```
   It rewrites `knowledge-base.toml` and `README.md`, removes the example topic and regenerates `index.md`. The repository URL comes from `git remote get-url origin` (pass `--repository <url>` if that fails). `contribution` defaults to `pull-request` on GitHub and `direct-push` elsewhere; override with `--contribution`.
3. Run `python .knowledge-base/manage.py lint`, commit ("Initialize knowledge base") and push to `main`.
4. Tell the user, briefly:
   - `pull-request` on GitHub: auto-merge uses the workflow token. It fails if branch protection on `main` requires reviews, or if the organization restricts workflow permissions to read-only (Settings → Actions → General → Workflow permissions). A weekly workflow opens an issue when a new template version is available.
   - `direct-push`: whoever may push to `main` may change topics; agents push other changes as branches for review. Agents mention new template versions after contributing. The GitHub workflow files are inert on other servers.
   - `LICENSE` is the template's MIT license; adjust copyright holder or license for the knowledge base content if needed.
   - Optional: to let agents use the knowledge base without being given a link, add a line to the global agent instructions (`~/.claude/CLAUDE.md`, Copilot instructions), e.g.
     `Knowledge base "<name>" (<purpose>): <repository-url> — read AGENTS.md there before using it.`
   - New topics: ask an agent working in the clone to create them.
