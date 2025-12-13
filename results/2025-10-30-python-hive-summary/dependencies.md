# Dependencies - 2025-10-30-python-hive

## Core Dependencies

| Package | Version | Purpose |
|---------|---------|---------|
| mcp | >=0.9.0 | MCP protocol |
| fastmcp | >=0.2.0 | FastMCP framework |
| neo4j | >=5.14.0 | Graph database driver |
| pydantic | >=2.5.0 | Data validation |
| pydantic-settings | >=2.1.0 | Settings management |
| python-dotenv | >=1.0.0 | Environment config |

## Async Support

| Package | Version | Purpose |
|---------|---------|---------|
| asyncio | >=3.4.3 | Async operations |
| aiofiles | >=23.2.1 | Async file I/O |

## Testing

| Package | Version | Purpose |
|---------|---------|---------|
| pytest | >=7.4.3 | Test framework |
| pytest-asyncio | >=0.21.1 | Async test support |
| pytest-cov | >=4.1.0 | Coverage reporting |

## Code Quality

| Package | Version | Purpose |
|---------|---------|---------|
| black | >=23.12.0 | Code formatting |
| ruff | >=0.1.8 | Linting |
| mypy | >=1.7.1 | Type checking |

## Utility

| Package | Version | Purpose |
|---------|---------|---------|
| python-json-logger | >=2.0.7 | JSON logging |
| typing-extensions | >=4.9.0 | Type hints |

## Total Dependencies: ~18 packages

## Comparison to Swarm Attempt

| Metric | Hive (this) | Swarm |
|--------|-------------|-------|
| Dependencies | 18 | 32 |
| LOC | 3,545 | 8,683 |
| Python files | 21 | 53 |

The Hive implementation is significantly leaner.
