# Brazil-Bench Leaderboard

## Evaluated Attempts

| Rank | Attempt | Pattern | Duration | LOC | Tests | Compliance | Fix Commits |
|------|---------|---------|----------|-----|-------|------------|-------------|
| 🥇 | 2025-12-01-python-claude-beads | Beads | **~11 min** | **1,826** | 18 BDD | 12/16 | **0** |
| 🥈 | 2025-12-13-python-claude-hive | Hive Mind v2 | ~37 min | 6,165 | 63 BDD | 13/16 | **0** |
| 🥉 | 2025-10-30-python-hive | Hive Mind v1 | ~41 min | 3,545 | **64 BDD** | **15/16** | 1 |
| 4 | 2025-09-30-python-swarm | Swarm v1 | ~1h 49m | 8,683 | 15 E2E | 14/16 | 7 |
| 5 | 2025-12-13-python-claude-swarm | Swarm v2 | ~1h 54m | 4,227 | 38 BDD | 10/16 | **0** |

## Detailed Comparison

### Efficiency Metrics

| Metric | Beads | Hive v2 | Hive v1 | Swarm v1 | Swarm v2 | Best |
|--------|-------|---------|---------|----------|----------|------|
| **Duration** | ~11 min | ~37 min | ~41 min | ~1h 49m | ~1h 54m | Beads (10x) |
| **Lines of Code** | 1,826 | 6,165 | 3,545 | 8,683 | 4,227 | Beads |
| **Python Files** | 14 | 18 | 21 | 53 | 17 | Beads |
| **Dependencies** | 8 | 0 | 18 | 32 | 20 | Hive v2 |
| **Total Commits** | 7 | 5 | 7 | 32 | 2 | Swarm v2 |
| **Fix Commits** | 0 | 0 | 1 | 7 | 0 | Beads/Hive v2/Swarm v2 |

### Quality Metrics

| Metric | Beads | Hive v2 | Hive v1 | Swarm v1 | Swarm v2 | Best |
|--------|-------|---------|---------|----------|----------|------|
| **Spec Compliance** | 12/16 | 13/16 | 15/16 | 14/16 | 10/16 | Hive v1 |
| **BDD Scenarios** | 18 | 63 | 64 | 15 E2E | 38 | Hive v1 |
| **Real Kaggle Data** | Yes | Yes | No | Yes | Yes | 4/5 attempts |
| **Clean Implementation** | Yes | Yes | Yes | No | Yes | 4/5 attempts |
| **Token Usage** | Low | ~345k | Moderate | High | Moderate | Beads |

### Pattern Characteristics

| Pattern | Orchestration | Agents | Key Feature |
|---------|---------------|--------|-------------|
| **Beads** | Issue tracking | Single | Sequential issue resolution |
| **Hive Mind v2** | Claude-Flow Queen | 5 parallel | Researcher, 3 Coders, Tester |
| **Hive Mind v1** | Claude Flow | 4 parallel | Researcher, Coder, Tester, Analyst |
| **Swarm v1** | Claude Flow | 64 available | Dynamic coordination |
| **Swarm v2** | Claude Code | Single | Query handler pattern |

## Efficiency Rankings

### Speed (Autonomous Duration)
1. 🥇 **Beads**: ~11 min
2. 🥈 **Hive Mind v2**: ~37 min (3.4x slower)
3. 🥉 **Hive Mind v1**: ~41 min (3.7x slower)
4. 4th **Swarm v1**: ~1h 49m (10x slower)
5. 5th **Swarm v2**: ~1h 54m (10.4x slower)

### Code Efficiency (LOC)
1. 🥇 **Beads**: 1,826 lines
2. 🥈 **Hive Mind v1**: 3,545 lines (1.9x more)
3. 🥉 **Swarm v2**: 4,227 lines (2.3x more)
4. 4th **Hive Mind v2**: 6,165 lines (3.4x more)
5. 5th **Swarm v1**: 8,683 lines (4.8x more)

