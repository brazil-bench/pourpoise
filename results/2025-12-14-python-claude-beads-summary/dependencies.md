# Dependencies

## Runtime Dependencies

| Package | Version | Purpose |
|---------|---------|---------|
| mcp | >=1.0.0 | Model Context Protocol server framework |
| neo4j | >=5.0.0 | Neo4j Python driver for graph database |
| pandas | >=2.0.0 | CSV data loading and processing |
| python-dateutil | >=2.8.0 | Date parsing with multiple format support |
| unidecode | >=1.3.0 | Unicode text transliteration (team names) |

## Development Dependencies

| Package | Version | Purpose |
|---------|---------|---------|
| pytest | >=7.0.0 | Test framework |
| pytest-bdd | >=7.0.0 | BDD testing with Gherkin feature files |
| pytest-asyncio | >=0.21.0 | Async test support |

## External Services

### Neo4j Database
- **Version:** 5.x
- **Ports:** 7474 (HTTP), 7687 (Bolt)
- **Auth:** Default neo4j/password123
- **Docker:** `neo4j:latest`

## Data Files

| File | Records | Size | Source |
|------|---------|------|--------|
| Brasileirao_Matches.csv | 4,180 | 289 KB | Kaggle CC BY 4.0 |
| Brazilian_Cup_Matches.csv | 1,337 | 87 KB | Kaggle CC BY 4.0 |
| Libertadores_Matches.csv | 1,255 | 94 KB | Kaggle CC BY 4.0 |
| BR-Football-Dataset.csv | 10,296 | 1.1 MB | Kaggle CC0 |
| novo_campeonato_brasileiro.csv | 6,886 | 575 KB | Kaggle CC BY 4.0 |
| fifa_data.csv | 18,207 | 8.9 MB | Kaggle Apache 2.0 |
| **Total** | **~42,161** | **~11 MB** | |

## Python Version
- Requires Python 3.10+
- Uses dataclasses, typing, asyncio features
