# Codebase Summary: 2025-10-30-python-hive

## Quick Overview

| Attribute | Value |
|-----------|-------|
| **Pattern** | Hive Mind (Claude Flow) |
| **Language** | Python |
| **Database** | Neo4j |
| **Framework** | FastMCP |
| **Lines of Code** | 3,545 (src/) |
| **Python Files** | 21 |
| **BDD Scenarios** | 64 (100% pass) |

## Documentation Index

- [Architecture](architecture.md) - System design and data flow
- [Components](components.md) - MCP tools and graph entities
- [Dependencies](dependencies.md) - External libraries used

## Key Highlights

### Strengths
- Lean implementation (3,545 LOC vs 8,683 for swarm)
- Faster autonomous completion (~41 min vs ~1h 49m)
- 64 BDD test scenarios with 100% pass rate
- Clean separation of concerns
- Well-documented with context blocks
- Docker-ready deployment

### Orchestration Pattern
Used **Hive Mind** with 4 parallel agents:
1. **Researcher** - Schema design
2. **Coder** - Server implementation
3. **Tester** - BDD test suite
4. **Analyst** - Documentation

### Implementation Notes
- Uses FastMCP (cleaner than raw MCP)
- Pydantic for all models
- pytest-bdd for Given-When-Then testing
- Single docker-compose for Neo4j

## Files of Interest

| File | Purpose |
|------|---------|
| `src/server.py` | Main MCP server |
| `src/models.py` | Pydantic data models |
| `src/database.py` | Neo4j connection |
| `tests/features/*.feature` | BDD scenarios |