### Implementation Quality (Fix Commits)
1. 🥇 **Beads**: 0 fix commits
1. 🥇 **Hive Mind v2**: 0 fix commits
1. 🥇 **Swarm v2**: 0 fix commits
4. 4th **Hive Mind v1**: 1 fix commit
5. 5th **Swarm v1**: 7 fix commits

## Completeness Rankings

### Spec Compliance
1. 🥇 **Hive Mind v1**: 15/16 requirements (94%)
2. 🥈 **Swarm v1**: 14/16 requirements (88%)
3. 🥉 **Hive Mind v2**: 13/16 requirements (81%)
4. 4th **Beads**: 12/16 requirements (75%)
5. 5th **Swarm v2**: 10/16 requirements (63%)

### Test Coverage
1. 🥇 **Hive Mind v1**: 64 BDD scenarios
2. 🥈 **Hive Mind v2**: 63 BDD scenarios
3. 🥉 **Swarm v2**: 38 BDD scenarios
4. 4th **Beads**: 18 BDD scenarios
5. 5th **Swarm v1**: 15 E2E tests

## Data Strategy Comparison

| Attempt | Data Strategy | Data Size | Coverage |
|---------|---------------|-----------|----------|
| **Swarm v1** | Real Kaggle | ~96 MB | Full datasets |
| **Swarm v2** | Real Kaggle | ~11 MB | 40,000+ records |
| **Hive Mind v2** | Real Kaggle | ~11 MB | 42,000+ records |
| **Beads** | Real Kaggle | ~11 MB | Core datasets |
| **Hive Mind v1** | Simulated | N/A | Test data only |

## Hive Mind Comparison (v1 vs v2)

| Metric | Hive Mind v2 (Dec 13) | Hive Mind v1 (Oct 30) | Winner |
|--------|----------------------|----------------------|--------|
| Duration | ~37 min | ~41 min | v2 (10% faster) |
| LOC | 6,165 | 3,545 | v1 (43% less code) |
| Tests | 63 BDD | 64 BDD | Tie |
| Compliance | 13/16 | 15/16 | v1 (higher) |
| Fix Commits | 0 | 1 | v2 (cleaner) |
| Real Data | Yes (11 MB) | No | v2 |
| Dependencies | 0 (stdlib) | 18 | v2 (simpler) |
| Agents | 5 parallel | 4 parallel | Similar |
| Token Usage | ~345k | Lower | v1 (more efficient) |

**Key Differences:**
- v2 uses Claude-Flow with strategic Queen coordinator
- v2 includes real Kaggle data but lower compliance
- v1 achieves higher compliance with simulated data
- v2 has zero external dependencies (stdlib only)

## Trade-off Analysis

### Beads (Most Efficient)
**Pros:**
- Fastest implementation (10x faster than swarm)
- Leanest codebase (4.8x less code)
- Zero rework needed
- Modern Python 3.12
- Real Kaggle data

**Cons:**
- Fewer tests (18)
- Lower spec compliance (75%)

**Best for:** Quick prototypes, demos, time-constrained benchmarks

### Hive Mind v2 (Balanced + Real Data)
**Pros:**
- Fast execution (~37 min)
- Zero fix commits (clean implementation)
- Real Kaggle data included
- Zero external dependencies
- High test coverage (63 BDD)

**Cons:**
- More code than v1 (6,165 vs 3,545)
- Lower compliance than v1 (81% vs 94%)
- High token usage (~345k)

**Best for:** Real data integration with parallel agents

### Hive Mind v1 (Most Complete)
**Pros:**
- Highest spec compliance (94%)
- Best test coverage (64 BDD)
- Good efficiency balance
- Proven pattern

**Cons:**
- No real data integration
- One fix commit needed
- More dependencies

**Best for:** Production-quality implementations, comprehensive coverage

### Swarm v1 (Most Infrastructure)
**Pros:**
- Good spec compliance (88%)
- Real Kaggle data (96 MB)
- Extensive tooling available

**Cons:**
- Most code (8,683 LOC)
- Most rework (7 fix commits)
- Complex coordination overhead

