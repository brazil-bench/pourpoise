# Brazilian Soccer MCP Server - Codebase Summary

## Overview
This is a Python implementation of an MCP (Model Context Protocol) server for Brazilian soccer data using Neo4j as the knowledge graph backend. The implementation follows the Beads orchestration pattern with issue-tracking based sequential task management.

## Key Statistics
- **Total Lines of Code:** 3,511
- **Source Code:** 2,346 lines (5 files)
- **Test Code:** 1,165 lines (6 files)
- **Dependencies:** 5 runtime + 3 dev
- **Test Scenarios:** 18 BDD scenarios

## Documentation Index
- [Architecture](architecture.md) - System design and component overview
- [Components](components.md) - Detailed component documentation
- [Dependencies](dependencies.md) - External dependencies and their purposes

## Technology Stack
- **Language:** Python 3.10+
- **Database:** Neo4j 5.0+
- **Protocol:** MCP (Model Context Protocol)
- **Testing:** pytest + pytest-bdd
- **Data Processing:** pandas

## Data Coverage
- **Teams:** 666 unique teams
- **Players:** 18,207 FIFA players
- **Matches:** 23,954 across 3 competitions
- **Total Records:** ~42,161
