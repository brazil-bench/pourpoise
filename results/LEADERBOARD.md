# Brazil-Bench Leaderboard

## Evaluated Attempts

| Rank | Attempt | Pattern | Duration | LOC | Tests | Compliance |
|------|---------|---------|----------|-----|-------|------------|
| 🥇 | 2025-12-01-python-claude-beads | Beads | **~11 min** | **1,826** | 18 BDD | 12/16 |
| 🥈 | 2025-10-30-python-hive | Hive Mind | ~41 min | 3,545 | **64 BDD** | **15/16** |
| 🥉 | 2025-09-30-python-swarm | Swarm | ~1h 49m | 8,683 | 15 E2E | 14/16 |

## Detailed Comparison

### Efficiency Metrics

| Metric | Beads | Hive Mind | Swarm | Best |
|--------|-------|-----------|-------|------|
| **Autonomous Duration** | ~11 min | ~41 min | ~1h 49m | Beads (10x faster) |
| **Lines of Code** | 1,826 | 3,545 | 8,683 | Beads (4.8x leaner) |
| **Python Files** | 14 | 21 | 53 | Beads (3.8x fewer) |
| **Dependencies** | 8 | 18 | 32 | Beads (4x fewer) |
| **Total Commits** | 7 | 7 | 32 | Tie (Beads/Hive) |
| **Fix Commits** | 0 | 1 | 7 | Beads (zero rework) |

### Quality Metrics

| Metric | Beads | Hive Mind | Swarm | Best |
|--------|-------|-----------|-------|------|
| **Spec Compliance** | 12/16 | 15/16 | 14/16 | Hive Mind |
| **MCP Tools** | 12 | 15 | 13 | Hive Mind |
| **BDD Scenarios** | 18 | 64 | 15 E2E | Hive Mind |
| **Test Pass Rate** | 100% | 100% | 100% | Tie |
| **Clean Implementation** | Yes | Yes | No | Beads/Hive |

### Pattern Characteristics

| Pattern | Coordination | Agents | Approach |
|---------|--------------|--------|----------|
| **Beads** | Issue tracking | Single (Claude 4.5) | Sequential issue resolution |
| **Hive Mind** | Claude Flow | 4 parallel | Researcher, Coder, Tester, Analyst |
| **Swarm** | Claude Flow | 64 available | Dynamic swarm coordination |

## Efficiency Rankings

### Speed (Autonomous Duration)
1. 🥇 **Beads**: ~11 min (core implementation)
2. 🥈 **Hive Mind**: ~41 min (3.7x slower)
3. 🥉 **Swarm**: ~1h 49m (10x slower)

### Code Efficiency (LOC)
1. 🥇 **Beads**: 1,826 lines
2. 🥈 **Hive Mind**: 3,545 lines (1.9x more)
3. 🥉 **Swarm**: 8,683 lines (4.8x more)

### Implementation Quality (Fix Commits)
1. 🥇 **Beads**: 0 fix commits
2. 🥈 **Hive Mind**: 1 fix commit
3. 🥉 **Swarm**: 7 fix commits

## Completeness Rankings

### Spec Compliance
1. 🥇 **Hive Mind**: 15/16 requirements
2. 🥈 **Swarm**: 14/16 requirements
3. 🥉 **Beads**: 12/16 requirements

### Test Coverage
1. 🥇 **Hive Mind**: 64 BDD scenarios
2. 🥈 **Beads**: 18 BDD scenarios
3. 🥉 **Swarm**: 15 E2E tests

### MCP Tool Coverage
1. 🥇 **Hive Mind**: 15 tools
2. 🥈 **Swarm**: 13 tools
3. 🥉 **Beads**: 12 tools

## Trade-off Analysis

### Beads (Most Efficient)
**Pros:**
- Fastest implementation by far
- Leanest codebase
- Zero rework needed
- Modern Python 3.12
- Simple architecture

**Cons:**
- Fewer MCP tools
- Lower spec compliance
- Fewer test scenarios

**Best for:** Quick prototypes, demos, time-constrained benchmarks

### Hive Mind (Most Complete)
**Pros:**
- Highest spec compliance
- Most MCP tools
- Best test coverage
- Good efficiency balance

**Cons:**
- 4x more code than Beads
- 4x slower than Beads
- More complex setup

**Best for:** Production-quality implementations, comprehensive coverage

### Swarm (Most Comprehensive Infrastructure)
**Pros:**
- Extensive tooling available
- Full infrastructure for scaling
- Good spec compliance

**Cons:**
- Slowest implementation
- Most code to maintain
- Most rework needed
- Complex coordination

**Best for:** Large-scale projects requiring extensive agent coordination

## Pattern Recommendations

| Scenario | Recommended Pattern |
|----------|---------------------|
| Quick demo/prototype | **Beads** |
| Production MCP server | **Hive Mind** |
| Complex multi-agent system | **Swarm** |
| Time-constrained benchmark | **Beads** |
| Maximum test coverage | **Hive Mind** |
| Learning/experimentation | **Beads** or **Hive Mind** |

## Key Insights

1. **Parallel agents don't always mean faster**: Beads (single agent) was 10x faster than Swarm (64 agents available).

2. **Less code can mean more focus**: Beads' minimal approach avoided rework entirely.

3. **Test quantity vs implementation time**: Hive Mind achieved 64 tests in 41 min, while Swarm managed only 15 in 109 min.

4. **Coordination overhead matters**: Swarm's complex coordination led to more fix commits and longer duration.

5. **Issue-driven development works**: Beads' simple issue tracking proved highly effective for AI coordination.

## Visualizations

### Duration Comparison
```
Beads:     ████ (~11 min)
Hive Mind: ████████████████ (~41 min)
Swarm:     ████████████████████████████████████████ (~109 min)
```

### Lines of Code
```
Beads:     ██ (1,826)
Hive Mind: ████ (3,545)
Swarm:     █████████ (8,683)
```

### Fix Commits (Lower is Better)
```
Beads:     (0)
Hive Mind: █ (1)
Swarm:     ███████ (7)
```

---

*Last updated: 2025-12-13*
*Attempts evaluated: 3*
