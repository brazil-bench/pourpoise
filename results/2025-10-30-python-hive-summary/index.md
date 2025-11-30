# Brazilian Soccer MCP Knowledge Graph (Hive Pattern) - Documentation Index

## Purpose

This documentation provides comprehensive context for AI assistants working with the 2025-10-30-python-hive implementation of the Brazilian Soccer MCP Knowledge Graph.

---

## Quick Reference

| If you need to... | Consult |
|-------------------|---------|
| Understand the project structure | [codebase_info.md](./codebase_info.md) |
| Learn about system design | [architecture.md](./architecture.md) |
| Find specific component details | [components.md](./components.md) |
| Understand the MCP tools API | [interfaces.md](./interfaces.md) |
| Work with entity models | [data_models.md](./data_models.md) |
| Check package dependencies | [dependencies.md](./dependencies.md) |

---

## Project Overview

**Implementation Pattern:** Hive (Claude-Flow orchestration with SPARC methodology)
**Language:** Python 3.8+
**Database:** Neo4j 5.14+
**MCP Framework:** FastMCP 0.2.0

## Key Metrics

| Metric | Value |
|--------|-------|
| Lines of Code (src) | 7,090 |
| Python Files (src) | 22 |
| MCP Tools Implemented | 18 (all spec requirements) |
| BDD Test Scenarios | 64 |
| Test Step Definitions | 175+ |
| Total Commits | 7 |
| Development Duration | ~9 hours |

## Spec Compliance

All 18 MCP tools from the specification are implemented:

### Player Tools (4/4)
- [x] `search_player(name, team?, position?)`
- [x] `get_player_stats(player_id, season?)`
- [x] `get_player_career(player_id)`
- [x] `get_player_transfers(player_id)`

### Team Tools (4/4)
- [x] `search_team(name)`
- [x] `get_team_roster(team_id, season?)`
- [x] `get_team_stats(team_id, season?)`
- [x] `get_team_history(team_id)`

### Match Tools (4/4)
- [x] `get_match_details(match_id)`
- [x] `search_matches(team?, date_from?, date_to?)`
- [x] `get_head_to_head(team1_id, team2_id)`
- [x] `get_match_scorers(match_id)`

### Competition Tools (3/3)
- [x] `get_competition_standings(competition_id, season)`
- [x] `get_competition_top_scorers(competition_id, season)`
- [x] `get_competition_matches(competition_id, season)`

### Analysis Tools (3/3)
- [x] `find_common_teammates(player1_id, player2_id)`
- [x] `get_rivalry_stats(team1_id, team2_id)`
- [x] `find_players_by_career_path(criteria)`

## Architecture Highlights

- **FastMCP Framework:** Modern async MCP server implementation
- **Neo4j Backend:** Graph database for relationship queries
- **Tool Separation:** Dedicated modules per domain (player, team, match, etc.)
- **BDD Testing:** Comprehensive Gherkin scenarios with pytest-bdd
- **SPARC Methodology:** Specification-Pseudocode-Architecture-Refinement-Completion

## File Locations

| Component | Path |
|-----------|------|
| MCP Server | `src/server.py` |
| Database Layer | `src/database.py` |
| Entity Models | `src/models.py` |
| Configuration | `src/config.py` |
| Player Tools | `src/tools/player_tools.py` |
| Team Tools | `src/tools/team_tools.py` |
| Match Tools | `src/tools/match_tools.py` |
| Competition Tools | `src/tools/competition_tools.py` |
| Analysis Tools | `src/tools/analysis_tools.py` |
| BDD Features | `tests/features/` |
| Test Steps | `tests/steps/` |

---

## Version Information

- **Documentation Generated:** 2025-11-29
- **Attempt Date:** 2025-10-30
- **Pattern:** Hive (Claude-Flow)
