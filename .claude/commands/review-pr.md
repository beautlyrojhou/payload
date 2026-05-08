# Review Pull Request

Review a pull request for the Payload CMS fork, providing thorough technical feedback.

## Usage

```
/review-pr <PR_NUMBER_OR_URL>
```

## Instructions

You are reviewing a pull request for the Payload CMS fork. Follow these steps:

### 1. Fetch PR Details

Use the GitHub CLI to get PR information:

```bash
gh pr view $PR_NUMBER --json title,body,author,baseRefName,headRefName,files,commits,labels
gh pr diff $PR_NUMBER
```

### 2. Understand the Context

- Read the PR title and description carefully
- Check if there's a linked issue (look for "Fixes #", "Closes #", "Resolves #")
- If linked, fetch the issue: `gh issue view <ISSUE_NUMBER>`
- Review what files were changed and why

### 3. Code Review Checklist

Evaluate the PR against these criteria:

#### Correctness
- Does the code actually solve the stated problem?
- Are there edge cases that aren't handled?
- Could this introduce regressions in other areas?

#### TypeScript Quality
- Are types properly defined (no unnecessary `any`)?
- Are generics used appropriately?
- Do exported types maintain backward compatibility?

#### Payload-Specific Concerns
- Does it follow existing Payload patterns and conventions?
- Are collection/global hooks handled correctly?
- Is the admin UI impacted? If so, is it consistent with existing UI?
- Are database adapters (MongoDB/Postgres) both considered if relevant?
- Are translations needed for any new UI strings? Check `.claude/skills/generate-translations/SKILL.md`

#### Testing
- Are there tests for the new functionality or bug fix?
- Do existing tests still pass conceptually?
- Are integration tests needed for complex features?

#### Documentation
- Is the code self-documenting with clear variable/function names?
- Are complex algorithms commented?
- Does the PR description explain the "why" not just the "what"?

#### Security
- Are user inputs validated/sanitized?
- Are access control checks in place where needed?
- No secrets or sensitive data committed?

#### Performance
- Are there any obvious N+1 query issues?
- Are expensive operations cached where appropriate?
- Are large dependencies added unnecessarily?

### 4. Determine Review Outcome

Choose one of:
- **APPROVE**: Code is correct, well-tested, and follows conventions
- **REQUEST_CHANGES**: Issues must be addressed before merging
- **COMMENT**: Feedback provided but not blocking

### 5. Write Review

Structure your review as:

```
## Summary
<1-2 sentence overview of what the PR does and your overall impression>

## Strengths
- <What was done well>

## Issues
### Critical (must fix)
- <File:Line> — <Issue description and suggested fix>

### Minor (should fix)
- <File:Line> — <Issue description>

### Suggestions (optional)
- <File:Line> — <Enhancement idea>

## Verdict: <APPROVE | REQUEST_CHANGES | COMMENT>
```

### 6. Post Review (if requested)

If the user wants to post the review:

```bash
# For approval
gh pr review $PR_NUMBER --approve --body "<review body>"

# For requesting changes
gh pr review $PR_NUMBER --request-changes --body "<review body>"

# For comment only
gh pr review $PR_NUMBER --comment --body "<review body>"
```

## Notes

- Be constructive and specific — reference exact files and line numbers
- Distinguish between blocking issues and suggestions
- Acknowledge good work when you see it
- Consider the PR author's experience level when calibrating feedback tone
- Check if this is a first-time contributor and be especially welcoming
