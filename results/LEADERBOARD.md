# Brazil-Bench Leaderboard

## Evaluated Attempts

| Rank | Attempt | Pattern | Duration | LOC | Tests | Compliance | Fix Commits |
|------|---------|---------|----------|-----|-------|------------|-------------|
| 🥇 | 2025-12-01-python-claude-beads | Beads v1 | **~11 min** | **1,826** | 18 BDD | 12/16 | **0** |
| 🥈 | 2025-12-14-python-claude-beads | Beads v2 | **~14 min** | 3,511 | 18 BDD | **14/16** | **0** |
| 🥉 | 2025-12-13-python-claude-hive | Hive Mind v2 | ~37 min | 6,165 | 63 BDD | 13/16 | **0** |
| 4 | 2025-10-30-python-hive | Hive Mind v1 | ~41 min | 3,545 | **64 BDD** | **15/16** | 1 |
| 5 | 2025-09-30-python-swarm | Swarm v1 | ~1h 49m | 8,683 | 15 E2E | 14/16 | 7 |
| 6 | 2025-12-13-python-claude-swarm | Swarm v2 | ~1h 54m | 4,227 | 38 BDD | 10/16 | **0** |

## Detailed Comparison

### Efficiency Metrics

| Metric | Beads v1 | Beads v2 | Hive v2 | Hive v1 | Swarm v1 | Swarm v2 | Best |
|--------|----------|----------|---------|---------|----------|----------|------|
| **Duration** | ~11 min | ~14 min | ~37 min | ~41 min | ~1h 49m | ~1h 54m | Beads v1 |
| **Lines of Code** | 1,826 | 3,511 | 6,165 | 3,545 | 8,683 | 4,227 | Beads v1 |
| **Python Files** | 14 | 11 | 18 | 21 | 53 | 17 | Beads v2 |
| **Dependencies** | 8 | 5 | 0 | 18 | 32 | 20 | Hive v2 |
| **Total Commits** | 7 | 4 | 5 | 7 | 32 | 2 | Swarm v2 |
| **Fix Commits** | 0 | 0 | 0 | 1 | 7 | 0 | Multiple |

### Quality Metrics

| Metric | Beads v1 | Beads v2 | Hive v2 | Hive v1 | Swarm v1 | Swarm v2 | Best |
|--------|----------|----------|---------|---------|----------|----------|------|
| **Spec Compliance** | 12/16 | 14/16 | 13/16 | 15/16 | 14/16 | 10/16 | Hive v1 |
| **BDD Scenarios** | 18 | 18 | 63 | 64 | 15 E2E | 38 | Hive v1 |
| **MCP Tools** | Yes | 10 tools | 0 | ? | ? | ? | Beads v2 |
| **Real Kaggle Data** | Yes | Yes | Yes | No | Yes | Yes | 5/6 attempts |
| **Clean Implementation** | Yes | Yes | Yes | Yes | No | Yes | 5/6 attempts |

### Pattern Characteristics

| Pattern | Orchestration | Agents | Key Feature |
|---------|---------------|--------|-------------|
| **Beads v1** | Issue tracking | Single | Fast sequential execution |
| **Beads v2** | Issue tracking (bd CLI) | Single | Full MCP with 10 tools |
| **Hive Mind v2** | Claude-Flow Queen | 5 parallel | Researcher, 3 Coders, Tester |
| **Hive Mind v1** | Claude Flow | 4 parallel | Researcher, Coder, Tester, Analyst |
| **Swarm v1** | Claude Flow | 64 available | Dynamic coordination |
| **Swarm v2** | Claude Code | Single | Query handler pattern |

## Beads Comparison (v1 vs v2)

| Metric | Beads v1 (Dec 1) | Beads v2 (Dec 14) | Winner |
|--------|------------------|-------------------|--------|
| Duration | ~11 min | ~14 min | v1 (27% faster) |
| LOC | 1,826 | 3,511 | v1 (48% less code) |
| Tests | 18 BDD | 18 BDD | Tie |
| Compliance | 12/16 | 14/16 | **v2 (higher)** |
| Fix Commits | 0 | 0 | Tie |
| Real Data | Yes | Yes (11 MB) | Tie |
| MCP Tools | Yes | 10 fully exposed | **v2** |
| Dependencies | 8 | 5 | v2 (fewer) |

**Key Differences:**
- v2 uses `bd` CLI for issue tracking with dependency chains
- v2 has more comprehensive MCP tool implementation (10 tools)
- v2 achieved higher spec compliance (88% vs 75%)
- v2 has slightly larger codebase but better feature coverage
- v2 self-corrected during development (fixed NaN handling)

