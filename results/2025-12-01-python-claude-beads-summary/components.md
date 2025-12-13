# Components - 2025-12-01-python-claude-beads

## MCP Tools Implemented (12 total)

### Player Tools (3)
| Tool | Description |
|------|-------------|
| `search_player` | Search players by name, team, or position |
| `get_player_stats` | Player statistics with optional season filter |
| `get_player_career` | Career history across teams |

### Team Tools (3)
| Tool | Description |
|------|-------------|
| `search_team` | Search teams by name |
| `get_team_roster` | Team roster by season |
| `get_team_stats` | Team statistics |

### Match Tools (3)
| Tool | Description |
|------|-------------|
| `get_match_details` | Match information |
| `search_matches` | Filter matches by criteria |
| `get_head_to_head` | Head-to-head statistics |

### Analysis Tools (3)
| Tool | Description |
|------|-------------|
| `get_competition_top_scorers` | Top scorers in competition |
| `find_common_teammates` | Players who played together |
| `find_players_who_played_for_both_teams` | Players who played for rival teams |

## Data Models (7 dataclasses)

| Model | Fields |
|-------|--------|
| **Player** | player_id, name, birth_date, nationality, position, jersey_number |
| **Team** | team_id, name, city, stadium, founded_year, colors |
| **Match** | match_id, date, home_team_id, away_team_id, scores, competition_id |
| **Competition** | competition_id, name, season, type, tier |
| **Stadium** | stadium_id, name, city, capacity, opened_year |
| **Coach** | coach_id, name, nationality, birth_date |
| **Goal** | player_id, match_id, minute, goal_type |
| **PlayerContract** | player_id, team_id, start_date, end_date |

## BDD Test Scenarios (18 total)

| Feature | Scenarios |
|---------|-----------|
| Player Search | 5 |
| Team Operations | 4 |
| Match Queries | 5 |
| Analysis | 4 |

## Graph Schema

### Relationships
- `PLAYS_FOR` (Player → Team)
- `SCORED_IN` (Player → Match)
- `COMPETED_IN` (Team → Match)
- `PART_OF` (Match → Competition)
- `MANAGES` (Coach → Team)
