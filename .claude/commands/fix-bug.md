# Fix Bug

Analyze and fix a bug in the payload codebase based on the provided issue or description.

## Usage

```
/fix-bug <issue-url-or-description>
```

## Instructions

You are an expert TypeScript developer working on the Payload CMS codebase. Your task is to investigate and fix a reported bug.

### Step 1: Understand the Bug

1. If given a GitHub issue URL, fetch the issue details using the GitHub API
2. Extract:
   - Bug description and expected vs actual behavior
   - Steps to reproduce
   - Affected versions
   - Any error messages or stack traces
   - Labels and priority

### Step 2: Investigate the Codebase

1. Identify the relevant files and modules affected
2. Search for related code using:
   - File paths mentioned in stack traces
   - Function/class names referenced in the issue
   - Related test files that might demonstrate the expected behavior
3. Understand the data flow and how the bug manifests
4. Check git history for recent changes that may have introduced the regression

### Step 3: Reproduce the Bug

1. Look for existing tests that cover the affected functionality
2. Identify what test would fail given the reported bug
3. If no test exists, note that one should be added

### Step 4: Implement the Fix

1. Make the minimal change necessary to fix the bug
2. Avoid refactoring unrelated code in the same PR
3. Ensure the fix handles edge cases mentioned in the issue
4. Follow existing code patterns and conventions in the file
5. Maintain TypeScript strict mode compliance

### Step 5: Add or Update Tests

1. Add a regression test that would have caught this bug
2. Place tests in the appropriate test file following existing patterns
3. Ensure the test name clearly describes what it's testing
4. Run the relevant test suite mentally to verify correctness

### Step 6: Verify the Fix

1. Review the changes for any unintended side effects
2. Check that the fix doesn't break related functionality
3. Verify TypeScript types are correct
4. Ensure no `any` types were introduced unnecessarily

### Step 7: Summarize

Provide a clear summary including:
- **Root Cause**: What caused the bug
- **Fix**: What was changed and why
- **Files Modified**: List of changed files
- **Tests Added/Modified**: What test coverage was added
- **Breaking Changes**: Whether this fix has any breaking implications
- **Suggested PR Title**: A conventional commit style title (e.g., `fix(collections): resolve duplicate key error on nested array fields`)

## Code Style Guidelines

- Use TypeScript with strict types
- Follow existing naming conventions in the file
- Prefer `const` over `let` where possible
- Use early returns to reduce nesting
- Add JSDoc comments for any new exported functions
- Keep changes focused and minimal
