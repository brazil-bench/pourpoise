# Components - 2025-09-30-python-swarm

## MCP Tools Implemented

### Player Tools (6 tools)
| Tool | Description |
|------|-------------|
| `search_player` | Search players by name |
| `get_player_stats` | Get player statistics |
| `search_players_by_position` | Filter players by position |
| `get_player_career` | Get player career history |
| `compare_players` | Compare two players |
| `get_player_transfers` | Get player transfer history |

### Team Tools (6 tools)
| Tool | Description |
|------|-------------|
| `search_team` | Search teams by name |
| `get_team_stats` | Get team statistics |
| `get_team_roster` | Get team player roster |
| `search_teams_by_league` | Filter teams by league |
| `compare_teams` | Compare two teams |
| `get_team_history` | Get team history |

### Match Tools (3 tools)
| Tool | Description |
|------|-------------|
| `get_match_details` | Get match details |
| `search_matches_by_date` | Search matches by date range |
| `get_competition_info` | Get competition information |

### Analysis Tools
| Tool | Description |
|------|-------------|
| `get_head_to_head` | Head-to-head team comparison |
| `find_common_teammates` | Find players who played together |
| `get_rivalry_stats` | Rivalry statistics |

## Graph Entities

### Node Types
- **Player**: name, birth_date, nationality, position, jersey_number
- **Team**: name, city, stadium, founded_year
- **Match**: date, home_score, away_score, competition
- **Competition**: name, season, type, tier
- **Stadium**: name, city, capacity

### Relationship Types
- `PLAYS_FOR` (Player → Team)
- `SCORED_IN` (Player → Match)
- `COMPETED_IN` (Team → Match)
- `PART_OF` (Match → Competition)
- `PLAYED_AT` (Match → Stadium)

## Data Sources
- Kaggle Brazilian Football Matches dataset
- FIFA player data (supplementary)
- Synthetic test data for demos