**Best for:** Learning swarm patterns, extensive tooling needs

### Swarm v2 (Cleanest Swarm)
**Pros:**
- Zero fix commits (clean implementation)
- Real Kaggle data included
- Query handler pattern (good architecture)

**Cons:**
- Fewest MCP tools (6)
- Lowest spec compliance (63%)
- Slowest implementation

**Best for:** When clean implementation is priority over features

## Key Insights

1. **Beads remains fastest**: Single-agent pattern is 3-10x faster than multi-agent patterns.

2. **Hive Mind patterns excel at testing**: Both v1 (64 tests) and v2 (63 tests) lead in test coverage.

3. **Real data vs compliance trade-off persists**: Hive Mind v1 (simulated data) has highest compliance. Real data implementations score lower.

4. **Zero-dependency implementation possible**: Hive Mind v2 uses only Python stdlib, proving external deps aren't required.

5. **Token cost varies significantly**: Hive Mind v2 used ~345k tokens across 5 agents, indicating parallel execution has overhead.

6. **Clean implementations are common**: 3/5 attempts (60%) had zero fix commits.

## Visualizations

### Duration Comparison
```
Beads:      ██ (~11 min)
Hive v2:    ███████ (~37 min)
Hive v1:    ████████ (~41 min)
Swarm v1:   █████████████████████ (~109 min)
Swarm v2:   ██████████████████████ (~114 min)
```

### Lines of Code
```
Beads:      ██ (1,826)
Hive v1:    ████ (3,545)
Swarm v2:   █████ (4,227)
Hive v2:    ███████ (6,165)
Swarm v1:   █████████ (8,683)
```

### Spec Compliance
```
Hive v1:    ███████████████ (15/16 = 94%)
Swarm v1:   ██████████████ (14/16 = 88%)
Hive v2:    █████████████ (13/16 = 81%)
Beads:      ████████████ (12/16 = 75%)
Swarm v2:   ██████████ (10/16 = 63%)
```

### Fix Commits (Lower is Better)
```
Beads:      (0)
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
Beads:      █████████ (18)
Swarm v1:   ███████ (15)
```

## Recommendations

| Scenario | Recommended Pattern |
|----------|---------------------|
| Quick prototype | **Beads** |
| Production quality | **Hive Mind v1** |
| Real data + parallel | **Hive Mind v2** |
| Real data + simple | **Beads** |
| Maximum features | **Hive Mind v1** |
| Zero dependencies | **Hive Mind v2** |
| Learning patterns | **Swarm v1** |
| Time-constrained | **Beads** |

## Summary Table

| Attempt | Speed | Code Size | Completeness | Quality | Data | Tests |
|---------|-------|-----------|--------------|---------|------|-------|
| Beads | ⭐⭐⭐⭐⭐ | ⭐⭐⭐⭐⭐ | ⭐⭐⭐ | ⭐⭐⭐⭐⭐ | ⭐⭐⭐⭐ | ⭐⭐ |
| Hive v2 | ⭐⭐⭐⭐ | ⭐⭐⭐ | ⭐⭐⭐⭐ | ⭐⭐⭐⭐⭐ | ⭐⭐⭐⭐ | ⭐⭐⭐⭐⭐ |
| Hive v1 | ⭐⭐⭐⭐ | ⭐⭐⭐⭐ | ⭐⭐⭐⭐⭐ | ⭐⭐⭐⭐ | ⭐⭐ | ⭐⭐⭐⭐⭐ |
| Swarm v1 | ⭐⭐ | ⭐⭐ | ⭐⭐⭐⭐ | ⭐⭐ | ⭐⭐⭐⭐⭐ | ⭐⭐ |
| Swarm v2 | ⭐⭐ | ⭐⭐⭐ | ⭐⭐ | ⭐⭐⭐⭐⭐ | ⭐⭐⭐⭐⭐ | ⭐⭐⭐ |

---

*Last updated: 2025-12-13*
*Attempts evaluated: 5*
