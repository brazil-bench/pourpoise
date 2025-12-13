# Dependencies - 2025-12-01-python-claude-beads

## Core Dependencies (pyproject.toml)

| Package | Version | Purpose |
|---------|---------|---------|
| mcp | >=1.0.0 | MCP protocol |
| neo4j | >=5.0.0 | Graph database |
| pandas | >=2.0.0 | Data processing |
| pytest | >=8.0.0 | Testing |
| pytest-bdd | >=7.0.0 | BDD testing |
| pytest-asyncio | >=0.23.0 | Async test support |

## Dev Dependencies

| Package | Version | Purpose |
|---------|---------|---------|
| ruff | >=0.1.0 | Linting |
| mypy | >=1.0.0 | Type checking |

## Total Dependencies: 8 packages

## Build System

- **Build backend**: hatchling
- **Python version**: >=3.12

## Comparison to Other Attempts

| Metric | Beads | Hive Mind | Swarm |
|--------|-------|-----------|-------|
| Dependencies | 8 | 18 | 32 |
| LOC | 1,826 | 3,545 | 8,683 |
| Python files | 14 | 21 | 53 |

**Beads is the leanest implementation by far.**
