# Self Improvement

Guidelines for continuously improving Claude Code rules based on emerging patterns and best practices.

## When to Trigger

This rule activates when:
- AI makes the same mistake 2+ times
- A pattern emerges that should be standardized
- New tool/workflow gets used repeatedly
- Code review feedback indicates missing guidance
- User explicitly says "add a rule for this"

---

## Improvement Process

### 1. Recognize the Pattern

Ask:
- Is this mistake/pattern happening repeatedly?
- Would a rule have prevented this?
- Is there existing code that exemplifies the right approach?

### 2. Analyze the Gap

Determine if this needs:
- **New rule** - Pattern not covered anywhere
- **Rule update** - Existing rule incomplete
- **Rule merge** - Multiple rules covering same concern
- **Example addition** - Rule exists but needs better examples

### 3. Propose the Change

When you identify an improvement opportunity:

```markdown
## Rule Improvement Proposal

**Type:** New Rule / Update Existing / Add Example

**Trigger:** [What pattern/mistake prompted this]

**Current Behavior:** [What happens now]

**Proposed Rule/Change:**
[Draft of new content]

**Impact:** [What this prevents/enables]

**Confirm?** Should I create/update this rule?
```

### 4. Implement After Approval

Only create/update rules after user confirms.

---

## Rule Improvement Triggers

### Repeated Mistakes

| Mistake Pattern | Potential Rule |
|-----------------|----------------|
| Wrong file location | Update project structure rule |
| Incorrect import pattern | Add to stack overview |
| Missing error handling | Add error patterns to dev workflow |
| Wrong API usage | Update API patterns rule |
| Test anti-patterns | Update testing conventions |

### New Patterns

| New Pattern | Action |
|-------------|--------|
| New tool added | Create tool integration rule |
| New component pattern | Add to component guidelines |
| New API endpoint pattern | Update API conventions |
| New workflow step | Update workflow rule |

### Feedback Signals

- "You keep forgetting to..."
- "I've told you before that..."
- "This is wrong again..."
- "Why does this keep happening?"
- User manually corrects something repeatedly

---

## Rule Quality Criteria

Before proposing a rule, verify:

- [ ] **Actionable** - Contains specific, concrete guidance
- [ ] **Examples** - Includes DO and DON'T examples
- [ ] **Scoped** - Applies to specific situation, not too broad
- [ ] **Linked** - References related rules
- [ ] **Tested** - Based on actual codebase patterns
- [ ] **Current** - Reflects latest tools/versions

---

## Self-Improvement Areas

### Workflow Rules

Monitor for improvements in:
- morning-kickoff.md - Missing context types
- session-logging.md - Entry types needed
- end-of-day-reflection.md - Summary sections

### Development Rules

Monitor for improvements in:
- github-flow.md - New commands needed
- tdd-workflow.md - Testing patterns
- research-workflow.md - Investigation types

### Communication Rules

Monitor for improvements in:
- slack-cli-integration.md - New channels/patterns
- Channel IDs changing
- New communication workflows

---

## Common Improvement Examples

### Adding a Command

```markdown
## Improvement: Missing gh command for draft PRs

**Trigger:** User asked twice how to convert draft PR to ready

**Current:** github-flow.md doesn't cover draft -> ready conversion

**Proposed Addition:**
```bash
# Mark draft PR as ready for review
gh pr ready <PR_NUMBER> --repo <REPO>
```

**Location:** github-flow.md -> PR Workflow section
```

### Adding an Example

```markdown
## Improvement: Race condition diagram template

**Trigger:** Created race condition diagram 3 times manually

**Current:** mermaid-diagrams.md has sequence diagrams but no race condition pattern

**Proposed Addition:**
### Race Condition Investigation

```mermaid
sequenceDiagram
    [template]
```

**Location:** mermaid-diagrams.md -> Investigation Diagrams section
```

### Creating New Rule

```markdown
## Improvement: PR Review Checklist Rule

**Trigger:** Forgot to check tests twice when reviewing PRs

**Current:** No rule for PR review process

**Proposed New Rule:**
# PR Review Checklist
[Full rule content]

**Location:** Create new file pr-review-checklist.md
```

---

## Deprecation Process

When a rule becomes obsolete:

1. **Identify** - Pattern no longer used
2. **Verify** - Confirm with user before removing
3. **Update references** - Remove links from other rules
4. **Archive** - Move to deprecated or delete

```markdown
## Deprecation: old-pattern.md

**Reason:** Pattern replaced by new-pattern.md

**Action:**
- [ ] Update CLAUDE.md
- [ ] Update referencing rules
- [ ] Delete or mark deprecated

**Confirm?**
```

---

## Continuous Monitoring

During every session, watch for:

1. **Corrections** - User correcting AI output
2. **Repetition** - Same question asked multiple times
3. **Confusion** - Unclear which rule applies
4. **Gaps** - Situations with no applicable rule
5. **Friction** - Workflows that feel manual

Proactively suggest improvements when patterns emerge.
