# Codebase Summary: 2025-12-01-python-claude-beads

## Quick Overview

| Attribute | Value |
|-----------|-------|
| **Pattern** | Beads (Issue-Driven AI) |
| **Language** | Python 3.12 |
| **Database** | Neo4j |
| **Framework** | MCP Server |
| **Lines of Code** | 1,826 (src/) |
| **Python Files** | 14 |
| **BDD Scenarios** | 18 |

## Documentation Index

- [Architecture](architecture.md) - System design with Beads pattern
- [Components](components.md) - MCP tools and data models
- [Dependencies](dependencies.md) - External libraries

## Key Highlights

### Strengths
- **Extremely lean**: Only 1,826 LOC (vs 3,545 Hive, 8,683 Swarm)
- **Zero fix commits**: Clean implementation with no rework
- **Fast autonomous work**: ~11 minutes for all phases
- **Modern Python**: Uses 3.12 with pyproject.toml
- **Simple dataclasses**: No Pydantic overhead
- **Beads coordination**: Issue-driven AI workflow

### Orchestration Pattern
Uses **Beads** - Steve Yegge's AI-native issue tracking:
- Issues created with dependencies
- All tracked in `.beads/issues.jsonl`
- CLI-first (`bd` commands)
- Git-native sync

### Beads Issues Tracked
1. **Phase 1**: Data Preparation (~11 min)
2. **Phase 2**: MCP Server Development (~11 min)
3. **Phase 3**: Integration & Testing (~11 min)

## Files of Interest

| File | Purpose |
|------|---------|
| `src/brazilian_soccer_mcp/server.py` | All 12 MCP tools |
| `src/brazilian_soccer_mcp/models.py` | Dataclass models |
| `src/brazilian_soccer_mcp/database.py` | Neo4j connection |
| `.beads/issues.jsonl` | Issue tracking history |
