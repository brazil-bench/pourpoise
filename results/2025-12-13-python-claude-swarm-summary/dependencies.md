# Dependencies - 2025-12-13-python-claude-swarm

## Core Dependencies (pyproject.toml)

| Package | Version | Purpose |
|---------|---------|---------|
| neo4j | >=5.14.0 | Graph database driver |
| mcp | >=0.9.0 | MCP protocol |
| pandas | >=2.0.0 | Data processing |
| pydantic | >=2.0.0 | Data validation |
| pydantic-settings | >=2.0.0 | Settings management |
| python-dotenv | >=1.0.0 | Environment config |
| unicodedata2 | >=15.0.0 | Unicode normalization |

## Dev Dependencies

| Package | Version | Purpose |
|---------|---------|---------|
| pytest | >=7.4.0 | Test framework |
| pytest-asyncio | >=0.21.0 | Async test support |
| pytest-cov | >=4.1.0 | Coverage reporting |
| pytest-bdd | >=6.1.0 | BDD testing |
| pytest-mock | >=3.11.0 | Mocking |
| faker | >=19.0.0 | Test data generation |
| black | >=23.0.0 | Code formatting |
| ruff | >=0.1.0 | Linting |
| mypy | >=1.5.0 | Type checking |
| isort | >=5.12.0 | Import sorting |

## Docs Dependencies

| Package | Version | Purpose |
|---------|---------|---------|
| sphinx | >=7.0.0 | Documentation |
| sphinx-rtd-theme | >=1.3.0 | Theme |
| sphinx-autodoc-typehints | >=1.24.0 | Type hints |

## Build System

- **Build backend**: setuptools
- **Python version**: >=3.9

## Total Dependencies: 7 core + 10 dev + 3 docs = 20 packages

## Comparison to Other Attempts

| Metric | This (Swarm v2) | Beads | Hive Mind | Swarm v1 |
|--------|-----------------|-------|-----------|----------|
| Core deps | 7 | 6 | ~12 | ~20 |
| LOC | 4,227 | 1,826 | 3,545 | 8,683 |
| Python files | 17 | 14 | 21 | 53 |
