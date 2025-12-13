# Architecture: 2025-12-13-python-claude-hive

## System Design

### Hive Mind Orchestration
```
┌─────────────────────────────────────────────────────────┐
│                    QUEEN COORDINATOR                     │
│                    (Strategic Type)                      │
│                                                          │
│  ┌─────────┐ ┌─────────┐ ┌─────────┐ ┌─────────┐       │
│  │Researcher│ │ Coder 1 │ │ Coder 2 │ │ Coder 3 │       │
│  │         │ │         │ │         │ │         │       │
│  │ Docs    │ │ Phase 1 │ │ Phase 2 │ │ Phase 3 │       │
│  └────┬────┘ └────┬────┘ └────┬────┘ └────┬────┘       │
│       │           │           │           │             │
│       └───────────┴─────┬─────┴───────────┘             │
│                         │                                │
│                   ┌─────┴─────┐                          │
│                   │  Tester   │                          │
│                   │           │                          │
│                   │ BDD Tests │                          │
│                   └───────────┘                          │
└─────────────────────────────────────────────────────────┘
```

### Data Flow
```
┌──────────────┐     ┌──────────────┐     ┌──────────────┐
│   CSV Files  │────▶│  DataLoader  │────▶│ QueryEngine  │
│   (Kaggle)   │     │              │     │              │
└──────────────┘     └──────────────┘     └──────┬───────┘
                                                  │
                                                  ▼
                     ┌──────────────┐     ┌──────────────┐
                     │   Neo4j DB   │◀────│ Neo4jClient  │
                     │              │     │              │
                     └──────────────┘     └──────────────┘
```

### Layer Architecture
```
┌─────────────────────────────────────────────────────────┐
│                    PRESENTATION LAYER                    │
│  (Query interface, Response formatting)                  │
├─────────────────────────────────────────────────────────┤
│                     QUERY LAYER                          │
│  QueryEngine, Statistics, HeadToHeadStats               │
├─────────────────────────────────────────────────────────┤
│                      DATA LAYER                          │
│  DataLoader, TeamNormalizer, Models                      │
├─────────────────────────────────────────────────────────┤
│                    STORAGE LAYER                         │
│  Neo4jClient, GraphSchema, GraphQueries                 │
├─────────────────────────────────────────────────────────┤
│                   PERSISTENCE LAYER                      │
│  CSV Files (Kaggle), Neo4j Database                      │
└─────────────────────────────────────────────────────────┘
```

## Design Patterns

### 1. Data Transfer Objects (DTOs)
- `Team`, `Player`, `Match`, `Competition` dataclasses
- Immutable representations of domain entities
- Type-safe with full type hints

### 2. Repository Pattern
- `DataLoader` acts as repository for CSV data
- `Neo4jClient` provides graph database access
- Abstracts storage details from query logic

### 3. Builder Pattern
- Index building in `QueryEngine._build_indexes()`
- Lazy initialization of data structures

### 4. Template Method
- `GraphQueries` provides Cypher query templates
- Parameterized queries for safety and reuse

## Module Dependencies

```
models.py
    │
    ▼
data_loader.py ──────┐
    │                │
    ▼                ▼
team_normalizer.py   statistics.py
    │                │
    └───────┬────────┘
            ▼
    query_engine.py
            │
            ▼
    neo4j_client.py
            │
            ▼
    graph_schema.py ◀── graph_queries.py
```

## Key Design Decisions

### 1. Fuzzy Team Name Matching
Uses `difflib.get_close_matches()` with 0.6 cutoff for flexible team name queries.

### 2. Index-Based Lookups
Pre-built indexes for:
- `matches_by_team`
- `matches_by_competition`
- `matches_by_season`

### 3. Graph Schema
6 Node Types:
- Team, Player, Match, Competition, Season, Stadium

8 Relationship Types:
- PLAYED_HOME, PLAYED_AWAY, PLAYS_FOR, COMPETED_IN, etc.

### 4. Environment-Based Configuration
`neo4j_config.py` supports:
- Environment variables
- .env file loading
- Sensible defaults
