# Component Documentation

## Source Components

### server.py (511 lines)
**Purpose:** MCP Server implementation with tool definitions and handlers

**MCP Tools:**
| Tool | Description | Required Params |
|------|-------------|-----------------|
| find_matches | Search matches by team/season/competition | team |
| get_head_to_head | Head-to-head record between two teams | team1, team2 |
| get_team_stats | Comprehensive team statistics | team |
| find_players | Search player database | - |
| get_top_players | Top-rated players by criteria | - |
| get_standings | League standings for season | competition, season |
| get_biggest_wins | Matches with largest margins | - |
| get_league_stats | Aggregate competition statistics | competition |
| get_competition_winners | Historical winners | competition |
| list_teams | List all teams | - |

**Key Functions:**
- `list_tools()` - Returns all available MCP tools
- `call_tool()` - Routes tool calls to appropriate handlers
- `initialize_database()` - Schema setup and data loading
- `run_server()` - Main server loop with stdio transport

### database.py (651 lines)
**Purpose:** Neo4j connection management and data loading

**Classes:**
| Class | Purpose |
|-------|---------|
| Neo4jConnection | Database connection and query execution |
| DataLoader | CSV file loading into Neo4j |

**Key Features:**
- Connection pooling
- Schema creation with constraints
- Batch data loading
- Team name normalization
- NaN-safe type conversion

### queries.py (854 lines)
**Purpose:** Query builders and executors for all data retrieval

**Classes:**
| Class | Purpose |
|-------|---------|
| QueryExecutor | High-level query interface |

**Query Categories:**
1. **Match Queries** - find_matches_by_team, find_matches_between_teams
2. **Team Queries** - get_team_statistics, get_all_teams
3. **Player Queries** - find_players, get_top_players_by_rating
4. **Analytics** - get_season_standings, get_biggest_wins, get_league_statistics

### models.py (285 lines)
**Purpose:** Data models and team name normalization

**Dataclasses:**
| Model | Fields |
|-------|--------|
| Team | name, state, original_names |
| Player | id, name, age, nationality, overall, potential, club, position |
| Match | id, datetime, home_team, away_team, home_goals, away_goals, competition, season, round, result, stadium, statistics |
| Competition | name, short_name, country, type |

**Constants:**
- BRASILEIRAO, COPA_DO_BRASIL, LIBERTADORES - Competition definitions
- Team name variations mapping for normalization

### __init__.py (45 lines)
**Purpose:** Package exports and version info

## Test Components

### Feature Files (tests/features/)
| File | Scenarios |
|------|-----------|
| match_queries.feature | 6 scenarios |
| team_queries.feature | 3 scenarios |
| player_queries.feature | 5 scenarios |
| analytics.feature | 4 scenarios |

### Test Implementations
| File | Tests | Coverage |
|------|-------|----------|
| test_match_queries.py | 6 | Head-to-head, filtering, home/away |
| test_team_queries.py | 3 | Team stats, season filtering |
| test_player_queries.py | 5 | Nationality, club, ratings |
| test_analytics.py | 4 | Standings, biggest wins, league stats |

### conftest.py (111 lines)
**Shared Fixtures:**
- `neo4j_connection` - Mock database connection
- `query_executor` - Query executor with mock data
- `sample_matches`, `sample_players` - Test data fixtures
