# Components - 2025-10-30-python-hive

## MCP Tools Implemented (15 total)

### Player Tools
| Tool | Description |
|------|-------------|
| `search_player` | Find players by name/team/position |
| `get_player_stats` | Player statistics |
| `get_player_career` | Career history across teams |
| `get_player_transfers` | Transfer history |

### Team Tools
| Tool | Description |
|------|-------------|
| `search_team` | Find teams by name |
| `get_team_roster` | Team rosters by season |
| `get_team_stats` | Team statistics |
| `get_team_history` | Historical team data |

### Match Tools
| Tool | Description |
|------|-------------|
| `get_match_details` | Match information |
| `search_matches` | Filter matches by criteria |
| `get_head_to_head` | Team rivalry statistics |
| `get_match_scorers` | Goal scorers in a match |

### Competition Tools
| Tool | Description |
|------|-------------|
| `get_competition_standings` | League tables |
| `get_competition_top_scorers` | Top scorers by competition |
| `get_competition_matches` | Matches in a competition |

### Analysis Tools
| Tool | Description |
|------|-------------|
| `find_common_teammates` | Players who played together |
| `get_rivalry_stats` | Rivalry statistics |
| `find_players_by_career_path` | Career trajectory analysis |

## Graph Schema

### Node Types (6)
- **Player**: player_id, name, birth_date, nationality, position, jersey_number
- **Team**: team_id, name, city, stadium, founded_year
- **Match**: match_id, date, home_score, away_score, competition
- **Competition**: competition_id, name, season, type, tier
- **Stadium**: stadium_id, name, city, capacity
- **Coach**: coach_id, name, nationality, birth_date

### Relationship Types (9)
- `PLAYS_FOR` (Player → Team)
- `SCORED_IN` (Player → Match)
- `ASSISTED_IN` (Player → Match)
- `COMPETED_IN` (Team → Match)
- `PART_OF` (Match → Competition)
- `PLAYED_AT` (Match → Stadium)
- `TRANSFERRED_FROM` (Player → Team)
- `TRANSFERRED_TO` (Player → Team)
- `MANAGES` (Coach → Team)

## BDD Test Scenarios (64 total)

| Feature | Scenarios |
|---------|-----------|
| Player | 10+ |
| Team | 10+ |
| Match | 10+ |
| Competition | 10+ |
| Analysis | 10+ |
