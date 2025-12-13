# Codebase Summary: 2025-12-13-python-claude-swarm

## Quick Overview

| Attribute | Value |
|-----------|-------|
| **Pattern** | Claude Swarm (Opus 4.5) |
| **Language** | Python 3.9+ |
| **Database** | Neo4j |
| **Framework** | MCP Server |
| **Lines of Code** | 4,227 (src/) |
| **Python Files** | 17 |
| **BDD Scenarios** | 38 |
| **Real Data** | ~11 MB Kaggle |

## Documentation Index

- [Architecture](architecture.md) - System design and query handlers
- [Components](components.md) - MCP tools and data models
- [Dependencies](dependencies.md) - External libraries

## Key Highlights

### Strengths
- **Single commit implementation**: All phases in one clean commit
- **Zero fix commits**: Clean first-pass implementation
- **Real Kaggle data**: All 6 CSV datasets (~11 MB, 40k+ records)
- **Query handler pattern**: Clean separation of concerns
- **Team name normalization**: Handles Brazilian team name variations
- **Comprehensive BDD tests**: 38 scenarios across 3 features

### Implementation Pattern
Uses **Claude Code with Swarm orchestration** (Opus 4.5):
- Single implementation commit
- No intermediate fix commits
- Co-authored by Claude Opus 4.5

### Data Strategy
**Real Kaggle Data** pre-downloaded:
- 6 CSV files totaling ~11 MB
- FIFA player database (18,000+ players)
- Brazilian match history (20,000+ matches)
- Multiple competitions covered

## Files of Interest

| File | Purpose |
|------|---------|
| `src/brazilian_soccer_mcp/mcp_server.py` | Main server (6 MCP tools) |
| `src/brazilian_soccer_mcp/query_handlers.py` | Domain query handlers |
| `src/brazilian_soccer_mcp/models.py` | Pydantic models |
| `src/brazilian_soccer_mcp/data_loaders.py` | CSV data loaders |
| `src/brazilian_soccer_mcp/utils/team_normalizer.py` | Team name normalization |
