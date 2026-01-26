# PR Review Analysis

Systematically analyze PR review comments and create actionable plans for addressing feedback. Use this workflow when you have PRs with reviewer comments that need attention.

## Trigger Phrases

- "review my PR comments"
- "analyze PR feedback"
- "PR action plan"
- "address review comments"
- "what do reviewers want"

---

## Data Collection

### Step 1: Gather Open PRs

```bash
# Platform PRs
gh pr list --author="@me" --state=open --repo flockx-official/platform \
  --json number,title,url,createdAt,reviewDecision --limit 10

# Clients PRs
gh pr list --author="@me" --state=open --repo flockx-official/community-web-app \
  --json number,title,url,createdAt,reviewDecision --limit 10
```

### Step 2: Get Review Comments (Inline)

```bash
# Get inline/file-level comments with location info
gh api repos/flockx-official/platform/pulls/{PR_NUMBER}/comments \
  --jq '.[] | {
    author: .user.login,
    createdAt: .created_at,
    body: .body,
    path: .path,
    line: .line,
    startLine: .start_line
  }'
```

### Step 3: Get Overall Reviews

```bash
# Get review-level comments (approval, changes requested, etc.)
gh pr view {PR_NUMBER} --repo flockx-official/platform \
  --json reviews --jq '.reviews[] | {
    author: .author.login,
    state: .state,
    body: .body,
    submittedAt: .submittedAt
  }'
```

---

## Comment Classification

### Comment Types

| Type | Description | Action Required |
|------|-------------|-----------------|
| **Bug** | Code defect identified | Fix required |
| **Architecture** | Design/structure concern | Discuss or refactor |
| **Style** | Code style/convention | Quick fix |
| **Question** | Clarification needed | Respond with explanation |
| **Suggestion** | Optional improvement | Consider implementing |
| **Blocking** | Must fix before merge | Critical path |

### Commenter Priority

| Source | Weight | Notes |
|--------|--------|-------|
| **Human Reviewer** | High | Devon, Kelsey, Jonathan - address first |
| **Cursor Bot** | Medium | Valid bugs, but verify before fixing |
| **Sentry Bot** | Medium | Often duplicates Cursor |
| **GitHub Actions** | Low | Usually automated checks |

---

## Output Format

### Comment Analysis Table

| Column | Description |
|--------|-------------|
| **PR** | PR number and repo |
| **Commenter** | Who left the comment |
| **Date/Time** | When commented |
| **Type** | Bug/Architecture/Style/Question/Suggestion |
| **File** | File path |
| **Line(s)** | Line number(s) |
| **Issue** | Brief description of concern |
| **Suggested Fix** | What needs to change |
| **Priority** | High / Medium / Low |

### Action Plan Template

```markdown
## PR Review Action Plan

**Generated:** {date}
**PRs Analyzed:** {list}

### Critical (Must Fix)

1. [PR#] Issue - File:Line - Fix description

### Important (Should Address)

1. [PR#] Issue - File:Line - Fix description

### Nice to Have (Consider)

1. [PR#] Issue - File:Line - Fix description

### Needs Discussion

1. [PR#] Issue - Who to discuss with - Question

### Already Addressed

1. [PR#] Issue - How it was resolved
```

---

## Analysis Workflow

### Phase 1: Collect

1. List all open PRs from last 48-72 hours
2. Fetch inline comments for each PR
3. Fetch review-level feedback
4. Filter out automated/noise comments

### Phase 2: Classify

For each comment:

1. Identify the commenter (human vs bot)
2. Determine comment type (bug, architecture, etc.)
3. Extract file and line information
4. Summarize the issue in one sentence
5. Propose a fix or action

### Phase 3: Prioritize

1. **Critical:** Blocking bugs, security issues, broken functionality
2. **Important:** Architectural concerns from senior reviewers
3. **Low:** Style nitpicks, suggestions, optional improvements

### Phase 4: Plan

1. Group related comments (same file, same concern)
2. Identify dependencies (fix A before B)
3. Estimate effort for each fix
4. Create ordered action list

---

## Common Patterns

### Duplicate Comments

Cursor Bot and Sentry Bot often flag the same issue. When you see duplicates:

- Count as single issue
- Note that multiple reviewers flagged it (increases priority)
- Use the most detailed description

### Architectural Feedback

When reviewers suggest refactoring:

1. Ask: Is this blocking the PR?
2. If blocking: Do the refactor
3. If not blocking: Propose follow-up card
4. Document decision in PR comment

### Bot False Positives

Common false positives to watch for:

- Cursor flagging intentional patterns as bugs
- Sentry flagging edge cases that are handled elsewhere
- Style suggestions that conflict with project conventions

When in doubt, add a comment explaining why the code is correct.

---

## Quick Commands

```bash
# All comments on a PR
gh api repos/{owner}/{repo}/pulls/{number}/comments

# Review states (approved, changes requested)
gh pr view {number} --repo {owner}/{repo} --json reviews

# Check if PR is mergeable
gh pr view {number} --repo {owner}/{repo} --json mergeable,mergeStateStatus

# List your PRs needing attention
gh pr list --author="@me" --state=open --json number,title,reviewDecision \
  | jq '.[] | select(.reviewDecision == "CHANGES_REQUESTED")'
```
