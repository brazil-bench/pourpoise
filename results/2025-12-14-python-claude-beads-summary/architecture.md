# Architecture

## System Overview

```
┌─────────────────────────────────────────────────────────────┐
│                     MCP Server (server.py)                   │
│  ┌─────────────────────────────────────────────────────────┐│
│  │                    10 MCP Tools                          ││
│  │  find_matches | get_head_to_head | get_team_stats       ││
│  │  find_players | get_top_players | get_standings         ││
│  │  get_biggest_wins | get_league_stats | list_teams       ││
│  │  get_competition_winners                                 ││
│  └─────────────────────────────────────────────────────────┘│
└─────────────────────────────────────────────────────────────┘
                              │
                              ▼
┌─────────────────────────────────────────────────────────────┐
│                  Query Executor (queries.py)                 │
│  - find_matches_by_team()      - get_team_statistics()      │
│  - find_matches_between_teams() - find_players()            │
│  - get_season_standings()      - get_biggest_wins()         │
│  - get_league_statistics()     - get_competition_winners()  │
└─────────────────────────────────────────────────────────────┘
                              │
                              ▼
┌─────────────────────────────────────────────────────────────┐
│                 Database Layer (database.py)                 │
│  ┌─────────────────┐  ┌─────────────────────────────────┐   │
│  │ Neo4jConnection │  │ DataLoader                      │   │
│  │ - connect()     │  │ - load_brasileirao_matches()    │   │
│  │ - execute()     │  │ - load_copa_brasil_matches()    │   │
│  │ - execute_write │  │ - load_libertadores_matches()   │   │
│  │ - setup_schema  │  │ - load_extended_stats()         │   │
│  └─────────────────┘  │ - load_historical_brasileirao() │   │
│                       │ - load_fifa_players()           │   │
│                       └─────────────────────────────────┘   │
└─────────────────────────────────────────────────────────────┘
                              │
                              ▼
┌─────────────────────────────────────────────────────────────┐
│                      Neo4j Database                          │
│  ┌─────────────────────────────────────────────────────────┐│
│  │  Nodes: Team, Player, Match, Competition, Season        ││
│  │  Relationships: HOME_TEAM, AWAY_TEAM, PART_OF, IN_SEASON││
│  └─────────────────────────────────────────────────────────┘│
└─────────────────────────────────────────────────────────────┘
```

## Neo4j Schema

### Nodes
| Node | Key Properties |
|------|----------------|
| Team | name, state, original_names |
| Player | id, name, age, nationality, overall, potential, club, position |
| Match | id, datetime, home_goals, away_goals, competition, season, round, result |
| Competition | name, short_name, type |
| Season | year |

### Relationships
- `(Match)-[:HOME_TEAM]->(Team)` - Home team in a match
- `(Match)-[:AWAY_TEAM]->(Team)` - Away team in a match
- `(Match)-[:PART_OF]->(Competition)` - Match belongs to competition
- `(Match)-[:IN_SEASON]->(Season)` - Match in a season
- `(Player)-[:PLAYS_FOR]->(Team)` - Player's club

### Constraints & Indexes
- Unique constraint on Team(name)
- Unique constraint on Player(id)
- Unique constraint on Match(id)
- Unique constraint on Competition(name)
- Unique constraint on Season(year)

## Data Flow

### Initialization
1. Connect to Neo4j
2. Setup schema (constraints, indexes)
3. Load CSV files via DataLoader
4. Create nodes with deduplication
5. Create relationships

### Query Execution
1. MCP client calls tool with parameters
2. Server routes to QueryExecutor method
3. QueryExecutor builds Cypher query
4. Execute against Neo4j
5. Format and return results

## Design Patterns

### Connection Management
- Singleton pattern for Neo4j connection
- Context manager for session handling
- Lazy initialization

### Data Loading
- Batch loading with transaction management
- Team name normalization for deduplication
- Safe type conversion with NaN handling

### Query Building
- Cypher templates with parameter binding
- Result pagination support
- Flexible filtering options
