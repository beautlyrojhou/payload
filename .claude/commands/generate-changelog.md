# Generate Changelog

Generate a structured changelog entry for recent changes in the payload codebase.

## Usage

```
/generate-changelog [from-tag] [to-tag]
```

## Arguments

- `from-tag` (optional): Starting git tag or commit SHA. Defaults to the last release tag.
- `to-tag` (optional): Ending git tag or commit SHA. Defaults to `HEAD`.

## Instructions

You are generating a changelog for the **payload** CMS project. Follow these steps carefully:

### 1. Gather Commits

Run the following to get commits between the two refs:

```bash
git log $FROM_TAG..$TO_TAG --pretty=format:"%H %s" --no-merges
```

If no tags are provided, find the latest tag first:

```bash
git describe --tags --abbrev=0
```

### 2. Categorize Changes

Group commits into the following categories based on conventional commit prefixes and content:

- **🚀 Features** — `feat:` commits
- **🐛 Bug Fixes** — `fix:` commits
- **⚡ Performance** — `perf:` commits
- **♻️ Refactors** — `refactor:` commits
- **📦 Dependencies** — `chore(deps):` or dependency-related commits
- **📖 Documentation** — `docs:` commits
- **🧪 Tests** — `test:` commits
- **🔧 Chores** — remaining `chore:` commits
- **💥 Breaking Changes** — any commit with `BREAKING CHANGE` in the body or `!` after the type

### 3. Format the Changelog

Output the changelog in the following markdown format:

```markdown
## [VERSION] - YYYY-MM-DD

### 💥 Breaking Changes
- Description of breaking change (#PR or commit)

### 🚀 Features
- Short description of feature (#PR or commit)

### 🐛 Bug Fixes
- Short description of fix (#PR or commit)

### ⚡ Performance
- Short description of improvement (#PR or commit)

### ♻️ Refactors
- Short description of refactor (#PR or commit)

### 📦 Dependencies
- Bumped `package-name` from `x.x.x` to `y.y.y`

### 📖 Documentation
- Short description of doc change (#PR or commit)

### 🧪 Tests
- Short description of test change (#PR or commit)

### 🔧 Chores
- Short description of chore (#PR or commit)
```

Omit sections that have no entries.

### 4. Enrich with PR References

For each commit, attempt to find the associated PR number:

```bash
git log --pretty=format:"%H %s" | grep -oP '\(#\d+\)'
```

If a PR number is found in the commit message (e.g., `(#1234)`), include it as a link:
`[#1234](https://github.com/payloadcms/payload/pull/1234)`

### 5. Highlight Notable Changes

For any of the following, add a `> **Note:**` callout beneath the entry:
- New collection or field types added
- Changes to the admin UI
- Database adapter changes
- Authentication or access control changes
- Plugin API changes

### 6. Output Location

Ask the user if they want to:
1. Print the changelog to stdout
2. Prepend it to `CHANGELOG.md`
3. Create a new file `CHANGELOG-[version].md`

Default to option **2** if no preference is given.

### 7. Version Detection

If a version number is not explicitly provided:
- Read `packages/payload/package.json` to get the current version
- Use that as the changelog version header

```bash
cat packages/payload/package.json | jq -r '.version'
```

## Example Output

```markdown
## [3.12.0] - 2024-11-15

### 🚀 Features
- Add support for `hasMany` relationship fields in list view filters [#7821](https://github.com/payloadcms/payload/pull/7821)
- Introduce `beforeDuplicate` collection hook [#7834](https://github.com/payloadcms/payload/pull/7834)

### 🐛 Bug Fixes
- Fix locale fallback not applying in nested blocks [#7809](https://github.com/payloadcms/payload/pull/7809)
- Resolve admin UI crash when `defaultValue` is undefined on array fields [#7815](https://github.com/payloadcms/payload/pull/7815)

### 📦 Dependencies
- Bumped `drizzle-orm` from `0.33.0` to `0.34.1`
```
