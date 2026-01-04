# Brazil-Bench Leaderboard

> Last updated: 2025-01-04
> Attempts evaluated: 8
> Methodology: Effective Tests with Skip Penalty + Integration Test Quality (v4)

## Scoring Methodology

```
Score = (Spec Compliance % × 50) + (Test Score × 30) + (Quality × 15) + (Efficiency × 5)

Where:
- Spec Compliance % = (implemented / total) × 100
- Test Score = min(100, EFFECTIVE_TESTS × 1.5)  [Uses effective tests, not total]
- Quality = 100 - (fix_commits × 10) - skip_penalty - integration_penalty
- Skip Penalty = max(0, (skip_ratio - 0.10) × 50)  [Penalizes >10% skipped tests]
- Integration Penalty = 10 if tests skip due to missing dependencies, 0 if self-contained
- Efficiency = 100 - min(100, LOC / 100)
```

**Priority Order:** Compliance > Effective Tests > Quality > Integration Tests > Duration

**Integration Test Requirement (v4):**
- Tests must be self-contained using testcontainers or pytest-docker
- Tests that skip with "Neo4j not running" receive -10 quality penalty
- **In-memory mocks are NOT acceptable** - storage must be persistent
- testcontainers with real Neo4j in Docker is the recommended approach

---

## Top 10 Leaderboard

| Rank | Attempt | Pattern | Score | Spec | Tests | Integration | Skip% | Issues |
|------|---------|---------|-------|------|-------|-------------|-------|--------|
| 1 🥇 | 2025-12-14-python-claude-beads-2 | Beads v3 | **91.8** | 16/16 | 59 | ✗ External | 20% | 2 open |
| 2 🥈 | 2025-10-30-python-hive | Hive v1 | **90.9** | 15/16 | 64 | ✗ External | 0% | 3 open |
| 3 🥉 | 2025-12-15-python-claude-ruvector | RuVector | **90.1** | 16/16 | 61 | ✗ External | 0% | 2 open |
| 4 | 2025-12-13-python-claude-hive | Hive v2 | **86.0** | 16/16 | 43 | ✓ testcontainers | 0% | 0 |
| 5 | 2025-12-13-python-claude-swarm | Swarm v2 | **82.8** | 16/16 | 37 | ✗ Skip | 3% | 1 open |
| 6 | 2025-12-14-python-claude-beads | Beads v2 | **78.4** | 16/16 | 25 | ✓ testcontainers | 0% | 0 |
| 7 | 2025-12-01-python-claude-beads | Beads v1 | **75.5** | 16/16 | 18 | ✗ Mock | 0% | 1 open |
| 8 | 2025-09-30-python-swarm | Swarm v1 | **54.2** | 14/16 | 15 | ✗ Skip | 0% | 5 open |

---

## Critical: Test Quality Analysis

### Test Counts vs Effective Tests

| Attempt | Total Tests | Skipped | **Effective** | Skip Type | Verdict |
|---------|-------------|---------|---------------|-----------|---------|
| Beads v3 | 74 | 15 | **59** | Integration (Neo4j) | Acceptable |
| Hive v1 | 64 | 0 | **64** | None | Excellent |
| RuVector | 61 | 0 | **61** | None | Excellent |
| Swarm v2 | 38 | ~1 | **~37** | External dep | Clean |
| Hive v2 | 43 | 0 | **43** | None (testcontainers) | Excellent |
| Beads v2 | 25 | 0 | **25** | None (testcontainers) | Excellent |
| Beads v1 | 18 | 0 | **18** | None | Clean |
| Swarm v1 | 15 | 0 | **15** | None | Clean |

### Integration Test Quality (v4 Requirement)

