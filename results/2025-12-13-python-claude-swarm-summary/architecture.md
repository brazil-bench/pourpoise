# Architecture - 2025-12-13-python-claude-swarm

## Overview

This implementation uses **Claude Code with Swarm orchestration** (Claude Opus 4.5). The entire implementation was completed in a single commit, suggesting efficient autonomous development.

## System Components

```
┌─────────────────────────────────────────────────────────────────┐
│                     Claude AI (MCP Client)                       │
└─────────────────────────────────────────────────────────────────┘
                              │
                              ▼ MCP Protocol
┌─────────────────────────────────────────────────────────────────┐
│              BrazilianSoccerMCPServer                            │
│  ┌─────────────────────────────────────────────────────────────┐│
│  │                    Query Handlers                            ││
│  │  ┌───────────┐ ┌───────────┐ ┌───────────┐ ┌────────────┐  ││
│  │  │ Match     │ │ Team      │ │ Player    │ │Competition │  ││
│  │  │ Handler   │ │ Handler   │ │ Handler   │ │ Handler    │  ││
│  │  └───────────┘ └───────────┘ └───────────┘ └────────────┘  ││
│  │                    ┌─────────────────────┐                  ││
│  │                    │ Statistics Handler  │                  ││
│  │                    └─────────────────────┘                  ││
│  └─────────────────────────────────────────────────────────────┘│
└─────────────────────────────────────────────────────────────────┘
                              │
                              ▼ Cypher Queries
┌─────────────────────────────────────────────────────────────────┐
│                    Neo4j Graph Database                          │
│  Nodes: Team, Match, Player, Competition                        │
│  Real Kaggle Data: ~11MB (6 CSV files)                          │
└─────────────────────────────────────────────────────────────────┘
```

## Directory Structure

```
src/brazilian_soccer_mcp/
├── __init__.py
├── __main__.py           # Entry point
├── config.py             # Pydantic settings
├── models.py             # Pydantic data models
├── schema.py             # Neo4j schema definitions
├── neo4j_manager.py      # Database connection
├── mcp_server.py         # Main MCP server (6 tools)
├── query_handlers.py     # Query handler classes
├── data_loaders.py       # CSV data loaders
├── statistics.py         # Statistics calculations
└── utils/
    ├── __init__.py
    └── team_normalizer.py  # Team name normalization

tests/
├── conftest.py           # pytest fixtures
├── fixtures/
│   └── sample_data.py    # Test data
├── features/             # BDD feature files
│   ├── players.feature
│   ├── teams.feature
│   └── matches.feature
└── step_defs/            # Step definitions
    ├── test_players.py
    ├── test_teams.py
    └── test_matches.py

data/kaggle/              # Real Kaggle data (~11MB)
├── Brasileirao_Matches.csv
├── Brazilian_Cup_Matches.csv
├── Libertadores_Matches.csv
├── BR-Football-Dataset.csv
├── novo_campeonato_brasileiro.csv
└── fifa_data.csv
```

## Key Design Decisions

1. **Query Handler Pattern**: Separate handler classes for each domain (Match, Team, Player, etc.)
2. **Pydantic Models**: Type-safe models with validators
3. **Team Name Normalization**: Dedicated utility for handling name variations
4. **Real Data Integration**: All 6 Kaggle datasets pre-loaded
5. **Single Implementation Commit**: Clean, efficient development

## Data Flow

1. **Data Loading**: Kaggle CSV → data_loaders.py → Neo4j
2. **Query Processing**: MCP request → Server → Query Handler → Neo4j
3. **Response Formatting**: Results → Pydantic models → MCP TextContent
