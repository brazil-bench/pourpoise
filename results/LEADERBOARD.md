# Brazil-Bench Leaderboard

> Last updated: 2025-12-16
> Attempts evaluated: 8
> Methodology: Effective Tests with Skip Penalty Scoring (v3)

## Scoring Methodology

```
Score = (Spec Compliance % × 50) + (Test Score × 30) + (Quality × 15) + (Efficiency × 5)

Where:
- Spec Compliance % = (implemented / total) × 100
- Test Score = min(100, EFFECTIVE_TESTS × 1.5)  [Uses effective tests, not total]
- Quality = 100 - (fix_commits × 10) - skip_penalty
- Skip Penalty = max(0, (skip_ratio - 0.10) × 50)  [Penalizes >10% skipped tests]
- Efficiency = 100 - min(100, LOC / 100)
```

**Priority Order:** Compliance > Effective Tests > Quality > Skip Ratio > Duration

---

## Top 10 Leaderboard

| Rank | Attempt | Pattern | Score | Spec | Effective Tests | Skip% | Duration | Issues |
|------|---------|---------|-------|------|-----------------|-------|----------|--------|
| 1 🥇 | 2025-12-14-python-claude-beads-2 | Beads v3 | **93.3** | 16/16 | 59 | 20% | ~23m | 1 open |
| 2 🥈 | 2025-10-30-python-hive | Hive v1 | **92.4** | 15/16 | 64 | 0% | ~41m | 2 open |
| 3 🥉 | 2025-12-15-python-claude-ruvector | RuVector | **91.6** | 16/16 | 61 | 0% | ~2h 18m | 1 open |
| 4 | 2025-12-14-python-claude-beads | Beads v2 | **70.1** | 14/16 | 18 | 0% | ~14m | 3 open |
| 5 | 2025-12-13-python-claude-swarm | Swarm v2 | **65.8** | 10/16 | 37 | 3% | ~1h 54m | 5 open |
| 6 | 2025-12-01-python-claude-beads | Beads v1 | **64.7** | 12/16 | 18 | 0% | ~11m | 7 open |
| 7 | 2025-12-13-python-claude-hive | Hive v2 | **56.5** | 13/16 | ~10 | **84%** | ~37m | 6 open |
| 8 | 2025-09-30-python-swarm | Swarm v1 | **55.7** | 14/16 | 15 | 0% | ~1h 49m | 4 open |

---

## Critical: Test Quality Analysis

### Test Counts vs Effective Tests

| Attempt | Total Tests | Skipped | **Effective** | Skip Type | Verdict |
|---------|-------------|---------|---------------|-----------|---------|
| Beads v3 | 74 | 15 | **59** | Integration (Neo4j) | Acceptable |
| Hive v1 | 64 | 0 | **64** | None | Excellent |
| RuVector | 61 | 0 | **61** | None | Excellent |
| Beads v2 | 18 | 0 | **18** | None | Clean |
| Swarm v2 | 38 | ~1 | **~37** | External dep | Clean |
| Beads v1 | 18 | 0 | **18** | None | Clean |
| **Hive v2** | **63** | **53** | **~10** | **"Not implemented"** | **INFLATED** |
| Swarm v1 | 15 | 0 | **15** | None | Clean |

### Inflated Test Count Warning

> **2025-12-13-python-claude-hive (Hive v2)** has 53 of 63 tests (84%) that skip with
> "not yet implemented" messages. This is NOT a legitimate integration test skip pattern.
>
> Skip messages found:
> - "DataLoader not yet implemented" (13 tests)
> - "QueryEngine not yet implemented" (14 tests)
> - "TeamNormalizer not yet implemented" (13 tests)
> - Various "not yet implemented" (12 tests)
>
> **Impact:** Score drops from ~85 to **56.5** due to:
> - Skip penalty: (0.84 - 0.10) × 50 = **37 points** off quality
> - Effective test count: **~10** instead of 63

---

## Score Breakdown

| Attempt | Compliance (50%) | Tests (30%) | Quality (15%) | Efficiency (5%) | **Total** |
|---------|------------------|-------------|---------------|-----------------|-----------|
| Beads v3 | 50.00 | 26.55 | 14.25 | 2.53 | **93.3** |
| Hive v1 | 46.88 | 28.80 | 13.50 | 3.23 | **92.4** |
| RuVector | 50.00 | 27.45 | 13.50 | 0.63 | **91.6** |
| Beads v2 | 43.75 | 8.10 | 15.00 | 3.24 | **70.1** |
| Swarm v2 | 31.25 | 16.65 | 15.00 | 2.89 | **65.8** |
| Beads v1 | 37.50 | 8.10 | 15.00 | 4.09 | **64.7** |
| **Hive v2** | 40.63 | 4.50 | **9.45** | 1.92 | **56.5** |
| Swarm v1 | 43.75 | 6.75 | 4.50 | 0.66 | **55.7** |

### Top 3 Detailed Scoring

**1st Place: Beads v3 (93.3)**
```
Compliance: 16/16 (100%) × 50 = 50.00
Tests: min(100, 59×1.5)=88.5 × 30 = 26.55
Quality: (100 - 0 - 5) × 15 = 14.25  [5pt skip penalty for 20%]
Efficiency: (100 - 49.5) × 5 = 2.53
```

