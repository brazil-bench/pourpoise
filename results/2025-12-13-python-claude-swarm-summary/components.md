# Components - 2025-12-13-python-claude-swarm

## MCP Tools Implemented (6 total)

| Tool | Description |
|------|-------------|
| `search_matches` | Find matches by team, date range, competition, season |
| `get_team_info` | Get team details and statistics |
| `search_players` | Search players by name, club, position, rating |
| `get_head_to_head` | Head-to-head statistics between teams |
| `get_standings` | Competition standings/league tables |
| `get_statistics` | Statistical analysis and aggregations |

## Query Handlers

| Handler | Responsibility |
|---------|----------------|
| `MatchQueryHandler` | Match search and filtering |
| `TeamQueryHandler` | Team information queries |
| `PlayerQueryHandler` | Player search and stats |
| `CompetitionQueryHandler` | Competition data |
| `StatisticsQueryHandler` | Statistical analysis |

## Data Models (Pydantic)

### Competition
```python
name: str          # Competition name
type: str          # "league" or "cup"
```

### Team
```python
name: str              # Full team name
state: str             # State abbreviation
normalized_name: str   # Normalized name
stadium: str           # Home stadium
city: str              # City
founded: int           # Year founded
colors: str            # Team colors
```

### Match
```python
id: str                    # Unique identifier
date: datetime             # Match date/time
home_team_normalized: str  # Home team
away_team_normalized: str  # Away team
home_score: int            # Home goals
away_score: int            # Away goals
season: int                # Season year
round: str                 # Round/gameweek
competition: str           # Competition name
venue: str                 # Stadium
attendance: int            # Attendance
referee: str               # Referee name
```

### Player
```python
name: str           # Player name
nationality: str    # Nationality
age: int            # Age
club: str           # Current club
overall: int        # FIFA overall rating
potential: int      # FIFA potential rating
position: str       # Playing position
preferred_foot: str # Left/Right
```

## BDD Test Scenarios (38 total)

| Feature | Scenarios |
|---------|-----------|
| Players | 15+ |
| Teams | 10+ |
| Matches | 13+ |

## Data Sources (Real Kaggle Data)

| File | Size | Records |
|------|------|---------|
| fifa_data.csv | 9.1 MB | 18,000+ players |
| BR-Football-Dataset.csv | 1.1 MB | 10,000+ matches |
| novo_campeonato_brasileiro.csv | 588 KB | 6,000+ matches |
| Brasileirao_Matches.csv | 295 KB | 4,000+ matches |
| Libertadores_Matches.csv | 96 KB | 1,200+ matches |
| Brazilian_Cup_Matches.csv | 89 KB | 1,300+ matches |
| **Total** | **~11 MB** | **40,000+ records** |
