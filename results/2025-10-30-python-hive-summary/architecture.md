# Architecture - 2025-10-30-python-hive

## Overview

This implementation uses a **Hive Mind Pattern** with Claude Flow orchestration. Four specialized agents worked in parallel:
- **Researcher** - Schema design and data model
- **Coder** - MCP server implementation
- **Tester** - BDD test suite creation
- **Analyst** - Documentation and analysis

## System Components

```
┌─────────────────────────────────────────────────────────────────┐
│                     Claude AI (MCP Client)                       │
└─────────────────────────────────────────────────────────────────┘
                              │
                              ▼ MCP Protocol
┌─────────────────────────────────────────────────────────────────┐
│                    MCP Server (FastMCP)                          │
│  ┌───────────┐ ┌───────────┐ ┌───────────┐ ┌─────────────────┐ │
│  │ Player    │ │ Team      │ │ Match     │ │ Competition     │ │
│  │ Tools     │ │ Tools     │ │ Tools     │ │ Tools           │ │
│  └───────────┘ └───────────┘ └───────────┘ └─────────────────┘ │
│                    ┌─────────────────────┐                      │
│                    │   Analysis Tools    │                      │
│                    └─────────────────────┘                      │
└─────────────────────────────────────────────────────────────────┘
                              │
                              ▼ Cypher Queries
┌─────────────────────────────────────────────────────────────────┐
│                    Neo4j Graph Database                          │
│  Nodes: Player, Team, Match, Competition, Stadium, Coach        │
│  Relationships: PLAYS_FOR, SCORED_IN, COMPETED_IN, etc.         │
└─────────────────────────────────────────────────────────────────┘
```

## Directory Structure

```
src/
├── __init__.py
├── config.py          # Pydantic settings configuration
├── database.py        # Neo4j connection management
├── models.py          # Pydantic data models
├── server.py          # Main MCP server
└── tools/
    ├── __init__.py
    ├── player_tools.py
    ├── team_tools.py
    ├── match_tools.py
    ├── competition_tools.py
    └── analysis_tools.py

tests/
├── features/          # BDD feature files
│   ├── player.feature
│   ├── team.feature
│   ├── match.feature
│   ├── competition.feature
│   └── analysis.feature
└── step_defs/         # Step definitions
```

## Key Design Decisions

1. **FastMCP Framework**: Uses FastMCP for cleaner MCP protocol implementation
2. **Pydantic Models**: Type-safe models with Field validators
3. **Modular Tools**: Separate tool modules by domain (player, team, match, etc.)
4. **Async Throughout**: Full async/await for database operations
5. **Docker Neo4j**: Simple docker-compose setup for database
6. **BDD Testing**: Given-When-Then structured tests with pytest-bdd

## Data Flow

1. **MCP Request** → Server receives tool call
2. **Tool Dispatch** → Routes to appropriate tool module
3. **Cypher Query** → Generates and executes Neo4j query
4. **Response** → Formats and returns structured data
