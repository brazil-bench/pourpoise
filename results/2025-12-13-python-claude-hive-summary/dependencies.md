# Dependencies: 2025-12-13-python-claude-hive

## Runtime Dependencies

### Python Standard Library Only
This implementation uses **zero external dependencies** for core functionality.

**Standard Library Modules Used:**
| Module | Purpose |
|--------|---------|
| `dataclasses` | Data model definitions |
| `datetime` | Date/time handling |
| `typing` | Type hints |
| `csv` | CSV file parsing |
| `pathlib` | File path operations |
| `logging` | Error logging |
| `collections` | defaultdict for indexes |
| `difflib` | Fuzzy string matching |
| `os` | Environment variables |
| `sys` | System operations |
| `unittest.mock` | Test mocking |

### Optional Dependencies

#### Neo4j Driver
```
neo4j>=5.0.0
```
Required only for Phase 3 graph database features.

#### Future Phases (Commented Out)
```python
# pandas>=2.0.0    # Advanced data analysis
# numpy>=1.24.0    # Statistical computations
# mcp>=0.1.0       # MCP server implementation
```

## Test Dependencies

### requirements-test.txt
```
pytest>=7.0.0
pytest-cov>=4.0.0
pytest-bdd>=6.0.0
```

## Development Dependencies

None explicitly defined. Uses Python 3.9+ with standard library.

## Data Dependencies

### Kaggle Datasets (Included)
| Dataset | License | Size |
|---------|---------|------|
| Brasileirao_Matches.csv | CC BY 4.0 | 289 KB |
| Brazilian_Cup_Matches.csv | CC BY 4.0 | 87 KB |
| Libertadores_Matches.csv | CC BY 4.0 | 94 KB |
| BR-Football-Dataset.csv | CC0 | 1.1 MB |
| novo_campeonato_brasileiro.csv | CC BY 4.0 | 575 KB |
| fifa_data.csv | Apache 2.0 | 8.9 MB |

## Infrastructure Dependencies

### Neo4j (Optional)
- Version: 4.x+ or 5.x
- Ports: 7474 (HTTP), 7687 (Bolt)
- Authentication: Required

### Docker (Optional)
For Neo4j deployment:
```bash
docker run -d --name neo4j \
  -p 7474:7474 -p 7687:7687 \
  -e NEO4J_AUTH=neo4j/password \
  neo4j:latest
```

## Dependency Graph

```
┌─────────────────────────────────────────────────┐
│               Application Code                   │
├─────────────────────────────────────────────────┤
│                                                  │
│   ┌─────────┐  ┌─────────┐  ┌─────────┐        │
│   │ models  │  │  data   │  │  query  │        │
│   │         │  │ loader  │  │ engine  │        │
│   └────┬────┘  └────┬────┘  └────┬────┘        │
│        │            │            │              │
│        └────────────┼────────────┘              │
│                     │                           │
│              Python Standard Library            │
│   ┌─────────────────┴─────────────────┐        │
│   │ dataclasses, datetime, csv, typing │        │
│   │ pathlib, logging, collections      │        │
│   └───────────────────────────────────┘        │
│                                                  │
├─────────────────────────────────────────────────┤
│           Optional: Neo4j Driver                 │
│                  neo4j>=5.0.0                    │
└─────────────────────────────────────────────────┘
```

## Version Requirements

| Requirement | Version |
|-------------|---------|
| Python | 3.9+ |
| Neo4j (optional) | 4.x+ |
| pytest (dev) | 7.0+ |

## Compatibility Notes

1. **Python 3.9+**: Uses `dataclasses` and modern typing features
2. **UTF-8**: All CSV files use UTF-8 encoding for Portuguese characters
3. **Cross-platform**: Works on Linux, macOS, Windows
4. **No compilation**: Pure Python, no C extensions required
