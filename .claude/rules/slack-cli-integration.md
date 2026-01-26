# Slack CLI Integration

Enable efficient Slack communication directly from the CLI using the Slack API. Integrates with GitHub workflow for notifications.

## Setup

The Slack user token is stored in `~/.zshrc` as `SLACK_CLI_TOKEN`. Before making API calls:

```bash
source ~/.zshrc
```

---

## Channel Quick Reference

| Alias | Channel | ID | Use For |
|-------|---------|-----|---------|
| `devs` | #fetch-devs | C08C70B48PJ | PR reviews, dev questions |
| `internal` | #fetchai-ao-internal | C08960MC0RG | AO internal discussions |
| `planning` | #ao-planning | C08CETC7YS2 | Client updates, Devon |
| `llm` | #fetch-llm | C08B2ME3YKW | AI/LLM discussions |
| `issues` | #llm_issues | C08V0EH4S7M | Bug tracking |

---

## Workflow-Triggered Messages

### PR Ready for Review

**When:** After `gh pr create` or when PR is ready
**Channel:** #fetch-devs (C08C70B48PJ)
**Tag:** Kelsey, Jonathan, Jared, Meg

```bash
source ~/.zshrc && curl -s -X POST "https://slack.com/api/chat.postMessage" \
  -H "Authorization: Bearer $SLACK_CLI_TOKEN" \
  -H "Content-type: application/json" \
  -d '{
    "channel": "C08C70B48PJ",
    "text": "PR ready for review:\n\n<https://github.com/flockx-official/REPO/pull/NUMBER|TITLE>\n\nSUMMARY\n\nTests passing\n\ncc <@U04CY6KMVC0> <@U022PDG1LHH> <@U072ZF8Q292> <@U03D8EQ2C95>"
  }'
```

**Template:**
```
PR ready for review:

<https://github.com/flockx-official/{repo}/pull/{number}|{type}: {title}>

{1-2 sentence summary}

Tests passing
Locally verified

cc <@U04CY6KMVC0> <@U022PDG1LHH> <@U072ZF8Q292> <@U03D8EQ2C95>
```

---

### Picked Up New Card

**When:** After starting work on a new card
**Channel:** #fetch-devs (C08C70B48PJ) - optional, for visibility
**Tag:** None usually

```
Picking up #{card_number}: {title}

Branch: `{branch_name}`

Will update when I have a PR ready.
```

---

### Blocked on Something

**When:** Can't proceed without help
**Channel:** #fetch-devs or DM to relevant expert
**Tag:** Person who can unblock

```
Blocked on #{card_number}:

**Issue:** {description of what's blocking}
**Tried:** {what you've attempted}
**Need:** {specific help needed}

@{person} when you have a moment?
```

---

### Card Complete / PR Merged

**When:** After PR merge, for client visibility
**Channel:** #ao-planning (C08CETC7YS2) - for significant features
**Tag:** Devon if relevant

```
Shipped: #{card_number} - {title}

{1-2 sentence description of what it does for users}

PR: <https://github.com/flockx-official/{repo}/pull/{number}|#{number}>
```

---

### Status Update

**When:** End of day, mid-sprint, or when asked
**Channel:** #ao-planning or #fetch-devs depending on audience

```
Status Update - {date}

**Completed:**
- #{card} - {description}
- #{card} - {description}

**In Progress:**
- #{card} - {status}

**Next:**
- #{card}

**Blockers:** {none or description}
```

---

### Bug Discovered

**When:** Found a bug during development
**Channel:** #llm_issues (C08V0EH4S7M)

```
Found issue: {short description}

**Where:** {page/feature}
**Steps:**
1. {step 1}
2. {step 2}

**Expected:** {what should happen}
**Actual:** {what happens}

{screenshot if applicable}
```

---

### Need Design/Product Input

**When:** Product decision needed
**Channel:** #ao-planning (C08CETC7YS2)
**Tag:** Devon

```
Need input on #{card_number}:

**Question:** {specific question}

**Options:**
A. {option 1} - {tradeoff}
B. {option 2} - {tradeoff}

**My recommendation:** {A or B} because {reason}

<@U04M3LQMWRJ> thoughts?
```

