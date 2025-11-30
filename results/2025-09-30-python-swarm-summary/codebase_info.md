# Codebase Information

## Project Overview

**Name:** Brazilian Soccer MCP Knowledge Graph
**Primary Language:** Python 3.8+
**Purpose:** Model Context Protocol (MCP) server providing intelligent access to Brazilian soccer data through a Neo4j graph database

## Technology Stack

| Category | Technology | Version |
|----------|------------|---------|
| Runtime | Python | 3.8+ |
| Graph Database | Neo4j | 5.15.0 |
| MCP Framework | mcp | 1.0.0 |
| Web Framework | FastAPI | 0.104.1 |
| Data Processing | pandas | 2.1.4 |
| Testing | pytest-bdd | 7.0.0 |
| CLI | click | 8.1.7 |

## Directory Structure

```
reviews/2025-09-30-python-swarm/
├── src/                          # Source code
│   ├── graph/                    # Neo4j integration layer
│   │   ├── database.py           # Connection management & queries
│   │   ├── models.py             # Entity models & graph schema
│   │   ├── queries.py            # Cypher query builders
│   │   └── schema.py             # Schema definitions
│   ├── data_pipeline/            # Data processing
│   │   ├── kaggle_loader.py      # CSV data loading & parsing
│   │   └── graph_builder.py      # Graph population logic
│   ├── mcp_server/               # MCP server implementation
│   │   ├── server.py             # Main MCP server
│   │   ├── config.py             # Server configuration
│   │   ├── http_server.py        # HTTP transport layer
│   │   └── tools/                # MCP tool implementations
│   │       ├── player_tools.py   # Player query tools
│   │       ├── team_tools.py     # Team query tools
│   │       ├── match_tools.py    # Match query tools
│   │       └── analysis_tools.py # Analysis & rivalry tools
│   └── utils/                    # Shared utilities
│       └── data_utils.py         # Data transformation helpers
├── tests/                        # Test suite
│   ├── features/                 # Gherkin feature files
│   ├── step_defs/                # BDD step implementations
│   ├── e2e/                      # End-to-end tests
│   └── integration/              # Integration tests
├── config/                       # Configuration files
│   └── config.yaml               # Application configuration
├── data/                         # Data storage
│   └── kaggle/                   # Kaggle datasets (CSV)
├── docs/                         # Documentation
├── scripts/                      # Utility scripts
└── main.py                       # CLI entry point
```

## Key Metrics

| Metric | Value |
|--------|-------|
| Python Files | ~30 |
| MCP Tools | 13 |
| Graph Entity Types | 10 |
| Relationship Types | 15+ |
| BDD Test Scenarios | 39+ |

## Database Configuration

- **URI:** `bolt://localhost:7687`
- **Default Credentials:** `neo4j` / `neo4j123`
- **Connection Pooling:** 50 max connections
- **Query Cache TTL:** 30 minutes

## Build & Run

```bash
# Install dependencies
pip install -r requirements.txt

# Initialize database schema
python main.py schema

# Build graph from data
python main.py build --clear-first

# Run MCP server
python src/run_mcp_server.py

# Run tests
pytest tests/ -v
```