## Efficiency Rankings

### Speed (Autonomous Duration)
1. 🥇 **Beads v1**: ~11 min
2. 🥈 **Beads v2**: ~14 min (1.3x slower)
3. 🥉 **Hive Mind v2**: ~37 min (3.4x slower)
4. 4th **Hive Mind v1**: ~41 min (3.7x slower)
5. 5th **Swarm v1**: ~1h 49m (10x slower)
6. 6th **Swarm v2**: ~1h 54m (10.4x slower)

### Code Efficiency (LOC)
1. 🥇 **Beads v1**: 1,826 lines
2. 🥈 **Beads v2**: 3,511 lines (1.9x more)
3. 🥉 **Hive Mind v1**: 3,545 lines (1.9x more)
4. 4th **Swarm v2**: 4,227 lines (2.3x more)
5. 5th **Hive Mind v2**: 6,165 lines (3.4x more)
6. 6th **Swarm v1**: 8,683 lines (4.8x more)

### Implementation Quality (Fix Commits)
1. 🥇 **Beads v1**: 0 fix commits
1. 🥇 **Beads v2**: 0 fix commits
1. 🥇 **Hive Mind v2**: 0 fix commits
1. 🥇 **Swarm v2**: 0 fix commits
5. 5th **Hive Mind v1**: 1 fix commit
6. 6th **Swarm v1**: 7 fix commits

## Completeness Rankings

### Spec Compliance
1. 🥇 **Hive Mind v1**: 15/16 requirements (94%)
2. 🥈 **Beads v2**: 14/16 requirements (88%)
2. 🥈 **Swarm v1**: 14/16 requirements (88%)
4. 4th **Hive Mind v2**: 13/16 requirements (81%)
5. 5th **Beads v1**: 12/16 requirements (75%)
6. 6th **Swarm v2**: 10/16 requirements (63%)

### Test Coverage
1. 🥇 **Hive Mind v1**: 64 BDD scenarios
2. 🥈 **Hive Mind v2**: 63 BDD scenarios
3. 🥉 **Swarm v2**: 38 BDD scenarios
4. 4th **Beads v1**: 18 BDD scenarios
4. 4th **Beads v2**: 18 BDD scenarios
6. 6th **Swarm v1**: 15 E2E tests

## Data Strategy Comparison

| Attempt | Data Strategy | Data Size | Coverage |
|---------|---------------|-----------|----------|
| **Swarm v1** | Real Kaggle | ~96 MB | Full datasets |
| **Swarm v2** | Real Kaggle | ~11 MB | 40,000+ records |
| **Hive Mind v2** | Real Kaggle | ~11 MB | 42,000+ records |
| **Beads v1** | Real Kaggle | ~11 MB | Core datasets |
| **Beads v2** | Real Kaggle | ~11 MB | 42,161 records |
| **Hive Mind v1** | Simulated | N/A | Test data only |

## Trade-off Analysis

### Beads v1 (Fastest)
**Pros:**
- Fastest implementation (11 min)
- Leanest codebase (1,826 LOC)
- Zero rework needed
- Real Kaggle data

**Cons:**
- Fewer tests (18)
- Lower spec compliance (75%)

**Best for:** Quick prototypes, demos, time-constrained benchmarks

### Beads v2 (Best Balance)
**Pros:**
- Very fast execution (~14 min)
- High spec compliance (88%)
- Full MCP implementation (10 tools)
- Self-correcting during development
- Real Kaggle data included

**Cons:**
- More code than v1 (3,511 vs 1,826)
- Same test count as v1

**Best for:** Production-ready MCP servers with full feature coverage

### Hive Mind v2 (Balanced + Parallel)
**Pros:**
- Fast execution (~37 min)
- Zero fix commits
- Real Kaggle data included
- High test coverage (63 BDD)

**Cons:**
- More code (6,165)
- High token usage (~345k)

**Best for:** Real data integration with parallel agents

### Hive Mind v1 (Most Complete)
**Pros:**
- Highest spec compliance (94%)
- Best test coverage (64 BDD)
- Good efficiency balance

**Cons:**
- No real data integration
- One fix commit needed

**Best for:** Production-quality implementations, comprehensive coverage

### Swarm v1 (Most Infrastructure)
**Pros:**
- Good spec compliance (88%)
- Real Kaggle data (96 MB)

**Cons:**
- Most code (8,683 LOC)
- Most rework (7 fix commits)

**Best for:** Learning swarm patterns, extensive tooling needs

### Swarm v2 (Cleanest Swarm)
**Pros:**
- Zero fix commits
- Real Kaggle data included

