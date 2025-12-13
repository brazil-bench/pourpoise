# Brazil-Bench Leaderboard

## Evaluated Attempts

| Rank | Attempt | Pattern | Duration | LOC | Tests | Compliance | Fix Commits |
|------|---------|---------|----------|-----|-------|------------|-------------|
| 🥇 | 2025-12-01-python-claude-beads | Beads | **~11 min** | **1,826** | 18 BDD | 12/16 | **0** |
| 🥈 | 2025-10-30-python-hive | Hive Mind | ~41 min | 3,545 | **64 BDD** | **15/16** | 1 |
| 🥉 | 2025-12-13-python-claude-swarm | Swarm v2 | ~1h 54m | 4,227 | 38 BDD | 10/16 | **0** |
| 4 | 2025-09-30-python-swarm | Swarm v1 | ~1h 49m | 8,683 | 15 E2E | 14/16 | 7 |

## Detailed Comparison

### Efficiency Metrics

| Metric | Beads | Hive Mind | Swarm v2 | Swarm v1 | Best |
|--------|-------|-----------|----------|----------|------|
| **Duration** | ~11 min | ~41 min | ~1h 54m | ~1h 49m | Beads (10x) |
| **Lines of Code** | 1,826 | 3,545 | 4,227 | 8,683 | Beads (4.8x) |
| **Python Files** | 14 | 21 | 17 | 53 | Beads |
| **Dependencies** | 8 | 18 | 20 | 32 | Beads |
| **Total Commits** | 7 | 7 | 2 | 32 | Swarm v2 |
| **Fix Commits** | 0 | 1 | 0 | 7 | Beads/Swarm v2 |

### Quality Metrics

| Metric | Beads | Hive Mind | Swarm v2 | Swarm v1 | Best |
|--------|-------|-----------|----------|----------|------|
| **Spec Compliance** | 12/16 | 15/16 | 10/16 | 14/16 | Hive Mind |
| **MCP Tools** | 12 | 15 | 6 | 13 | Hive Mind |
| **BDD Scenarios** | 18 | 64 | 38 | 15 E2E | Hive Mind |
| **Real Kaggle Data** | Yes | No | Yes | Yes | 3/4 attempts |
| **Clean Implementation** | Yes | Yes | Yes | No | 3/4 attempts |

### Pattern Characteristics

| Pattern | Orchestration | Agents | Key Feature |
|---------|---------------|--------|-------------|
| **Beads** | Issue tracking | Single | Sequential issue resolution |
| **Hive Mind** | Claude Flow | 4 parallel | Researcher, Coder, Tester, Analyst |
| **Swarm v2** | Claude Code | Single | Query handler pattern |
| **Swarm v1** | Claude Flow | 64 available | Dynamic coordination |

## Efficiency Rankings

### Speed (Autonomous Duration)
1. 🥇 **Beads**: ~11 min
2. 🥈 **Hive Mind**: ~41 min (3.7x slower)
3. 🥉 **Swarm v1**: ~1h 49m (10x slower)
4. 4th **Swarm v2**: ~1h 54m (10.4x slower)

### Code Efficiency (LOC)
1. 🥇 **Beads**: 1,826 lines
2. 🥈 **Hive Mind**: 3,545 lines (1.9x more)
3. 🥉 **Swarm v2**: 4,227 lines (2.3x more)
4. 4th **Swarm v1**: 8,683 lines (4.8x more)

### Implementation Quality (Fix Commits)
1. 🥇 **Beads**: 0 fix commits
1. 🥇 **Swarm v2**: 0 fix commits
3. 🥉 **Hive Mind**: 1 fix commit
4. 4th **Swarm v1**: 7 fix commits

## Completeness Rankings

### Spec Compliance
1. 🥇 **Hive Mind**: 15/16 requirements (94%)
2. 🥈 **Swarm v1**: 14/16 requirements (88%)
3. 🥉 **Beads**: 12/16 requirements (75%)
4. 4th **Swarm v2**: 10/16 requirements (63%)

### Test Coverage
1. 🥇 **Hive Mind**: 64 BDD scenarios
2. 🥈 **Swarm v2**: 38 BDD scenarios
3. 🥉 **Beads**: 18 BDD scenarios
4. 4th **Swarm v1**: 15 E2E tests

### MCP Tool Coverage
1. 🥇 **Hive Mind**: 15 tools
2. 🥈 **Swarm v1**: 13 tools
3. 🥉 **Beads**: 12 tools
4. 4th **Swarm v2**: 6 tools

