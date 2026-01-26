# Mermaid Diagrams

Standard patterns and templates for mermaid diagrams used throughout daily notes, research, and documentation.

## When to Use Diagrams

**Always diagram:**

- Complex data flows
- Multi-step processes
- Architecture decisions
- Bug investigation flows
- Feature implementation phases
- State transitions
- API sequences

**Diagram triggers:**

- "Show me the flow"
- "Visualize this"
- "How does this work"
- "Diagram this architecture"

---

## Flowchart Patterns

### Basic Flow

```mermaid
flowchart TD
    A[Start] --> B[Process]
    B --> C{Decision?}
    C -->|Yes| D[Path A]
    C -->|No| E[Path B]
    D --> F[End]
    E --> F
```

### Feature Implementation Flow

```mermaid
flowchart LR
    subgraph Phase1[Phase 1: Backend]
        A[DB Schema] --> B[API Endpoint]
    end

    subgraph Phase2[Phase 2: Frontend]
        C[Component] --> D[Hook]
        D --> E[Context]
    end

    subgraph Phase3[Phase 3: Tests]
        F[Unit Tests] --> G[E2E Tests]
    end

    Phase1 --> Phase2
    Phase2 --> Phase3
```

### Component Data Flow

```mermaid
flowchart TD
    subgraph Frontend
        UI[Component] --> Hook[Custom Hook]
        Hook --> Context[Context Provider]
    end

    subgraph API
        Route[API Route] --> Handler[Handler]
    end

    subgraph Backend
        Service[Service Layer]
        Model[Django Model]
    end

    Context -->|fetch| Route
    Handler --> Service
    Service --> Model
    Model -->|response| Service
    Service -->|data| Handler
    Handler -->|JSON| Route
    Route -->|state| Context
```

---

## Sequence Diagrams

### API Request Flow

```mermaid
sequenceDiagram
    participant U as User
    participant C as Component
    participant H as Hook
    participant A as API Route
    participant B as Backend

    U->>C: Click action
    C->>H: triggerAction()
    H->>A: POST /api/action
    A->>B: Forward request
    B-->>A: Response
    A-->>H: JSON response
    H-->>C: Update state
    C-->>U: Show result
```

### WebSocket Flow

```mermaid
sequenceDiagram
    participant Client
    participant WSHandler
    participant EventBus
    participant Service

    Client->>WSHandler: Connect
    WSHandler->>EventBus: Subscribe

    loop Message Stream
        Service->>EventBus: Publish event
        EventBus->>WSHandler: Notify
        WSHandler->>Client: Send message
    end

    Client->>WSHandler: Disconnect
    WSHandler->>EventBus: Unsubscribe
```

---

## State Diagrams

### PR Lifecycle

```mermaid
stateDiagram-v2
    [*] --> Draft
    Draft --> Open: Ready for review
    Open --> ChangesRequested: Reviewer requests changes
    ChangesRequested --> Open: Changes addressed
    Open --> Approved: Approved by reviewer
    Approved --> Merged: Merge PR
    Merged --> [*]

    Open --> Closed: Closed without merge
    ChangesRequested --> Closed: Abandoned
    Closed --> [*]
```

### Card Workflow

```mermaid
stateDiagram-v2
    [*] --> Backlog
    Backlog --> Todo: Prioritized
    Todo --> InProgress: Started
    InProgress --> InReview: PR created
    InReview --> InProgress: Changes requested
    InReview --> Done: Merged
    Done --> [*]

    InProgress --> Blocked: Blocker hit
    Blocked --> InProgress: Unblocked
```

---

## Architecture Diagrams

### System Overview

```mermaid
flowchart TB
    subgraph Clients
        Web[fetch-llm<br/>Next.js]
        Native[Native Apps<br/>React Native]
    end

    subgraph Platform
        API[FastAPI]
        Django[Django Admin]
        Celery[Celery Workers]
    end

    subgraph Data
        PG[(PostgreSQL)]
        Redis[(Redis)]
        ES[(Elasticsearch)]
        Neo4j[(Neo4j)]
    end

    subgraph AI
        LangGraph[LangGraph Agents]
        OpenAI[OpenAI API]
    end

    Web --> API
    Native --> API
    API --> Django
    API --> Celery
    Django --> PG
    Celery --> Redis
    API --> ES
    API --> Neo4j
    API --> LangGraph
    LangGraph --> OpenAI
```

---

## Chart Diagrams

### Progress Visualization

```mermaid
pie title Sprint Progress
    "Completed" : 60
    "In Progress" : 25
    "Blocked" : 10
    "Todo" : 5
```

---

## Git Diagrams

### Branch Strategy

```mermaid
gitGraph
    commit id: "main"
    branch staging
    checkout staging
    commit id: "staging"
    branch feature/1234
    checkout feature/1234
    commit id: "feat: start"
    commit id: "feat: progress"
    checkout staging
    merge feature/1234
    checkout main
    merge staging tag: "v1.2.0"
```

---

## Investigation Diagrams

### Bug Root Cause

```mermaid
flowchart TD
    Bug[Bug: Loader stuck] --> Q1{Reproducible?}
    Q1 -->|Yes| Q2{When does it happen?}
    Q1 -->|No| More[Need more info]

    Q2 -->|Multi-user no @mention| Trace[Trace code path]

    Trace --> Finding1[setLoading true on submit]
    Finding1 --> Finding2[isAIResponding stays false]
    Finding2 --> Finding3[useEffect deps unchanged]
    Finding3 --> RootCause[Root Cause: No trigger<br/>to clear loading state]

    RootCause --> Fix[Fix: Check mention<br/>before showing loader]
```

---

## Template Quick Reference

### When to use which:

| Scenario | Diagram Type |
|----------|--------------|
| Multi-step process | `flowchart TD/LR` |
| API interactions | `sequenceDiagram` |
| State transitions | `stateDiagram-v2` |
| System architecture | `flowchart TB` with subgraphs |
| Progress tracking | `pie` |
| Git workflow | `gitGraph` |

### Direction shortcuts:

- `TD` / `TB` - Top to bottom (vertical)
- `LR` - Left to right (horizontal)
- `RL` - Right to left
- `BT` - Bottom to top

### Node shapes:

- `[text]` - Rectangle
- `(text)` - Rounded rectangle
- `{text}` - Diamond (decision)
- `[(text)]` - Cylinder (database)
- `((text))` - Circle

### Line styles:

- `-->` - Solid arrow
- `-.->` - Dotted arrow
- `==>` - Thick arrow
- `--text-->` - Arrow with label
- `-->|text|` - Arrow with label (alt)

---

## Best Practices

1. **Keep diagrams focused** - One concept per diagram
2. **Use subgraphs** - Group related components
3. **Add context** - Labels on arrows explain relationships
4. **Color for emphasis** - Highlight problem areas in investigations
5. **Direction matters** - TD for hierarchies, LR for flows
6. **Update diagrams** - Keep them in sync with code changes