**Cons:**
- Lowest spec compliance (63%)
- Slowest implementation

**Best for:** When clean implementation is priority over features

## Key Insights

1. **Beads dominates speed**: Both v1 (~11 min) and v2 (~14 min) are 3-10x faster than multi-agent patterns.

2. **Beads v2 improves compliance**: v2 achieved 88% compliance vs v1's 75% with full MCP tool exposure.

3. **Hive Mind patterns excel at testing**: Both Hive v1 (64 tests) and v2 (63 tests) lead in test coverage.

4. **Real data vs compliance trade-off persists**: Hive Mind v1 (simulated data) has highest compliance.

5. **Zero-dependency implementation possible**: Hive Mind v2 uses only Python stdlib.

6. **Clean implementations are common**: 4/6 attempts (67%) had zero fix commits.

7. **MCP tool count varies**: Beads v2 has the most exposed MCP tools (10).

## Visualizations

### Duration Comparison
```
Beads v1:   ██ (~11 min)
Beads v2:   ███ (~14 min)
Hive v2:    ███████ (~37 min)
Hive v1:    ████████ (~41 min)
Swarm v1:   █████████████████████ (~109 min)
Swarm v2:   ██████████████████████ (~114 min)
```

### Lines of Code
```
Beads v1:   ██ (1,826)
Beads v2:   ████ (3,511)
Hive v1:    ████ (3,545)
Swarm v2:   █████ (4,227)
Hive v2:    ███████ (6,165)
Swarm v1:   █████████ (8,683)
```

### Spec Compliance
```
Hive v1:    ███████████████ (15/16 = 94%)
Beads v2:   ██████████████ (14/16 = 88%)
Swarm v1:   ██████████████ (14/16 = 88%)
Hive v2:    █████████████ (13/16 = 81%)
Beads v1:   ████████████ (12/16 = 75%)
Swarm v2:   ██████████ (10/16 = 63%)
```

### Fix Commits (Lower is Better)
```
Beads v1:   (0)
Beads v2:   (0)
Hive v2:    (0)
Swarm v2:   (0)
Hive v1:    █ (1)
Swarm v1:   ███████ (7)
```

### Test Count
```
Hive v1:    ████████████████████████████████ (64)
Hive v2:    ███████████████████████████████ (63)
Swarm v2:   ███████████████████ (38)
Beads v1:   █████████ (18)
Beads v2:   █████████ (18)
Swarm v1:   ███████ (15)
```

## Recommendations

| Scenario | Recommended Pattern |
|----------|---------------------|
| Quick prototype | **Beads v1** |
| Production MCP server | **Beads v2** |
| Production quality | **Hive Mind v1** |
| Real data + parallel | **Hive Mind v2** |
| Maximum features | **Hive Mind v1** |
| Zero dependencies | **Hive Mind v2** |
| Time-constrained | **Beads v1/v2** |
| Full MCP tools | **Beads v2** |

## Summary Table

| Attempt | Speed | Code Size | Completeness | Quality | Data | Tests |
|---------|-------|-----------|--------------|---------|------|-------|
| Beads v1 | ⭐⭐⭐⭐⭐ | ⭐⭐⭐⭐⭐ | ⭐⭐⭐ | ⭐⭐⭐⭐⭐ | ⭐⭐⭐⭐ | ⭐⭐ |
| Beads v2 | ⭐⭐⭐⭐⭐ | ⭐⭐⭐⭐ | ⭐⭐⭐⭐ | ⭐⭐⭐⭐⭐ | ⭐⭐⭐⭐ | ⭐⭐ |
| Hive v2 | ⭐⭐⭐⭐ | ⭐⭐⭐ | ⭐⭐⭐⭐ | ⭐⭐⭐⭐⭐ | ⭐⭐⭐⭐ | ⭐⭐⭐⭐⭐ |
| Hive v1 | ⭐⭐⭐⭐ | ⭐⭐⭐⭐ | ⭐⭐⭐⭐⭐ | ⭐⭐⭐⭐ | ⭐⭐ | ⭐⭐⭐⭐⭐ |
| Swarm v1 | ⭐⭐ | ⭐⭐ | ⭐⭐⭐⭐ | ⭐⭐ | ⭐⭐⭐⭐⭐ | ⭐⭐ |
| Swarm v2 | ⭐⭐ | ⭐⭐⭐ | ⭐⭐ | ⭐⭐⭐⭐⭐ | ⭐⭐⭐⭐⭐ | ⭐⭐⭐ |

---

*Last updated: 2025-12-14*
*Attempts evaluated: 6*
