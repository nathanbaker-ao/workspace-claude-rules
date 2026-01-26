# Team Directory

Quick reference for team members, GitHub usernames, roles, and **when to contact who**.

---

## Atomic Object Team

**Consultants** working on the Fetch AI / ASI:One project.

| Name | GitHub | Slack ID | Role | Contact For |
|------|--------|----------|------|-------------|
| **Nathan Baker** | `nathanbaker-ao` | U09LX74SGJE | Developer | (that's you) |
| **Ty Swanson** | `thswanson97` | U024BKEMWAU | Developer | AO internal, pairing |
| **Drew Colthorp** | `dcolthorp` | U024G4PES | Director | Escalations, architecture decisions |
| **Tammy Pearson** | `tammypearson` | UEJBBCEPK | Delivery Lead | Scheduling, client relations, blockers |
| **Nick Hawn** | `nickhawn` | U0367TNU1KK | Senior Developer | Workflows, meta connectors, recurring tasks, backend |
| **Kelsey Brennan** | `kelseybrennan12` | U04CY6KMVC0 | Developer | Planner mode, chat v2, PR reviews |
| **Jared Currie** | `jccurrie1` | U072ZF8Q292 | Developer | Frontend, participant management, PR reviews |
| **Jonathan Chaffer** | `jonathanchaffer` | U022PDG1LHH | Developer | PR reviews, general dev questions |
| **Meghan Harris** | `harrmegh` | U03D8EQ2C95 | Developer | Frontend, PR reviews |

### AO Quick DMs

| Person | DM Channel | Use For |
|--------|------------|---------|
| Nick Hawn | D09S68T9ZFY | Recurring tasks, workflows, technical deep-dives |
| Ty Swanson | D09MELP5SDC | AO internal, cursor rules, pairing |

---

## FlockX Team (Client)

**Fetch AI / FlockX** client-side team members.

| Name | GitHub | Slack ID | Role | Contact For |
|------|--------|----------|------|-------------|
| **Devon Bleibtrey** | `bleib1dj` | U04M3LQMWRJ | CTO | Product decisions, priorities, blockers, client updates |
| **Nick Hazekamp** | `nhazekam` | UNXP6CT6K | Developer | PR reviews, mobile development |
| **Hemant Sharma** | `sh-wallet` | U050TLF65NU | Developer | Mobile app, PR reviews (tag for awareness) |

---

## Communication Routing

### By Situation -> Channel -> Who

| Situation | Channel | Who to Tag | Notes |
|-----------|---------|------------|-------|
| **PR ready for review** | #fetch-devs | Kelsey, Jonathan, Jared, Meg | Add Devon on GitHub only (not in channel) |
| **Blocked on something** | #fetch-devs or DM | Relevant expert | DM for quick questions |
| **Client status update** | #ao-planning | Devon (if needed) | Weekly/milestone updates |
| **Internal AO discussion** | #fetchai-ao-internal | AO team | Strategy, process, internal concerns |
| **Bug discovered** | #llm_issues | - | Document for triage |
| **Design/product question** | #ao-planning | Devon | Product direction questions |
| **Meeting prep (Devon)** | #ao-planning | - | Bi-weekly sync updates |
| **Technical architecture** | DM or #fetch-devs | Nick, Drew | Deep technical discussions |
| **Frontend question** | #fetch-devs or DM | Jared, Kelsey, Meg | Component patterns, state management |
| **Backend question** | #fetch-devs or DM | Nick, Jonathan | API, Django, FastAPI |

### By Topic -> Expert

| Topic | Primary Contact | Backup | Channel |
|-------|-----------------|--------|---------|
| Workflows / Recurring Tasks | Nick Hawn | - | DM or #fetch-devs |
| Meta Connectors | Nick Hawn | - | DM or #fetch-devs |
| Planner Mode | Kelsey Brennan | - | #fetch-devs |
| Chat v2 | Kelsey Brennan | Jonathan | #fetch-devs |
| Frontend Components | Jared Currie | Meg | #fetch-devs |
| Product/Priorities | Devon Bleibtrey | Tammy | #ao-planning |
| Client Relations | Tammy Pearson | Drew | #fetchai-ao-internal |
| Architecture Decisions | Drew Colthorp | Nick | DM or escalate |

---

## Key Slack Channels

| Channel | ID | Purpose | When to Use |
|---------|-----|---------|-------------|
| #fetchai-ao-internal | C08960MC0RG | AO internal | Strategy, process, internal concerns |
| #fetch-devs | C08C70B48PJ | Dev team | PR reviews, dev questions, status updates |
| #ao-planning | C08CETC7YS2 | Client planning | Devon meetings, client updates, priorities |
| #fetch-llm | C08B2ME3YKW | LLM/AI | AI-specific discussions |
| #llm_issues | C08V0EH4S7M | Issue tracking | Bug reports, issue triage |
| #flockx-stream | C099TFWRF63 | FlockX updates | General FlockX news |
| #flockx-issues | C09ANTYRCEL | FlockX issues | FlockX-specific bugs |
| #asi-one-mobile-app | C0999KNTC7M | Slack Connect | Mobile PRs, @Hemant for awareness |

---

## Slack @mention Format

```
<@U04M3LQMWRJ>  -> Devon Bleibtrey (CTO)
<@UNXP6CT6K>    -> Nick Hazekamp
<@U050TLF65NU>  -> Hemant Sharma (mobile)
<@U04CY6KMVC0>  -> Kelsey Brennan
<@U022PDG1LHH>  -> Jonathan Chaffer
<@U072ZF8Q292>  -> Jared Currie
<@U0367TNU1KK>  -> Nick Hawn
<@U03D8EQ2C95>  -> Meghan Harris
<@U024BKEMWAU>  -> Ty Swanson
```

---

## GitHub Reviewers

Add all reviewers to PRs:

```bash
gh pr edit <PR_NUMBER> --repo <REPO> --add-reviewer kelseybrennan12,jonathanchaffer,jccurrie1,harrmegh,bleib1dj,nhazekam
```

**For Mobile PRs:** Also add Hemant and post to #asi-one-mobile-app:
```bash
gh pr edit <PR_NUMBER> --repo <REPO> --add-reviewer nhazekam,sh-wallet
```

**Note:** Devon (`bleib1dj`) is NOT in #fetch-devs - add him on GitHub only, communicate via #ao-planning.

---

## Quick Reference Commands

```bash
# Check if user exists on GitHub
gh api users/USERNAME --jq '.login + " - " + .name'

# List org members
gh api orgs/atomicobject/members --jq '.[].login' | sort

# Get Slack user info
source ~/.zshrc && curl -s "https://slack.com/api/users.info?user=SLACK_ID" \
  -H "Authorization: Bearer $SLACK_CLI_TOKEN" | python3 -m json.tool
```
