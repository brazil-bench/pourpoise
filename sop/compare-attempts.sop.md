# Compare Benchmark Attempts

## Overview
This SOP compares evaluated brazil-bench attempts across multiple dimensions to produce
a ranked leaderboard and detailed comparison summary. It supports up to 10 attempts
in the "Top 10" format, automatically pruning lower-ranked entries when more are added.

## Parameters
- **results_dir** (optional, default: `./results`): Directory containing evaluation reports
- **output_file** (optional, default: `./results/LEADERBOARD.md`): Output comparison file
- **max_entries** (optional, default: 10): Maximum entries in leaderboard

## Steps

### 1. Discover Evaluation Results
Find all completed evaluation reports in the results directory.

**Constraints:**
- You MUST scan for `*.md` files in the results directory (excluding LEADERBOARD.md)
- You MUST verify each file is a valid evaluation report (has "# Evaluation:" header)
- You MUST extract the attempt name from each report
- You SHOULD skip any malformed or incomplete reports
```bash
ls {results_dir}/*.md | grep -v LEADERBOARD.md
```

### 2. Extract Metrics from Each Report
Parse each evaluation report to extract comparable metrics.

**Constraints:**
- You MUST extract all metrics from the Metrics table in each report
- You MUST extract the pattern/orchestration type
- You MUST extract spec compliance (X/Y format)
- You MUST extract test counts if available
- You MUST extract git metrics (commits, duration, fix commits)
- You SHOULD normalize duration to hours for comparison
- You MUST handle missing metrics gracefully (mark as "N/A")

**Required Metrics:**
| Metric | Source | Notes |
|--------|--------|-------|
| Pattern | Summary section | swarm, hive, solo, crew, custom |
| Spec Compliance | Summary or checklist | Count of implemented requirements |
| Lines of Code | Metrics table | Source code only |
| Files | Metrics table | Python files in src |
| Dependencies | Metrics table | Package count |
| Commits | Metrics table | Total git commits |
| Duration | Metrics table | Development time |
| Fix Commits | Git Analysis | Commits with "fix" in message |
| Test Scenarios | Test Summary | BDD or unit test count |

### 3. Calculate Ranking Score
Compute a composite score for ranking attempts.

**Constraints:**
- You MUST use a weighted scoring formula
- You MUST prioritize spec compliance (highest weight)
- You SHOULD penalize excessive code/complexity
- You SHOULD reward fewer fix commits (cleaner development)
- You MUST document the scoring formula used

**Suggested Scoring Formula:**
```
Score = (Spec Compliance % × 40)
      + (Test Coverage Score × 20)
      + (Efficiency Score × 20)
      + (Code Quality Score × 20)

Where:
- Spec Compliance % = (implemented / total) × 100
- Test Coverage Score = min(100, test_scenarios × 1.5)
- Efficiency Score = 100 - min(100, (LOC / 100))  # Penalize bloat
- Code Quality Score = 100 - (fix_commits × 10)   # Penalize fixes
```

### 4. Rank and Sort Attempts
Order attempts by composite score.

**Constraints:**
- You MUST sort by score descending (highest first)
- You MUST handle ties by using secondary sort (fewer LOC wins)
- You MUST limit to top {max_entries} entries
- You MUST assign rank numbers (1, 2, 3, ...)
- You SHOULD note if entries were pruned

### 5. Generate Comparison Tables
Create detailed comparison tables for the report.

**Constraints:**
- You MUST create a summary leaderboard table
- You MUST create a detailed metrics comparison table
- You MUST create a pattern distribution summary
- You SHOULD create a requirements coverage matrix
- You SHOULD include trend indicators if historical data exists

### 6. Generate Analysis
Provide insights from the comparison.

**Constraints:**
- You MUST identify the winning attempt and why
- You MUST note patterns that performed better/worse
- You SHOULD identify common strengths across top performers
- You SHOULD identify areas where all attempts struggled
- You MUST NOT make subjective quality judgments beyond metrics

