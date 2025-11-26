# Wiki Sync Workflow

[[Home]] > Workflow Operations

This repository mirrors everything in `docs/wiki/` to the GitHub Wiki so that editing our docs stays in the same review flow as the codebase. The automation lives in `.github/workflows/sync_wiki.yml` and behaves as follows.

## When the workflow runs

- **Triggers**: any push to `main` that touches files under `docs/wiki/**`, or an on-demand `workflow_dispatch` run from the Actions tab.
- **Permissions**: the workflow grants itself `contents: write` so it can push directly to the Wiki repo.

## What the workflow does

1. Checks out the current repository at the commit that triggered the workflow.
2. Clones the Wiki repository (`<repo>.wiki.git`) into a local `wiki/` directory using the GitHub token that Actions provides.
3. Uses `rsync -av --delete` to copy everything from `docs/wiki/` into the freshly cloned `wiki/` folder.
   - `.git/` and `.nojekyll` inside `docs/wiki/` are ignored.
   - `--delete` ensures files removed from `docs/wiki/` are also removed from the Wiki.
4. If the sync created any changes, GitHub Actions configures the bot identity, commits with the message `Sync docs from main repo`, and pushes back to the Wiki repository.
5. If no changes were created, the workflow exits after printing “No changes to sync.”

## Editing workflow-managed docs

- **Where to edit**: add, rename, or delete wiki pages only under `docs/wiki/`. That folder is treated as the single source of truth.
- **Folder structure**: create subdirectories as needed; the workflow mirrors the full tree, so new folders automatically appear in the Wiki.
- **Front matter / assets**: include any Markdown, images, or other assets alongside the pages. Reference them with relative paths that stay within `docs/wiki/`.
- **Renames / deletions**: rename or remove the files in `docs/wiki/`, then commit the changes. Because rsync runs with `--delete`, the Wiki copy will match whatever remains in the folder.
- **Manual syncs**: if you need the Wiki updated before a change merges to `main`, use the “Run workflow” button in the Actions tab (workflow: “Sync docs to Wiki”).

Keeping all Wiki edits inside `docs/wiki/` ensures reviewers see doc updates in pull requests, and the automation takes care of copying them to the Wiki after merge.

---

Back to [[Home]]
