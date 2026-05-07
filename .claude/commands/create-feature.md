# Create Feature Command

Implement a new feature in the Payload CMS fork based on a GitHub issue or feature description.

## Usage

```
/create-feature <issue-url-or-description>
```

## Instructions

You are a senior TypeScript developer working on a fork of payloadcms/payload. Your task is to implement a new feature following Payload's conventions and architecture.

### Step 1: Understand the Feature

If given a GitHub issue URL:
- Fetch and analyze the issue description
- Review any linked discussions or PRs
- Identify acceptance criteria and edge cases

If given a description:
- Parse the requirements carefully
- Ask clarifying questions if the scope is ambiguous

### Step 2: Explore the Codebase

Before writing any code:
1. Identify which packages are affected (e.g., `packages/payload`, `packages/next`, `packages/db-postgres`)
2. Find existing similar features to follow established patterns
3. Check for relevant types in `packages/payload/src/types`
4. Review existing tests for the area you're modifying

```bash
# Find relevant files
find packages -type f -name '*.ts' | xargs grep -l '<relevant-keyword>' | head -20

# Check package structure
ls packages/<package-name>/src
```

### Step 3: Plan the Implementation

Outline:
- Files to create
- Files to modify
- New types/interfaces needed
- Breaking changes (if any)
- Migration requirements (for DB changes)

### Step 4: Implement

Follow these conventions:

**TypeScript**
- Use strict typing, avoid `any`
- Export types from appropriate index files
- Use existing utility types from `payload/types`

**File Structure**
```
packages/<package>/src/
  <feature>/
    index.ts        # Main export
    types.ts        # Type definitions
    <feature>.ts    # Core implementation
```

**Code Style**
- Follow existing ESLint configuration
- Use named exports over default exports
- Document public APIs with JSDoc comments
- Handle errors gracefully with descriptive messages

**Collections & Fields**
- Respect existing hook patterns (`beforeChange`, `afterRead`, etc.)
- Maintain backwards compatibility where possible
- Follow the access control patterns

### Step 5: Write Tests

```bash
# Run existing tests to ensure nothing is broken
pnpm test --filter=<package>

# Test file location convention
packages/<package>/src/<feature>/<feature>.spec.ts
```

Test requirements:
- Unit tests for utility functions
- Integration tests for API endpoints
- E2E tests for UI features (in `test/` directory)

### Step 6: Update Documentation

- Add JSDoc to all public functions and types
- Update relevant `README.md` if the feature is user-facing
- Add changelog entry if applicable

### Step 7: Verify

```bash
# Type check
pnpm tsc --noEmit

# Lint
pnpm lint

# Build affected packages
pnpm build --filter=<package>

# Run tests
pnpm test --filter=<package>
```

## Output

Provide:
1. Summary of changes made
2. List of files created/modified
3. Any breaking changes or migration notes
4. Suggested PR description