---

### Meeting Prep (Devon Bi-weekly)

**When:** Before Tuesday/Friday Devon syncs
**Channel:** #ao-planning (C08CETC7YS2)

```
Update for {date} sync:

**Shipped since last sync:**
- {feature/fix 1}
- {feature/fix 2}

**In flight:**
- {wip item 1} - ETA {date}
- {wip item 2} - ETA {date}

**Questions/decisions needed:**
- {question 1}

**Blockers:** {none or description}
```

---

## API Patterns

### Read Messages

```bash
# Last 20 messages from a channel
curl -s "https://slack.com/api/conversations.history?channel=CHANNEL_ID&limit=20" \
  -H "Authorization: Bearer $SLACK_CLI_TOKEN" | python3 -m json.tool

# Messages from today
curl -s "https://slack.com/api/conversations.history?channel=CHANNEL_ID&oldest=$(date -v0H -v0M -v0S +%s)&limit=50" \
  -H "Authorization: Bearer $SLACK_CLI_TOKEN" | python3 -m json.tool
```

### Post Message

```bash
curl -s -X POST "https://slack.com/api/chat.postMessage" \
  -H "Authorization: Bearer $SLACK_CLI_TOKEN" \
  -H "Content-type: application/json" \
  -d '{"channel":"CHANNEL_ID","text":"Your message here"}'
```

### Reply to Thread

```bash
curl -s -X POST "https://slack.com/api/chat.postMessage" \
  -H "Authorization: Bearer $SLACK_CLI_TOKEN" \
  -H "Content-type: application/json" \
  -d '{"channel":"CHANNEL_ID","text":"Your reply","thread_ts":"PARENT_MESSAGE_TS"}'
```

### Add Reaction

```bash
curl -s -X POST "https://slack.com/api/reactions.add" \
  -H "Authorization: Bearer $SLACK_CLI_TOKEN" \
  -H "Content-type: application/json" \
  -d '{"channel":"CHANNEL_ID","timestamp":"MESSAGE_TS","name":"eyes"}'
```

### Get User Info

```bash
curl -s "https://slack.com/api/users.info?user=USER_ID" \
  -H "Authorization: Bearer $SLACK_CLI_TOKEN" | python3 -m json.tool
```

---

## Team @mentions

```
<@U04M3LQMWRJ>  -> Devon Bleibtrey (CTO) - NOT in #fetch-devs
<@U04CY6KMVC0>  -> Kelsey Brennan
<@U022PDG1LHH>  -> Jonathan Chaffer
<@U072ZF8Q292>  -> Jared Currie
<@U0367TNU1KK>  -> Nick Hawn
<@U03D8EQ2C95>  -> Meghan Harris
<@U024BKEMWAU>  -> Ty Swanson
```

**PR Reviewer Tags (copy-paste):**
```
cc <@U04CY6KMVC0> <@U022PDG1LHH> <@U072ZF8Q292> <@U03D8EQ2C95>
```

---

## Link Formats

**Clickable URL:**
```
<https://github.com/flockx-official/platform/pull/123|feat: Add feature>
```

**Message link:**
```
https://atomicobject.slack.com/archives/{CHANNEL_ID}/p{TS_NO_DOT}
```
(Remove `.` from timestamp: `1764861304.749249` -> `1764861304749249`)

---

## Common Reactions

| Emoji | Name | Meaning |
|-------|------|---------|
| Eyes | `eyes` | Looking into it |
| Thumbs up | `thumbsup` | Acknowledged |
| Check | `white_check_mark` | Done/approved |
| Fire | `fire` | Great work |
| Hourglass | `hourglass` | In progress |

---

## Workflow Guidelines

### Before Posting

1. **Show exact message** that will be sent
2. **Confirm channel** is correct
3. **Wait for approval** ("send it", "post it", "yes")
4. **Show response** after posting

### Drafting Responses

1. Match Nathan's style: casual, direct, collaborative
2. Present draft for review before posting
3. **Never auto-post** - always confirm

---

## Security

- **Never** echo or log the actual token
- **Always** confirm before posting
- Token grants access to Nathan's Slack identity