### 7. Write Comparison Report
Output the final comparison document.

**Constraints:**
- You MUST write to {output_file}
- You MUST use the output format specified below
- You MUST include generation timestamp
- You MUST include source file references
- You SHOULD include methodology notes

## Output Format

```markdown
# Brazil-Bench Leaderboard

> Last updated: {timestamp}
> Attempts evaluated: {count}

## 🏆 Top 10 Leaderboard

| Rank | Attempt | Pattern | Score | Spec | LOC | Tests | Duration |
|------|---------|---------|-------|------|-----|-------|----------|
| 1 🥇 | {name} | {pattern} | {score} | {X/Y} | {loc} | {tests} | {hours}h |
| 2 🥈 | {name} | {pattern} | {score} | {X/Y} | {loc} | {tests} | {hours}h |
| 3 🥉 | {name} | {pattern} | {score} | {X/Y} | {loc} | {tests} | {hours}h |
| 4 | ... | ... | ... | ... | ... | ... | ... |

## Detailed Metrics Comparison

| Metric | {attempt1} | {attempt2} | ... | Best |
|--------|------------|------------|-----|------|
| Pattern | {val} | {val} | ... | - |
| Spec Compliance | {X/Y} | {X/Y} | ... | {winner} |
| Lines of Code | {loc} | {loc} | ... | {winner} |
| Python Files | {n} | {n} | ... | {winner} |
| Dependencies | {n} | {n} | ... | {winner} |
| Commits | {n} | {n} | ... | {winner} |
| Duration | {h}h | {h}h | ... | {winner} |
| Fix Commits | {n} | {n} | ... | {winner} |
| Test Scenarios | {n} | {n} | ... | {winner} |

## Pattern Performance

| Pattern | Attempts | Avg Score | Best Rank | Avg LOC | Avg Duration |
|---------|----------|-----------|-----------|---------|--------------|
| Hive | {n} | {score} | {rank} | {loc} | {hours}h |
| Swarm | {n} | {score} | {rank} | {loc} | {hours}h |
| Solo | {n} | {score} | {rank} | {loc} | {hours}h |
| Crew | {n} | {score} | {rank} | {loc} | {hours}h |

## Requirements Coverage Matrix

| Requirement | {attempt1} | {attempt2} | ... | Coverage |
|-------------|------------|------------|-----|----------|
| search_player | ✅ | ✅ | ... | 100% |
| get_player_stats | ✅ | ✅ | ... | 100% |
| ... | ... | ... | ... | ... |

## Analysis

### Winner: {attempt_name}
{Brief explanation of why this attempt ranked first}

### Pattern Insights
- **Best performing pattern:** {pattern} (avg score: {score})
- **Most efficient pattern:** {pattern} (lowest avg LOC: {loc})
- **Fastest pattern:** {pattern} (avg duration: {hours}h)

### Common Strengths
- {observation 1}
- {observation 2}

### Areas for Improvement
- {observation 1}
- {observation 2}

## Methodology

### Scoring Formula
```
Score = (Spec Compliance % × 40) + (Test Score × 20) + (Efficiency × 20) + (Quality × 20)
```

### Data Sources
- {attempt1}: results/{attempt1}.md
- {attempt2}: results/{attempt2}.md
- ...

---
Generated by compare-attempts SOP on {timestamp}
```

## Troubleshooting

**No evaluation reports found**
- Verify results_dir path is correct
- Run evaluate-attempt SOP first for each attempt
- Check file permissions

**Missing metrics in report**
- Re-run evaluate-attempt SOP for that attempt
- Check if the attempt used a different report format
- Mark as "N/A" and note in analysis

**Scoring seems unfair**
- Review the scoring formula weights
- Consider adjusting for specific benchmark goals
- Document any formula modifications

**Too many attempts (>10)**
- Only top 10 are shown in leaderboard
- Full data preserved in detailed comparison
- Consider archiving older/lower-ranked attempts