| Attempt | Pattern | Self-Contained | Persistent | Penalty | Issue |
|---------|---------|----------------|------------|---------|-------|
| Beads v3 | `pytest.skip("requires Neo4j")` | ✗ No | N/A | -1.5 | #2 filed |
| Hive v1 | External `localhost:7687` | ✗ No | ✗ External | -1.5 | #3 filed |
| RuVector | `pytest.skip("RuVector not available")` | ✗ No | N/A | -1.5 | #2 filed |
| Swarm v2 | `pytest.skip("Neo4j connection failed")` | ✗ No | N/A | -1.5 | #6 filed |
| Hive v2 | `testcontainers.Neo4jContainer` | ✓ Yes | ✓ Docker | 0 | #7 closed ✓ |
| Beads v1 | `MockNeo4jDatabase` in-memory | ✗ No | ✗ Not persistent | -1.5 | #8 filed |
| Beads v2 | `testcontainers.Neo4jContainer` | ✓ Yes | ✓ Docker | 0 | #4 closed ✓ |
| Swarm v1 | Multiple `pytest.skip()` patterns | ✗ No | N/A | -1.5 | #5 filed |

> **6 of 8 attempts fail** the self-contained integration test requirement.
> **Hive v2 and Beads v2** are the only attempts using testcontainers for self-contained tests.
> Tests must use testcontainers with real persistent storage (Neo4j in Docker).
> In-memory mocks like `MockNeo4jDatabase` do not provide persistence.

### Re-evaluation Score Changes

| Attempt | Initial Score | Current Score | Change | Reason |
|---------|---------------|---------------|--------|--------|
| Hive v2 | 56.5 | **86.0** | +29.5 | 7 issues closed, testcontainers implemented, 0 skips |
| Swarm v2 | 65.8 | **82.8** | +17.0 | 5 issues closed, -1.5 integration penalty |
| Beads v1 | 64.7 | **75.5** | +10.8 | 7 issues closed, -1.5 integration penalty (mock not persistent) |
| Beads v2 | 68.6 | **78.4** | +9.8 | 4 issues closed, testcontainers implemented, 16/16 compliance |

---

## Score Breakdown

| Attempt | Compliance (50%) | Tests (30%) | Quality (15%) | Efficiency (5%) | **Total** |
|---------|------------------|-------------|---------------|-----------------|-----------|
| Beads v3 | 50.00 | 26.55 | 12.75 | 2.53 | **91.8** |
| Hive v1 | 46.88 | 28.80 | 12.00 | 3.23 | **90.9** |
| RuVector | 50.00 | 27.45 | 12.00 | 0.63 | **90.1** |
| Swarm v2 | 50.00 | 16.65 | 13.50 | 2.23 | **82.8** |
| Hive v2 | 50.00 | 19.35 | 13.50 | 3.12 | **86.0** |
| Beads v1 | 50.00 | 8.10 | 13.50 | 3.91 | **75.5** |
| Beads v2 | 50.00 | 11.25 | 13.50 | 3.60 | **78.4** |
| Swarm v1 | 43.75 | 6.75 | 3.00 | 0.66 | **54.2** |

*Quality now includes -10 integration penalty for non-self-contained tests (×15% = -1.5 points)*

### Top 3 Detailed Scoring

**1st Place: Beads v3 (91.8)**
```
Compliance: 16/16 (100%) × 50 = 50.00
Tests: min(100, 59×1.5)=88.5 × 30% = 26.55
Quality: (100 - 0 - 5 - 10) × 15% = 12.75  [5pt skip, 10pt integration]
Efficiency: (100 - 49.5) × 5% = 2.53
```

**2nd Place: Hive v1 (90.9)**
```
Compliance: 15/16 (93.75%) × 50 = 46.88
Tests: min(100, 64×1.5)=96 × 30% = 28.80
Quality: (100 - 10 - 0 - 10) × 15% = 12.00  [10pt fix, 10pt integration]
Efficiency: (100 - 35.5) × 5% = 3.23
```

**3rd Place: RuVector (90.1)**
```
Compliance: 16/16 (100%) × 50 = 50.00
Tests: min(100, 61×1.5)=91.5 × 30% = 27.45
Quality: (100 - 10 - 0 - 10) × 15% = 12.00  [10pt fix, 10pt integration]
Efficiency: (100 - 87.5) × 5% = 0.63
```

