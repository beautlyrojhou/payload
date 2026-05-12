# Write Tests

Generate comprehensive tests for a given file, feature, or bug fix in the Payload CMS codebase.

## Usage

```
/write-tests <target> [--type unit|integration|e2e] [--coverage]
```

## Arguments

- `target` - File path, feature name, or description of what to test
- `--type` - Type of tests to generate (default: auto-detect based on target)
- `--coverage` - Analyze existing coverage and fill gaps

## Instructions

### 1. Analyze the Target

First, understand what needs to be tested:

```bash
# If target is a file path, read it
cat <target>

# Check for existing tests
find . -name "*.test.ts" -o -name "*.spec.ts" | xargs grep -l "<target_name>" 2>/dev/null

# Check test configuration
cat jest.config.js 2>/dev/null || cat vitest.config.ts 2>/dev/null
```

### 2. Identify Test Type

Based on the target, determine the appropriate test type:

- **Unit tests**: Pure functions, utilities, hooks, individual components
- **Integration tests**: API endpoints, database operations, plugin interactions
- **E2E tests**: Full user workflows, admin UI interactions

Payload test locations:
- Unit: `packages/<package>/src/__tests__/`
- Integration: `test/<feature>/`
- E2E: `test/e2e/`

### 3. Review Existing Test Patterns

```bash
# Find similar test files for patterns
ls test/
ls packages/*/src/__tests__/ 2>/dev/null

# Review a similar test for conventions
cat test/<similar-feature>/int.spec.ts 2>/dev/null
```

### 4. Generate Tests

Follow these Payload-specific testing conventions:

#### Unit Test Template
```typescript
import { <functionName> } from '../<path>'

describe('<ModuleName>', () => {
  describe('<methodName>', () => {
    it('should <expected behavior>', () => {
      // Arrange
      const input = ...
      
      // Act
      const result = <functionName>(input)
      
      // Assert
      expect(result).toEqual(...)
    })

    it('should handle edge case: <case>', () => {
      // ...
    })

    it('should throw when <invalid condition>', () => {
      expect(() => <functionName>(invalidInput)).toThrow('<error message>')
    })
  })
})
```

#### Integration Test Template
```typescript
import type { Payload } from 'payload'
import { getPayload } from 'payload'
import { config } from './config'

describe('<Feature> Integration', () => {
  let payload: Payload

  beforeAll(async () => {
    payload = await getPayload({ config })
  })

  afterAll(async () => {
    // cleanup
  })

  it('should <expected behavior>', async () => {
    const result = await payload.<operation>({
      collection: '<collection-slug>',
      // ...
    })
    expect(result).toBeDefined()
  })
})
```

### 5. Coverage Analysis

If `--coverage` flag is provided:

```bash
# Run existing tests with coverage
pnpm test --coverage --testPathPattern="<target>"

# Identify uncovered lines and branches
# Generate tests specifically for uncovered code paths
```

### 6. Validate Generated Tests

```bash
# Run the newly generated tests
pnpm test <test-file-path>

# Ensure no existing tests are broken
pnpm test --testPathPattern="<related-area>"
```

### 7. Output Summary

Provide a summary including:
- Number of test cases generated
- Test file location(s)
- Coverage improvement (if applicable)
- Any edge cases that should be manually reviewed
- Commands to run the tests

## Notes

- Always use `describe`/`it` blocks with descriptive names following the "should" convention
- Mock external dependencies (database, HTTP calls) in unit tests
- Use Payload's test helpers from `@payloadcms/test-helpers` when available
- Prefer `beforeAll`/`afterAll` for expensive setup in integration tests
- Follow existing naming conventions: `*.spec.ts` for unit, `int.spec.ts` for integration
- Check `pnpm-workspace.yaml` to understand the monorepo structure before placing test files