**2nd Place: Hive v1 (92.4)**
```
Compliance: 15/16 (93.75%) × 50 = 46.88
Tests: min(100, 64×1.5)=96 × 30 = 28.80
Quality: (100 - 10 - 0) × 15 = 13.50  [10pt for 1 fix commit]
Efficiency: (100 - 35.5) × 5 = 3.23
```

**3rd Place: RuVector (91.6)**
```
Compliance: 16/16 (100%) × 50 = 50.00
Tests: min(100, 61×1.5)=91.5 × 30 = 27.45
Quality: (100 - 10 - 0) × 15 = 13.50  [10pt for 1 fix commit]
Efficiency: (100 - 87.5) × 5 = 0.63
```

### Why Hive v2 Ranks 7th

**Despite 0 fix commits and fast completion:**
```
Compliance: 13/16 (81.25%) × 50 = 40.63
Tests: min(100, 10×1.5)=15 × 30 = 4.50  [Only ~10 effective tests!]
Quality: (100 - 0 - 37) × 15 = 9.45  [37pt penalty for 84% skip ratio]
Efficiency: (100 - 61.65) × 5 = 1.92
```

---

## Detailed Metrics Comparison

| Metric | Beads v3 | Hive v1 | RuVector | Beads v2 | Swarm v2 | Beads v1 | Hive v2 | Swarm v1 |
|--------|----------|---------|----------|----------|----------|----------|---------|----------|
| **Pattern** | Beads | Hive | Hive+RuVector | Beads | Swarm | Beads | Hive | Swarm |
| **Spec Compliance** | 16/16 | 15/16 | 16/16 | 14/16 | 10/16 | 12/16 | 13/16 | 14/16 |
| **Effective Tests** | 59 | 64 | 61 | 18 | ~37 | 18 | ~10 | 15 |
| **Skip Ratio** | 20% | 0% | 0% | 0% | ~3% | 0% | **84%** | 0% |
| **Lines of Code** | 4,947 | 3,545 | 8,751 | 3,511 | 4,227 | 1,826 | 6,165 | 8,683 |
| **Fix Commits** | 0 | 1 | 1 | 0 | 0 | 0 | 0 | 7 |
| **Duration** | ~23m | ~41m | ~2h 18m | ~14m | ~1h 54m | ~11m | ~37m | ~1h 49m |
| **Real Data** | Yes | Yes | Yes | Yes | Yes | Simulated | Yes | Yes |
| **Open Issues** | 1 | 2 | 1 | 3 | 5 | 7 | 6 | 4 |

---

## Pattern Performance

| Pattern | Attempts | Avg Score | Best Rank | Avg Effective Tests | Notes |
|---------|----------|-----------|-----------|---------------------|-------|
| **Beads** | 3 | 76.0 | 1st | 32 | Most consistent pattern |
| **Hive** | 2 | 74.5 | 2nd | 37 | High variance (v1 excellent, v2 inflated) |
| **Swarm** | 2 | 60.7 | 5th | 26 | Lower compliance, high fix counts |
| **RuVector** | 1 | 91.6 | 3rd | 61 | Innovative architecture |

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

- **Best pattern:** Beads (highest avg score: 76.0)
- **Most efficient:** Beads v1 (lowest LOC: 1,826)
- **Most tests:** Hive v1 (64 effective tests)
- **Fastest:** Beads v1 (~11 min autonomous)
- **Worst skip ratio:** Hive v2 (84% - tests never implemented)

### Common Strengths (Top 3)
- Full or near-full spec compliance
- Zero or one fix commits (clean implementations)
- Real Kaggle data integration
- Comprehensive BDD test coverage

### Areas for Improvement
- **Hive v2:** Tests were created but functionality never implemented
- **Swarm v1:** High fix commit count (7) indicates iteration issues
- **Swarm v2:** Low compliance (10/16) despite adequate test count

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
| 2025-12-14-python-claude-beads-2 | 1 | 0 | 1 | Test Quality |
| 2025-10-30-python-hive | 2 | 0 | 2 | Missing, Compliance |
| 2025-12-15-python-claude-ruvector | 1 | 0 | 1 | Test Quality |
| 2025-12-14-python-claude-beads | 3 | 0 | 3 | Missing, Compliance |
| 2025-12-13-python-claude-swarm | 5 | 0 | 5 | Missing, Docs, Compliance |
| 2025-12-01-python-claude-beads | 7 | 0 | 7 | Missing, Test Quality, Docs, Compliance |
| 2025-12-13-python-claude-hive | 6 | 0 | 6 | Missing, Test Quality, Compliance |
| 2025-09-30-python-swarm | 4 | 0 | 4 | Missing, Test Quality, Compliance |

**Total Open Issues:** 29

### Issue Distribution by Type

| Issue Type | Count | Description |
|------------|-------|-------------|
| `[Missing]` | ~12 | Missing spec requirements |
| `[Test Quality]` | ~8 | Skipped tests, best practices |
| `[Compliance]` | ~7 | Summary issues linking requirements |
| `[Docs]` | ~2 | README documentation gaps |

---

Generated by compare-attempts SOP on 2025-01-04 (v3 - Effective Tests with Skip Penalty)
