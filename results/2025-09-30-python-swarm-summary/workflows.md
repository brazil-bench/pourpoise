# Workflows

## Data Pipeline Workflow

The complete data pipeline from raw CSV to queryable graph.

```mermaid
flowchart TD
    A[CSV Data Files] --> B[KaggleLoader]
    B --> C{Parse & Validate}
    C -->|Valid| D[Extract Entities]
    C -->|Invalid| E[Log Errors]
    D --> F[GraphBuilder]
    F --> G[Create Schema]
    G --> H[Create Nodes]
    H --> I[Create Relationships]
    I --> J[Validate Graph]
    J -->|Pass| K[Ready for Queries]
    J -->|Fail| L[Report Issues]
```

### Step 1: Data Loading
```python
loader = KaggleLoader(data_dir="data")
data = loader.load_brazilian_championship_data()
```

### Step 2: Graph Building
```python
builder = GraphBuilder(db_connection)
stats = builder.build_complete_graph(data)
```

### Step 3: Validation
```python
validation = builder.validate_graph_integrity()
```

---

## MCP Query Workflow

How Claude interacts with the knowledge graph.

```mermaid
sequenceDiagram
    participant User
    participant Claude
    participant MCP as MCP Server
    participant Cache
    participant Neo4j

    User->>Claude: "Who scored the most for Flamengo?"
    Claude->>MCP: call_tool("get_team_stats", {team_name: "Flamengo"})
    MCP->>Cache: Check cache
    alt Cache Hit
        Cache-->>MCP: Cached result
    else Cache Miss
        MCP->>Neo4j: Execute Cypher query
        Neo4j-->>MCP: Query results
        MCP->>Cache: Store result
    end
    MCP-->>Claude: Formatted JSON response
    Claude-->>User: "Gabriel Barbosa scored 15 goals..."
```

---

## CLI Workflow

### Build Workflow
```mermaid
flowchart TD
    A[python main.py build] --> B{--clear-first?}
    B -->|Yes| C[Confirm deletion]
    C -->|Confirmed| D[Clear database]
    B -->|No| E[Test connection]
    D --> E
    E -->|Success| F[Setup schema]
    E -->|Fail| G[Exit with error]
    F --> H[Build graph]
    H --> I{--validate?}
    I -->|Yes| J[Validate integrity]
    I -->|No| K[Complete]
    J --> K
```

### Test Connection Workflow
```mermaid
flowchart TD
    A[python main.py test-connection] --> B[Load config]
    B --> C[Create driver]
    C --> D{Connect?}
    D -->|Success| E[Query database info]
    D -->|Fail| F[Report error]
    E --> G[Display statistics]
```

---

## Testing Workflow

### BDD Test Execution
```mermaid
flowchart TD
    A[pytest tests/] --> B[Load features]
    B --> C[Setup fixtures]
    C --> D[Connect to Neo4j]
    D --> E[Run scenarios]
    E --> F{All pass?}
    F -->|Yes| G[Report success]
    F -->|No| H[Report failures]
    H --> I[Generate HTML report]
```

### Example BDD Scenario
```gherkin
Feature: Player Search
  Scenario: Search for famous Brazilian player
    Given the knowledge graph contains player data
    When I search for "Neymar"
    Then I should get player details with career information
```

---

## Entity Loading Order

The graph builder follows a specific order to handle dependencies:

```mermaid
flowchart TD
    subgraph "Phase 1: Independent Entities"
        A[Competitions]
        B[Stadiums]
        C[Teams]
        D[Coaches]
        E[Seasons]
    end

    subgraph "Phase 2: Player Entities"
        F[Players]
    end

    subgraph "Phase 3: Match Entities"
        G[Matches]
    end

    subgraph "Phase 4: Event Entities"
        H[Goals]
        I[Cards]
        J[Transfers]
    end

    A --> G
    B --> G
    C --> G
    C --> F
    D --> C
    E --> G
    F --> H
    F --> I
    F --> J
    G --> H
    G --> I
```

---

## Error Handling Workflow

```mermaid
flowchart TD
    A[Operation] --> B{Try execute}
    B -->|Success| C[Return result]
    B -->|Connection Error| D[Log error]
    D --> E{Retryable?}
    E -->|Yes| F[Wait & retry]
    F --> B
    E -->|No| G[Raise exception]
    B -->|Query Error| H[Log with context]
    H --> I[Return error response]
```

---

## Caching Strategy

```mermaid
flowchart TD
    A[Query request] --> B{In cache?}
    B -->|Yes| C{Expired?}
    C -->|No| D[Return cached]
    C -->|Yes| E[Remove from cache]
    B -->|No| F[Execute query]
    E --> F
    F --> G[Store in cache]
    G --> H[Return result]
```

**Cache Configuration:**
- TTL: 30 minutes
- Key: Query hash + parameters
- Invalidation: On schema changes