**6th Place: Beads v1 (75.5)**
```
Compliance: 16/16 (100%) × 50 = 50.00
Tests: min(100, 18×1.5)=27 × 30% = 8.10
Quality: (100 - 0 - 0 - 10) × 15% = 13.50  [10pt integration - MockNeo4jDatabase not persistent]
Efficiency: (100 - 21.9) × 5% = 3.91
```

### Hive v2 Score Improvement (Rank #7 → #4)

**After 7 issues closed with pytest-bdd rewrite + testcontainers:**
```
Compliance: 16/16 (100%) × 50 = 50.00  [was 13/16]
Tests: min(100, 43×1.5)=64.5 × 30% = 19.35  [was ~10 effective, now 43]
Quality: (100 - 10 - 0 - 0) × 15% = 13.50  [1 fix, 0 skip, 0 integration]
Efficiency: (100 - 37.54) × 5% = 3.12
Total: 86.0  [was 56.5, +29.5 improvement]
```

**Key Achievement:** First and only attempt to implement testcontainers for self-contained integration tests.

---

## Detailed Metrics Comparison

| Metric | Beads v3 | Hive v1 | RuVector | Swarm v2 | Hive v2 | Beads v1 | Beads v2 | Swarm v1 |
|--------|----------|---------|----------|----------|---------|----------|----------|----------|
| **Pattern** | Beads | Hive | Hive+RuVector | Swarm | Hive | Beads | Beads | Swarm |
| **Spec Compliance** | 16/16 | 15/16 | 16/16 | 16/16 | 16/16 | 16/16 | 16/16 | 14/16 |
| **Effective Tests** | 59 | 64 | 61 | 37 | 43 | 18 | 25 | 15 |
| **Skip Ratio** | 20% | 0% | 0% | ~3% | 0% | 0% | 0% | 0% |
| **Lines of Code** | 4,947 | 3,545 | 8,751 | 5,546 | 3,754 | 2,194 | 2,793 | 8,683 |
| **Fix Commits** | 0 | 1 | 1 | 0 | 1 | 0 | 1 | 7 |
| **Duration** | ~23m | ~41m | ~2h 18m | ~1h 54m | ~41m | ~11m | ~14m | ~1h 49m |
| **Real Data** | Yes | Yes | Yes | Yes | Yes | Yes | Yes | Yes |
| **Open Issues** | 2 | 3 | 1 | 1 | **0** | 1 | **0** | 5 |

---

## Pattern Performance

| Pattern | Attempts | Avg Score | Best Rank | Avg Effective Tests | Notes |
|---------|----------|-----------|-----------|---------------------|-------|
| **Hive** | 2 | 87.6 | 2nd | 50 | Strong pattern (v1: 92.4, v2: 82.7) |
| **Beads** | 3 | 80.1 | 1st | 32 | Most consistent pattern |
| **RuVector** | 1 | 91.6 | 3rd | 61 | Innovative architecture |
| **Swarm** | 2 | 70.0 | 4th | 26 | Varied results (v1: 55.7, v2: 84.3) |

---

## Analysis

### Winner: Beads v3 (2025-12-14-python-claude-beads-2)

The Beads v3 attempt wins by combining:
- **Full spec compliance** (16/16) - the only Beads attempt to achieve this
- **High effective test count** (59) - legitimate unit tests that actually run
- **Zero fix commits** - clean first-pass implementation
- **Acceptable skip ratio** (20%) - only integration tests for Neo4j skip

### Key Differentiators

| Aspect | Beads v3 | Hive v1 | RuVector |
|--------|----------|---------|----------|
| Compliance | 16/16 | 15/16 | 16/16 |
| Effective Tests | 59 | 64 | 61 |
| Skip Ratio | 20% | 0% | 0% |
| Fix Commits | 0 | 1 | 1 |
| LOC | 4,947 | 3,545 | 8,751 |
| Duration | 23m | 41m | 2h 18m |

