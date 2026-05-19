# Fork-First Maintenance Flow

This fork is the active user-owned maintenance lane.

## Remotes

- `origin`: `https://github.com/vecyang1/Infinite-Canvas.git`
- `upstream`: `https://github.com/hero8152/Infinite-Canvas.git`

Use `origin/main` as the normal base for local work. Treat `upstream/main` as an external source to inspect or cherry-pick from, not as the default release target.

## Normal Work

```bash
git fetch origin --prune
git checkout main
git pull --ff-only origin main
git checkout -b codex/<short-description>
# edit, test
git push -u origin codex/<short-description>
```

For local launcher machines with the A-Sync lane installed:

```bash
mp2 repo
mp2 update
mp2 push
```

## Upstream Comparison

When the original repository changes, inspect it explicitly:

```bash
git fetch upstream --prune
git log --oneline --left-right origin/main...upstream/main
git diff --stat origin/main..upstream/main
```

Cherry-pick or manually port useful upstream commits only after checking that they do not remove user-owned fork features such as provider setup, API image generation, sidebar pinning, and fork docs.
