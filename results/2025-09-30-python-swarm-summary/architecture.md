# System Architecture

## Overview

The Brazilian Soccer MCP Knowledge Graph follows a layered architecture designed for extensibility and separation of concerns.

```mermaid
graph TB
    subgraph "Client Layer"
        Claude[Claude AI]
        CLI[CLI Interface]
    end

    subgraph "MCP Server Layer"
        Server[MCP Server]
        Tools[Tool Handlers]
        Cache[Query Cache]
    end

    subgraph "Data Access Layer"
        DB[Neo4j Database]
        Models[Graph Models]
        Queries[Query Builders]
    end

    subgraph "Data Pipeline"
        Loader[Kaggle Loader]
        Builder[Graph Builder]
    end

    Claude --> Server
    CLI --> Server
    Server --> Tools
    Tools --> Cache
    Tools --> Queries
    Queries --> DB
    Loader --> Builder
    Builder --> DB
```

## Design Patterns

### 1. Model Context Protocol (MCP)
The system implements the MCP specification for Claude AI integration:
- **Server:** Handles initialization, capability negotiation
- **Tools:** 13 specialized query tools exposed to Claude
- **Resources:** Schema documentation and help content

### 2. Repository Pattern
Database access is abstracted through the `Neo4jDatabase` class:
- Connection pooling and session management
- Transaction support for batch operations
- Query caching for performance

### 3. Builder Pattern
Graph construction uses a systematic builder approach:
- Entities created in dependency order
- Batch processing for large datasets
- Rollback capability on failure

### 4. Data Transfer Objects
Entity models use Python dataclasses:
- Type-safe attribute definitions
- Automatic serialization to Neo4j properties
- Enum support for constrained values

## Data Flow

```mermaid
sequenceDiagram
    participant C as Claude
    participant S as MCP Server
    participant T as Tool Handler
    participant D as Neo4j

    C->>S: Tool Call (e.g., search_player)
    S->>T: Route to PlayerTools
    T->>D: Execute Cypher Query
    D-->>T: Query Results
    T->>T: Format Response
    T-->>S: Structured Data
    S-->>C: JSON Response
```

## Graph Database Schema

```mermaid
erDiagram
    PLAYER ||--o{ PLAYS_FOR : has
    PLAYER ||--o{ SCORED : made
    PLAYER ||--o{ RECEIVED : got
    TEAM ||--o{ PLAYS_FOR : employs
    TEAM ||--o{ HOME_TEAM : is
    TEAM ||--o{ AWAY_TEAM : is
    TEAM ||--o{ PLAYS_AT : uses
    MATCH ||--o{ HOME_TEAM : has
    MATCH ||--o{ AWAY_TEAM : has
    MATCH ||--o{ PART_OF : belongs
    MATCH ||--o{ PLAYED_AT : location
    COMPETITION ||--o{ PART_OF : contains
    STADIUM ||--o{ PLAYS_AT : hosts
    STADIUM ||--o{ PLAYED_AT : venue
```

## Performance Considerations

| Aspect | Strategy |
|--------|----------|
| Query Caching | 30-minute TTL in-memory cache |
| Connection Pooling | Max 50 connections |
| Batch Operations | 1000 records per batch |
| Indexing | Indexes on frequently queried properties |
| Constraints | Unique constraints on entity IDs |

## Error Handling Strategy

1. **Connection Errors:** Automatic reconnection with exponential backoff
2. **Query Errors:** Logged with full context, graceful degradation
3. **Tool Errors:** Returned as structured error messages to Claude
4. **Validation Errors:** Pre-execution parameter validation