Hive v1 has the most effective tests (64) with 0% skip ratio, but loses to Beads v3
due to slightly lower compliance (15/16 vs 16/16).

### Pattern Insights

- **Best pattern:** Hive (highest avg score: 87.6)
- **Most efficient:** Beads v1 (lowest LOC: 2,194)
- **Most tests:** Hive v1 (64 effective tests)
- **Fastest:** Beads v1 (~11 min autonomous)
- **Best improvement:** Hive v2 (+26.2 points after issue fixes)

### Common Strengths (Top 3)
- Full or near-full spec compliance
- Zero or one fix commits (clean implementations)
- Real Kaggle data integration
- Comprehensive BDD test coverage

### Areas for Improvement
- ~~**Hive v2:** Tests were created but functionality never implemented~~ (Fixed: pytest-bdd rewrite)
- **Swarm v1:** High fix commit count (7) indicates iteration issues
- **Beads v2:** Lower compliance (14/16) with 3 open issues

---

## Recommendations

| Goal | Recommended Attempt |
|------|---------------------|
| **Highest quality** | Beads v3 (93.3 score, 100% compliance) |
| **Most tests** | Hive v1 (64 effective BDD scenarios) |
| **Full compliance** | Beads v3 or RuVector (16/16) |
| **Cleanest code** | Beads v1 (1,826 LOC, 0 fixes) |
| **Fastest prototype** | Beads v1 (~11 min) |
| **Innovation** | RuVector (vector DB architecture) |

---

## Data Sources

| Attempt | Report |
|---------|--------|
| 2025-12-14-python-claude-beads-2 | results/2025-12-14-python-claude-beads-2.md |
| 2025-10-30-python-hive | results/2025-10-30-python-hive.md |
| 2025-12-15-python-claude-ruvector | results/2025-12-15-python-claude-ruvector.md |
| 2025-12-14-python-claude-beads | results/2025-12-14-python-claude-beads.md |
| 2025-12-13-python-claude-swarm | results/2025-12-13-python-claude-swarm.md |
| 2025-12-01-python-claude-beads | results/2025-12-01-python-claude-beads.md |
| 2025-12-13-python-claude-hive | results/2025-12-13-python-claude-hive.md |
| 2025-09-30-python-swarm | results/2025-09-30-python-swarm.md |

---

## Issue Tracking Summary

| Attempt | Open | Closed | Total | Categories |
|---------|------|--------|-------|------------|
| 2025-12-14-python-claude-beads-2 | 2 | 0 | 2 | Test Quality |
| 2025-10-30-python-hive | 3 | 0 | 3 | Missing, Compliance, Test Quality |
| 2025-12-15-python-claude-ruvector | 2 | 1 | 3 | Test Quality (testcontainers), Bug (path) |
| 2025-12-13-python-claude-swarm | 1 | 5 | 6 | Test Quality (testcontainers) |
| **2025-12-13-python-claude-hive** | **0** | **7** | 7 | ~~All Fixed~~ ✓ |
| 2025-12-01-python-claude-beads | 1 | 7 | 8 | Test Quality (in-memory mock) |
| **2025-12-14-python-claude-beads** | **0** | **4** | 4 | ~~All Fixed~~ ✓ |
| 2025-09-30-python-swarm | 5 | 0 | 5 | Missing, Test Quality, Compliance |

**Total Open Issues:** 14

### Issue Distribution by Type

| Issue Type | Count | Description |
|------------|-------|-------------|
| `[Missing]` | ~12 | Missing spec requirements |
| `[Test Quality]` | ~8 | Skipped tests, best practices |
| `[Compliance]` | ~7 | Summary issues linking requirements |
| `[Docs]` | ~2 | README documentation gaps |

---

Generated by compare-attempts SOP on 2025-01-04 (v3 - Effective Tests with Skip Penalty)
