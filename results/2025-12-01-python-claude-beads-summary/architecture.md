# Architecture - 2025-12-01-python-claude-beads

## Overview

This implementation uses **Beads** - Steve Yegge's AI-native issue tracking system that lives in the repository. Issues tracked work as coordination mechanism for AI development.

## Pattern: Beads (Issue-Driven AI Coordination)

Unlike Swarm or Hive Mind patterns that spawn parallel agents, Beads provides:
- **Issue tracking** in `.beads/issues.jsonl`
- **Dependency management** between tasks
- **CLI-first** workflow (`bd` commands)
- **Git-native** sync

## System Components

```
┌─────────────────────────────────────────────────────────────────┐
│                     Claude AI (MCP Client)                       │
└─────────────────────────────────────────────────────────────────┘
                              │
                              ▼ MCP Protocol
┌─────────────────────────────────────────────────────────────────┐
│                    MCP Server (server.py)                        │
│  ┌───────────────────────────────────────────────────────────┐  │
│  │  12 MCP Tools: search_player, get_player_stats, ...       │  │
│  └───────────────────────────────────────────────────────────┘  │
└─────────────────────────────────────────────────────────────────┘
                              │
                              ▼ Cypher Queries
┌─────────────────────────────────────────────────────────────────┐
│                    Neo4j Graph Database                          │
│  Nodes: Player, Team, Match, Competition, Stadium, Coach        │
└─────────────────────────────────────────────────────────────────┘
```

## Directory Structure

```
.
├── .beads/                    # Beads issue tracking
│   ├── issues.jsonl           # Issue database
│   ├── config.yaml            # Beads configuration
│   └── metadata.json          # Repo metadata
├── src/
│   └── brazilian_soccer_mcp/
│       ├── __init__.py
│       ├── server.py          # Main MCP server (12 tools)
│       ├── database.py        # Neo4j connection
│       ├── models.py          # Dataclass models
│       ├── data_loader.py     # Sample data loader
│       └── kaggle_loader.py   # Kaggle data loader
└── tests/
    ├── features/              # BDD feature files
    │   ├── player_search.feature
    │   ├── team_operations.feature
    │   ├── match_queries.feature
    │   └── analysis.feature
    └── step_defs/             # Step definitions
```

## Key Design Decisions

1. **Minimalist Implementation**: Only 1,826 LOC total
2. **Standard Python Dataclasses**: No Pydantic, simple dataclasses
3. **Single Server File**: All 12 tools in one server.py
4. **Modern pyproject.toml**: Uses hatchling build system
5. **Python 3.12**: Latest Python version
6. **Beads Coordination**: Issue-driven development workflow

## Beads Workflow

Issues were created with dependencies:
```
Phase 1 (Data) ──blocks──> Phase 2 (MCP) ──blocks──> Phase 3 (Tests)
```

All phases closed within ~11 minutes of creation.