## Data Strategy Comparison

| Attempt | Data Strategy | Data Size | Coverage |
|---------|---------------|-----------|----------|
| **Swarm v2** | Real Kaggle | ~11 MB | 40,000+ records |
| **Swarm v1** | Real Kaggle | ~96 MB | Full datasets |
| **Beads** | Real Kaggle | ~11 MB | Core datasets |
| **Hive Mind** | Simulated | N/A | Test data only |

## Trade-off Analysis

### Beads (Most Efficient)
**Pros:**
- Fastest implementation (10x faster than swarm)
- Leanest codebase (4.8x less code)
- Zero rework needed
- Modern Python 3.12
- Real Kaggle data

**Cons:**
- Fewer MCP tools (12)
- Lower spec compliance (75%)

**Best for:** Quick prototypes, demos, time-constrained benchmarks

### Hive Mind (Most Complete)
**Pros:**
- Highest spec compliance (94%)
- Most MCP tools (15)
- Best test coverage (64 BDD)
- Good efficiency balance

**Cons:**
- 4x more code than Beads
- 4x slower than Beads
- No real data integration

**Best for:** Production-quality implementations, comprehensive coverage

### Swarm v2 (Cleanest Swarm)
**Pros:**
- Zero fix commits (clean implementation)
- Real Kaggle data included
- Query handler pattern (good architecture)
- Good test coverage (38 BDD)

**Cons:**
- Fewest MCP tools (6)
- Lowest spec compliance (63%)
- Slowest implementation

**Best for:** When real data integration is priority

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

## Key Insights

1. **Simpler patterns win on efficiency**: Beads (single agent) was 10x faster than Swarm patterns.

2. **Parallel agents improve completeness**: Hive Mind's 4 specialized agents achieved best spec compliance and test coverage.

3. **Clean implementations are possible**: Both Beads and Swarm v2 had zero fix commits, proving clean first-pass implementation is achievable.

4. **Real data vs completeness trade-off**: Attempts with real Kaggle data (Beads, Swarm v1/v2) had lower spec compliance than Hive Mind with simulated data.

5. **Test quantity correlates with pattern**: Hive Mind (64 tests) > Swarm v2 (38) > Beads (18) > Swarm v1 (15).

## Visualizations

### Duration Comparison
```
Beads:     ██ (~11 min)
Hive Mind: ████████ (~41 min)
Swarm v2:  ██████████████████████ (~114 min)
Swarm v1:  █████████████████████ (~109 min)
```

### Lines of Code
```
Beads:     ██ (1,826)
Hive Mind: ████ (3,545)
Swarm v2:  █████ (4,227)
Swarm v1:  █████████ (8,683)
```

### Spec Compliance
```
Hive Mind: ███████████████ (15/16 = 94%)
Swarm v1:  ██████████████ (14/16 = 88%)
Beads:     ████████████ (12/16 = 75%)
Swarm v2:  ██████████ (10/16 = 63%)
```

### Fix Commits (Lower is Better)
```
Beads:     (0)
Swarm v2:  (0)
Hive Mind: █ (1)
Swarm v1:  ███████ (7)
```

## Recommendations

| Scenario | Recommended Pattern |
|----------|---------------------|
| Quick prototype | **Beads** |
| Production quality | **Hive Mind** |
| Real data focus | **Swarm v2** or **Beads** |
| Maximum features | **Hive Mind** |
| Learning patterns | **Swarm v1** |
| Time-constrained | **Beads** |

## Summary Table

| Attempt | Speed | Code Size | Completeness | Quality | Data |
|---------|-------|-----------|--------------|---------|------|
| Beads | ⭐⭐⭐⭐⭐ | ⭐⭐⭐⭐⭐ | ⭐⭐⭐ | ⭐⭐⭐⭐⭐ | ⭐⭐⭐⭐ |
| Hive Mind | ⭐⭐⭐⭐ | ⭐⭐⭐⭐ | ⭐⭐⭐⭐⭐ | ⭐⭐⭐⭐ | ⭐⭐ |
| Swarm v2 | ⭐⭐ | ⭐⭐⭐ | ⭐⭐ | ⭐⭐⭐⭐⭐ | ⭐⭐⭐⭐⭐ |
| Swarm v1 | ⭐⭐ | ⭐⭐ | ⭐⭐⭐⭐ | ⭐⭐ | ⭐⭐⭐⭐⭐ |

---

*Last updated: 2025-12-13*
*Attempts evaluated: 4*
