# Components: 2025-12-13-python-claude-hive

## Source Modules

### Phase 1: Data Layer

#### `src/models.py` (149 lines)
Core data structures for the domain model.

**Classes:**
- `Team` - Soccer team with name, state, aliases
- `Player` - FIFA player with ratings and attributes
- `Match` - Game result with teams, scores, metadata
- `Competition` - Tournament/league information

**Constants:**
- `BRASILEIRAO_SERIE_A`
- `COPA_DO_BRASIL`
- `COPA_LIBERTADORES`

#### `src/data_loader.py` (~456 lines)
CSV data loading for all 6 Kaggle datasets.

**Key Methods:**
- `load_brasileirao_matches()` - Brasileirão Série A
- `load_copa_do_brasil_matches()` - Copa do Brasil
- `load_libertadores_matches()` - Copa Libertadores
- `load_extended_matches()` - BR-Football-Dataset
- `load_historical_matches()` - 2003-2019 data
- `load_players()` - FIFA player database
- `load_all()` - Load all datasets

**Features:**
- UTF-8 encoding support
- Multiple date format parsing
- Error handling with logging

#### `src/team_normalizer.py` (~282 lines)
Team name normalization with 60+ mappings.

**Key Methods:**
- `normalize(team_name)` - Normalize to canonical name
- `get_aliases(team_name)` - Get all known aliases
- `is_same_team(name1, name2)` - Compare team names

**Mappings Include:**
- State suffixes: "Palmeiras-SP" → "Palmeiras"
- Full names: "Sport Club Corinthians Paulista" → "Corinthians"
- Common variations: "São Paulo FC" → "São Paulo"

### Phase 2: Query Engine

#### `src/query_engine.py` (~650 lines)
Main query engine with indexed lookups.

**Match Queries:**
- `find_matches_between(team1, team2)`
- `find_matches_by_team(team)`
- `find_matches_by_date_range(start, end)`
- `find_matches_by_competition(competition)`
- `find_matches_by_season(season)`

**Team Queries:**
- `get_team_statistics(team, season)`
- `get_head_to_head(team1, team2)`
- `get_team_home_record(team, season)`
- `get_top_teams_by_goals(season, limit)`

**Player Queries:**
- `find_players_by_name(name)`
- `find_players_by_nationality(nationality)`
- `find_players_by_club(club)`
- `get_top_rated_players(limit)`
- `get_brazilian_players_at_brazilian_clubs()`

**Statistical Queries:**
- `get_competition_standings(competition, season)`
- `get_biggest_wins(competition, limit)`
- `get_average_goals_per_match(competition, season)`

#### `src/statistics.py` (~221 lines)
Statistical dataclasses and calculations.

**Classes:**
- `TeamStats` - Team performance metrics
- `HeadToHeadStats` - H2H comparison data
- `Record` - Win/draw/loss record
- `Standing` - League table position
- `TeamGoalStats` - Goal statistics

### Phase 3: Neo4j Knowledge Graph

#### `src/neo4j_client.py` (~634 lines)
Full-featured Neo4j database client.

**Connection:**
- `connect()` / `close()`
- `verify_connectivity()`
- Context manager support

**Data Operations:**
- `create_team(team)`
- `create_player(player)`
- `create_match(match)`
- `batch_import_matches(matches)`

**Queries:**
- `find_matches_between_teams(team1, team2)`
- `find_shortest_path(team1, team2)`
- `get_team_network(team, depth)`
- `get_head_to_head(team1, team2)`

#### `src/graph_schema.py` (~335 lines)
Graph database schema definitions.

**Node Types (6):**
1. Team - name, state, founded
2. Player - name, position, rating
3. Match - date, scores, round
4. Competition - name, type
5. Season - year
6. Stadium - name, city, capacity

**Relationship Types (8):**
1. PLAYED_HOME - Team → Match
2. PLAYED_AWAY - Team → Match
3. PLAYS_FOR - Player → Team
4. COMPETED_IN - Team → Competition
5. IN_SEASON - Match → Season
6. HOSTED_AT - Match → Stadium
7. PART_OF - Match → Competition
8. TRANSFERRED_TO - Player → Team

#### `src/graph_queries.py` (~461 lines)
Cypher query templates.

**Query Categories:**
- Team queries
- Match queries
- Player queries
- Path finding
- Aggregations

### Configuration

#### `config/neo4j_config.py` (~309 lines)
Environment-based Neo4j configuration.

**Settings:**
- `uri` - Database URI (bolt://localhost:7687)
- `user` - Username
- `password` - Password
- `database` - Database name
- `max_connection_lifetime` - Connection pool settings

**Sources:**
1. Environment variables
2. .env file
3. Default values

## Test Modules

| Module | Tests | Purpose |
|--------|-------|---------|
| `conftest.py` | 10 fixtures | Shared test fixtures |
| `test_data_loader.py` | 13 | Data loading verification |
| `test_query_engine.py` | 14 | Query method testing |
| `test_neo4j_client.py` | 10 | Neo4j integration |
| `test_team_normalizer.py` | 13 | Name normalization |
| `test_integration.py` | 12 | End-to-end flows |

## Data Files

| File | Purpose | Records |
|------|---------|---------|
| Brasileirao_Matches.csv | Série A matches | 4,180 |
| Brazilian_Cup_Matches.csv | Copa do Brasil | 1,337 |
| Libertadores_Matches.csv | Libertadores | 1,255 |
| BR-Football-Dataset.csv | Extended stats | 10,296 |
| novo_campeonato_brasileiro.csv | Historical | 6,886 |
| fifa_data.csv | Player database | 18,207 |
