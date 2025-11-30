# Interfaces and APIs

## MCP Tool Interface

The server exposes 13 tools through the Model Context Protocol.

### Player Tools

#### `search_player`
Search for players by name or partial name.

**Input Schema:**
```json
{
  "name": "string (required)",
  "limit": "integer (default: 10)"
}
```

**Example:** `search_player(name="Neymar", limit=5)`

---

#### `get_player_stats`
Get detailed statistics for a specific player.

**Input Schema:**
```json
{
  "player_name": "string (required)",
  "season": "string (optional)"
}
```

---

#### `get_player_career`
Get career history and teams for a player.

**Input Schema:**
```json
{
  "player_name": "string (required)"
}
```

---

### Team Tools

#### `search_team`
Search for teams by name or partial name.

**Input Schema:**
```json
{
  "name": "string (required)",
  "limit": "integer (default: 10)"
}
```

---

#### `get_team_roster`
Get current roster for a team.

**Input Schema:**
```json
{
  "team_name": "string (required)",
  "season": "string (optional)"
}
```

---

#### `get_team_stats`
Get statistics and performance data for a team.

**Input Schema:**
```json
{
  "team_name": "string (required)",
  "competition": "string (optional)"
}
```

---

### Match Tools

#### `get_match_details`
Get detailed information about a specific match.

**Input Schema:**
```json
{
  "match_id": "string (optional)",
  "team1": "string (optional)",
  "team2": "string (optional)",
  "date": "string YYYY-MM-DD (optional)"
}
```

---

#### `search_matches`
Search for matches by teams, date range, or competition.

**Input Schema:**
```json
{
  "team": "string (optional)",
  "start_date": "string YYYY-MM-DD (optional)",
  "end_date": "string YYYY-MM-DD (optional)",
  "competition": "string (optional)",
  "limit": "integer (default: 20)"
}
```

---

#### `get_head_to_head`
Get head-to-head statistics between two teams.

**Input Schema:**
```json
{
  "team1": "string (required)",
  "team2": "string (required)",
  "competition": "string (optional)"
}
```

---

### Competition Tools

#### `get_competition_standings`
Get current standings for a competition.

**Input Schema:**
```json
{
  "competition": "string (required)",
  "season": "string (optional)"
}
```

---

#### `get_competition_top_scorers`
Get top scorers for a competition.

**Input Schema:**
```json
{
  "competition": "string (required)",
  "season": "string (optional)",
  "limit": "integer (default: 10)"
}
```

---

### Analysis Tools

#### `find_common_teammates`
Find players who were teammates with specific players.

**Input Schema:**
```json
{
  "players": ["string array (required)"],
  "team": "string (optional)"
}
```

---

#### `get_rivalry_stats`
Get detailed rivalry statistics and history.

**Input Schema:**
```json
{
  "team1": "string (required)",
  "team2": "string (required)",
  "years": "integer (default: 10)"
}
```

---

## MCP Resources

### `brazilian-soccer://help`
Complete guide for using the Brazilian Soccer Knowledge Graph.

**MIME Type:** `text/plain`

### `brazilian-soccer://schema`
Neo4j graph database schema documentation.

**MIME Type:** `application/json`

---

## Database Interface

### Neo4jDatabase Class

```python
class Neo4jDatabase:
    def __init__(self, uri, user, password, ...): ...
    def connect(self) -> None: ...
    def execute_query(self, query, parameters) -> List[Dict]: ...
    def execute_read_query(self, query, parameters) -> List[Dict]: ...
    def execute_write_query(self, query, parameters) -> List[Dict]: ...
    def execute_transaction(self, queries) -> List[List[Dict]]: ...
    def get_schema_info(self) -> Dict[str, Any]: ...
    def test_connection(self) -> Dict[str, Any]: ...
    def close(self) -> None: ...
```

---

## CLI Interface

```bash
# Command structure
python main.py [OPTIONS] COMMAND [ARGS]...

# Global options
--env TEXT       Environment (development, testing, production)
--log-level TEXT Logging level

# Commands
build            Build the complete graph database
stats            Show graph database statistics
validate         Validate graph integrity
load-data        Load data from specified source
clear            Clear all data from the database
test-connection  Test database connection
schema           Manage database schema
```

---

## Response Formats

All tool responses are JSON-formatted:

```json
{
  "status": "success|error",
  "data": { ... },
  "error": "error message (if applicable)"
}
```
