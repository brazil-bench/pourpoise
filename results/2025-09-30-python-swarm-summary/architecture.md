# Architecture - 2025-09-30-python-swarm

## Overview

This implementation uses a **Swarm Pattern** with Claude Flow orchestration to build a Brazilian Soccer MCP Knowledge Graph server.

## System Components

```
┌─────────────────────────────────────────────────────────────────┐
│                     MCP Client (Claude)                          │
└─────────────────────────────────────────────────────────────────┘
                              │
                              ▼
┌─────────────────────────────────────────────────────────────────┐
│                    MCP Server Layer                              │
│  ┌─────────────┐ ┌─────────────┐ ┌─────────────┐ ┌────────────┐ │
│  │PlayerTools  │ │TeamTools    │ │MatchTools   │ │AnalysisTools│
│  └─────────────┘ └─────────────┘ └─────────────┘ └────────────┘ │
└─────────────────────────────────────────────────────────────────┘
                              │
                              ▼
┌─────────────────────────────────────────────────────────────────┐
│                    Graph Database Layer                          │
│  ┌─────────────┐ ┌─────────────┐ ┌─────────────┐               │
│  │  Models     │ │  Queries    │ │  Database   │               │
│  └─────────────┘ └─────────────┘ └─────────────┘               │
└─────────────────────────────────────────────────────────────────┘
                              │
                              ▼
┌─────────────────────────────────────────────────────────────────┐
│                         Neo4j                                    │
└─────────────────────────────────────────────────────────────────┘
```

## Directory Structure

```
src/
├── data_pipeline/
│   ├── kaggle_loader.py    # Load Kaggle CSV data
│   └── graph_builder.py    # Build Neo4j graph from data
├── graph/
│   ├── models.py           # Data models (Player, Team, Match, etc.)
│   ├── database.py         # Neo4j connection management
│   ├── queries.py          # Cypher query builders
│   └── schema.py           # Graph schema definitions
├── mcp_server/
│   ├── server.py           # Main MCP server
│   ├── http_server.py      # HTTP wrapper for testing
│   ├── config.py           # Server configuration
│   └── tools/
│       ├── player_tools.py
│       ├── team_tools.py
│       ├── match_tools.py
│       └── analysis_tools.py
└── utils/
    └── data_utils.py       # Utility functions
```

## Key Design Decisions

1. **Neo4j Graph Database**: Uses Neo4j for storing and querying the knowledge graph
2. **Modular Tool Architecture**: Separate tool modules for players, teams, matches, and analysis
3. **Async Operations**: Full async/await support for database operations
4. **Caching Layer**: 30-minute TTL cache for frequently accessed data
5. **Pydantic Models**: Type-safe data models with validation

## Data Flow

1. **Data Loading**: Kaggle CSV files → kaggle_loader.py → Neo4j
2. **Query Processing**: MCP request → Tool module → Cypher query → Neo4j → Response
3. **Caching**: Results cached in memory with TTL for performance
