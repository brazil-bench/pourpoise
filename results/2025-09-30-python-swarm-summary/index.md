# Brazilian Soccer MCP Knowledge Graph - Documentation Index

## Purpose

This documentation provides comprehensive context for AI assistants working with the Brazilian Soccer MCP Knowledge Graph codebase. Use this index to quickly locate relevant information for any development or analysis task.

---

## Quick Reference

| If you need to... | Consult |
|-------------------|---------|
| Understand the project structure | [codebase_info.md](./codebase_info.md) |
| Learn about system design | [architecture.md](./architecture.md) |
| Find specific component details | [components.md](./components.md) |
| Understand the MCP tools API | [interfaces.md](./interfaces.md) |
| Work with entity models | [data_models.md](./data_models.md) |
| Follow data pipeline processes | [workflows.md](./workflows.md) |
| Check package dependencies | [dependencies.md](./dependencies.md) |

---

## Documentation Files

### [codebase_info.md](./codebase_info.md)
**Purpose:** Basic project information and structure overview

**Contains:**
- Project name and purpose
- Technology stack (Python, Neo4j, MCP, FastAPI)
- Directory structure with file descriptions
- Key metrics (files, tools, entities)
- Database configuration
- Build and run commands

**Use when:** Starting work on the project, understanding the codebase layout, or checking technology versions.

---

### [architecture.md](./architecture.md)
**Purpose:** System design and architectural patterns

**Contains:**
- High-level architecture diagram (Mermaid)
- Design patterns (MCP, Repository, Builder, DTO)
- Data flow sequence diagrams
- Graph database schema (ER diagram)
- Performance considerations
- Error handling strategy

**Use when:** Understanding how components interact, making architectural decisions, or adding new features.

---

### [components.md](./components.md)
**Purpose:** Detailed component documentation

**Contains:**
- MCP Server (`src/mcp_server/server.py`)
- Neo4j Database Layer (`src/graph/database.py`)
- Graph Schema Models (`src/graph/models.py`)
- Kaggle Loader (`src/data_pipeline/kaggle_loader.py`)
- Graph Builder (`src/data_pipeline/graph_builder.py`)
- MCP Tool Modules (`src/mcp_server/tools/`)
- CLI Interface (`main.py`)
- Component dependency diagram

**Use when:** Modifying specific components, understanding responsibilities, or debugging.

---

### [interfaces.md](./interfaces.md)
**Purpose:** API and interface documentation

**Contains:**
- All 13 MCP tool definitions with input schemas
- Player, Team, Match, Competition, Analysis tools
- MCP resource URIs
- Neo4jDatabase class interface
- CLI command reference
- Response format specifications

**Use when:** Implementing new tools, calling existing tools, or understanding the API contract.

---

### [data_models.md](./data_models.md)
**Purpose:** Entity models and relationship types

**Contains:**
- All 10 entity types with full attribute lists:
  - Player, Team, Match, Competition, Stadium
  - Coach, Season, Goal, Card, Transfer
- Enum definitions (Position, Competition Type, Transfer Type)
- Relationship type diagram and table
- GraphEntity base class

**Use when:** Working with data entities, modifying the schema, or querying the graph.

---

### [workflows.md](./workflows.md)
**Purpose:** Process flows and operational procedures

**Contains:**
- Data pipeline workflow (CSV to graph)
- MCP query workflow (Claude to Neo4j)
- CLI workflows (build, test-connection)
- BDD testing workflow
- Entity loading order
- Error handling workflow
- Caching strategy

**Use when:** Following operational procedures, debugging data flow, or understanding process sequences.

---

### [dependencies.md](./dependencies.md)
**Purpose:** Package dependencies and external services

**Contains:**
- All Python packages with versions and purposes
- Core dependencies (MCP, Neo4j, pandas)
- Testing dependencies (pytest-bdd)
- Development tools (black, mypy)
- External services (Neo4j, Kaggle)
- Dependency graph
- Installation instructions

**Use when:** Adding new dependencies, checking compatibility, or troubleshooting imports.

---

## Key Concepts

### Model Context Protocol (MCP)
The project implements an MCP server that exposes 13 tools for Claude AI to query Brazilian soccer data. Tools are organized by category: Player, Team, Match, Competition, and Analysis.

### Neo4j Graph Database
Data is stored in a Neo4j graph database with 10 entity types and 15+ relationship types. The schema is defined in `src/graph/models.py`.

### BDD Testing
Tests use pytest-bdd with Gherkin feature files in `tests/features/` and step implementations in `tests/step_defs/`.

### CLI Interface
The `main.py` CLI provides commands for building, validating, and managing the graph database.

---

## Common Tasks

### Adding a New MCP Tool
1. Create tool method in appropriate `tools/*.py` file
2. Register tool in `server.py` `handle_list_tools()`
3. Add handler in `handle_call_tool()`
4. Add BDD test scenario in `tests/features/`
5. Update [interfaces.md](./interfaces.md)

### Adding a New Entity Type
1. Define dataclass in `src/graph/models.py`
2. Add constraint in `GraphSchema.create_constraints()`
3. Add indexes in `GraphSchema.create_indexes()`
4. Implement loading in `graph_builder.py`
5. Update [data_models.md](./data_models.md)

### Debugging Query Issues
1. Check query syntax in tool implementation
2. Verify indexes exist with `python main.py schema`
3. Test connection with `python main.py test-connection`
4. Check cache expiration (30 min TTL)
5. Review logs for error details

---

## File Locations

| Component | Path |
|-----------|------|
| Main CLI | `main.py` |
| MCP Server | `src/mcp_server/server.py` |
| Database Layer | `src/graph/database.py` |
| Entity Models | `src/graph/models.py` |
| Data Loader | `src/data_pipeline/kaggle_loader.py` |
| Graph Builder | `src/data_pipeline/graph_builder.py` |
| Player Tools | `src/mcp_server/tools/player_tools.py` |
| Team Tools | `src/mcp_server/tools/team_tools.py` |
| Match Tools | `src/mcp_server/tools/match_tools.py` |
| Analysis Tools | `src/mcp_server/tools/analysis_tools.py` |
| Test Features | `tests/features/` |
| Configuration | `config/config.yaml` |

---

## Version Information

- **Documentation Generated:** 2025-11-29
- **Python Version:** 3.8+
- **Neo4j Version:** 5.15.0
- **MCP Version:** 1.0.0
