# Sync Fork with Upstream

Keep this fork of payloadcms/payload up to date with upstream changes.

## Usage

```
/sync-fork [--branch <branch>] [--strategy <merge|rebase>] [--dry-run]
```

## Arguments

- `--branch`: Target branch to sync (default: current branch)
- `--strategy`: Sync strategy — `merge` or `rebase` (default: `merge`)
- `--dry-run`: Show what would happen without making changes

## Steps

### 1. Verify Upstream Remote

Check if the upstream remote is configured:

```bash
git remote -v
```

If `upstream` is not listed, add it:

```bash
git remote add upstream https://github.com/payloadcms/payload.git
```

### 2. Fetch Upstream Changes

```bash
git fetch upstream --tags
```

Note any new tags or branches that have appeared since the last sync.

### 3. Identify Divergence

Before syncing, report the current state:

```bash
git log --oneline upstream/main..HEAD
```

This shows commits in our fork that are NOT in upstream — these are our custom changes that must be preserved.

Also check:
```bash
git log --oneline HEAD..upstream/main
```

This shows upstream commits we haven't pulled in yet.

### 4. Check for Conflicts

Identify files that our fork has modified compared to upstream:

```bash
git diff --name-only upstream/main...HEAD
```

Pay special attention to:
- `packages/*/src/**` — core source files
- `package.json` and `pnpm-lock.yaml` — dependency changes
- `.github/workflows/**` — CI/CD pipeline files
- `test/**` — test suite changes

### 5. Perform Sync

**If strategy is `merge`:**
```bash
git merge upstream/main --no-ff -m "chore: sync with upstream payloadcms/payload"
```

**If strategy is `rebase`:**
```bash
git rebase upstream/main
```

Rebase is preferred when our fork has a small number of clean commits on top of upstream. Use merge when the history is complex or there are many fork-specific commits.

### 6. Resolve Conflicts

If conflicts arise, list them clearly:

```bash
git diff --name-only --diff-filter=U
```

For each conflicted file:
1. Explain what upstream changed
2. Explain what our fork changed
3. Recommend the appropriate resolution strategy
4. Apply the resolution

Common conflict areas in this fork:
- Custom plugin integrations in `packages/payload/src/`
- Fork-specific configuration in `payload.config.ts` examples
- Additional translations in `packages/translations/`

### 7. Update Dependencies

After syncing, check if `package.json` files changed upstream:

```bash
git diff HEAD~1 -- '**/package.json' | grep '\+\s*"version"'
```

If dependency versions changed, run:
```bash
pnpm install
```

### 8. Run Tests

Verify the sync didn't break anything:

```bash
pnpm build
pnpm test --passWithNoTests
```

If tests fail, identify whether the failure is due to:
- A conflict resolution issue (fix in our fork)
- An upstream regression (open an issue upstream)
- A fork-specific test that needs updating

### 9. Generate Sync Summary

Produce a summary report:

```
## Fork Sync Summary

**Date:** <date>
**Upstream commits pulled:** <count>
**Fork-specific commits preserved:** <count>
**Conflicts resolved:** <count>
**New upstream version:** <version if applicable>

### Notable Upstream Changes
<list key changes from upstream commit messages>

### Preserved Fork Changes
<list our custom commits that were rebased/merged>

### Action Items
<any follow-up tasks required>
```

### 10. Push (if not dry-run)

```bash
git push origin <branch>
```

If rebasing, a force push may be required:
```bash
git push origin <branch> --force-with-lease
```

> ⚠️ Never force-push to `main` without team review.
