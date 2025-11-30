# Interfaces and APIs

## MCP Tool Interface

The server exposes 18 tools through the Model Context Protocol.

### Player Tools (4)

#### `search_player`
Search for players by name, team, or position.

**Parameters:**
| Name | Type | Required | Description |
|------|------|----------|-------------|
| name | string | Yes | Player name (partial match) |
| team | string | No | Filter by team |
| position | string | No | Filter by position |

---

#### `get_player_stats`
Get detailed statistics for a specific player.

**Parameters:**
| Name | Type | Required | Description |
|------|------|----------|-------------|
| player_id | string | Yes | Unique player identifier |
| season | string | No | Filter by season |

---

#### `get_player_career`
Get career history and teams for a player.

**Parameters:**
| Name | Type | Required | Description |
|------|------|----------|-------------|
| player_id | string | Yes | Unique player identifier |

---

#### `get_player_transfers`
Get transfer history for a player.

**Parameters:**
| Name | Type | Required | Description |
|------|------|----------|-------------|
| player_id | string | Yes | Unique player identifier |

---

### Team Tools (4)

#### `search_team`
Search for teams by name.

**Parameters:**
| Name | Type | Required | Description |
|------|------|----------|-------------|
| name | string | Yes | Team name (partial match) |

---

#### `get_team_roster`
Get current roster for a team.

**Parameters:**
| Name | Type | Required | Description |
|------|------|----------|-------------|
| team_id | string | Yes | Unique team identifier |
| season | string | No | Filter by season |

---

#### `get_team_stats`
Get statistics for a team.

**Parameters:**
| Name | Type | Required | Description |
|------|------|----------|-------------|
| team_id | string | Yes | Unique team identifier |
| season | string | No | Filter by season |

---

#### `get_team_history`
Get historical information for a team.

**Parameters:**
| Name | Type | Required | Description |
|------|------|----------|-------------|
| team_id | string | Yes | Unique team identifier |

---

### Match Tools (4)

#### `get_match_details`
Get detailed information about a specific match.

**Parameters:**
| Name | Type | Required | Description |
|------|------|----------|-------------|
| match_id | string | Yes | Unique match identifier |

---

#### `search_matches`
Search for matches by criteria.

**Parameters:**
| Name | Type | Required | Description |
|------|------|----------|-------------|
| team | string | No | Filter by team |
| date_from | string | No | Start date (YYYY-MM-DD) |
| date_to | string | No | End date (YYYY-MM-DD) |

---

#### `get_head_to_head`
Get head-to-head statistics between two teams.

**Parameters:**
| Name | Type | Required | Description |
|------|------|----------|-------------|
| team1_id | string | Yes | First team identifier |
| team2_id | string | Yes | Second team identifier |

---

#### `get_match_scorers`
Get goal scorers for a match.

**Parameters:**
| Name | Type | Required | Description |
|------|------|----------|-------------|
| match_id | string | Yes | Unique match identifier |

---

### Competition Tools (3)

#### `get_competition_standings`
Get current standings for a competition.

**Parameters:**
| Name | Type | Required | Description |
|------|------|----------|-------------|
| competition_id | string | Yes | Competition identifier |
| season | string | Yes | Season identifier |

---

#### `get_competition_top_scorers`
Get top scorers for a competition.

**Parameters:**
| Name | Type | Required | Description |
|------|------|----------|-------------|
| competition_id | string | Yes | Competition identifier |
| season | string | Yes | Season identifier |

---

#### `get_competition_matches`
Get matches for a competition.

**Parameters:**
| Name | Type | Required | Description |
|------|------|----------|-------------|
| competition_id | string | Yes | Competition identifier |
| season | string | Yes | Season identifier |

---

### Analysis Tools (3)

#### `find_common_teammates`
Find players who were teammates.

**Parameters:**
| Name | Type | Required | Description |
|------|------|----------|-------------|
| player1_id | string | Yes | First player identifier |
| player2_id | string | Yes | Second player identifier |

---

#### `get_rivalry_stats`
Get detailed rivalry statistics.

**Parameters:**
| Name | Type | Required | Description |
|------|------|----------|-------------|
| team1_id | string | Yes | First team identifier |
| team2_id | string | Yes | Second team identifier |

---

#### `find_players_by_career_path`
Find players matching career criteria.

**Parameters:**
| Name | Type | Required | Description |
|------|------|----------|-------------|
| criteria | object | Yes | Search criteria object |

---

## Response Format

All tools return `List[TextContent]`:

```python
[TextContent(type="text", text="...formatted response...")]
```

## Database Interface

### Neo4jConnection Class

```python
class Neo4jConnection:
    async def connect(self) -> None
    async def close(self) -> None
    async def execute_query(
        self,
        query: str,
        parameters: Dict[str, Any] = None
    ) -> List[Dict[str, Any]]
    async def health_check(self) -> Dict[str, Any]
```
