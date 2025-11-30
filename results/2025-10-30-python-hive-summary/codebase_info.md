# Codebase Information

## Project Overview

**Name:** Brazilian Soccer MCP Knowledge Graph (Hive Pattern)
**Primary Language:** Python 3.8+
**Purpose:** MCP server providing intelligent access to Brazilian soccer data through Neo4j graph database

## Technology Stack

| Category | Technology | Version |
|----------|------------|---------|
| Runtime | Python | 3.8+ |
| Graph Database | Neo4j | 5.14.0+ |
| MCP Framework | FastMCP | 0.2.0+ |
| Async Support | asyncio | 3.4.3+ |
| Data Validation | pydantic | 2.5.0+ |
| Testing | pytest-bdd | 7.4.3+ |

## Directory Structure

```
2025-10-30-python-hive/
├── src/                          # Source code
│   ├── __init__.py               # Package initialization
│   ├── server.py                 # Main MCP server (900+ lines)
│   ├── database.py               # Neo4j connection management
│   ├── models.py                 # Entity models (Player, Team, Match, etc.)
│   ├── config.py                 # Configuration and settings
│   └── tools/                    # MCP tool implementations
│       ├── player_tools.py       # Player query tools
│       ├── team_tools.py         # Team query tools
│       ├── match_tools.py        # Match query tools
│       ├── competition_tools.py  # Competition query tools
│       └── analysis_tools.py     # Analysis & rivalry tools
├── tests/                        # Test suite
│   ├── conftest.py               # Pytest fixtures (400+ lines)
│   ├── features/                 # Gherkin feature files
│   │   ├── player.feature        # 10 scenarios
│   │   ├── team.feature          # 12 scenarios
│   │   ├── match.feature         # 13 scenarios
│   │   ├── competition.feature   # 14 scenarios
│   │   └── analysis.feature      # 15 scenarios
│   ├── steps/                    # Step definitions
│   ├── test_player_tools.py
│   ├── test_team_tools.py
│   ├── test_match_tools.py
│   ├── test_competition_tools.py
│   ├── test_analysis_tools.py
│   └── test_integration.py       # E2E tests
├── docs/                         # Documentation
├── scripts/                      # Utility scripts
├── CLAUDE.md                     # Claude-Flow configuration
├── requirements.txt              # Dependencies
└── README.md                     # Project documentation
```

## Key Metrics

| Metric | Value |
|--------|-------|
| Total Python Files (src) | 22 |
| Lines of Code (src) | 7,090 |
| BDD Test Scenarios | 64 |
| Step Definitions | 175+ |
| Test Files | 6 |
| Feature Files | 5 |

## Build & Run

```bash
# Install dependencies
pip install -r requirements.txt

# Run MCP server
python -m src.server

# Run tests
pytest tests/ -v

# Run with coverage
pytest --cov=src --cov-report=html
```

## Database Configuration

- **URI:** `bolt://localhost:7687`
- **Default Credentials:** From `.env` file
- **Driver:** neo4j-driver 5.14.0+
