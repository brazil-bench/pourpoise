# Evaluate Benchmark Attempt

## Overview
This SOP evaluates a completed brazil-bench attempt against the spec.md requirements,
capturing metrics for comparison across orchestration patterns.

## Parameters
- **attempt_repo** (required): Repository name (e.g., `attempt-3`)
- **output_dir** (optional, default: `./results`): Where to write evaluation results

## Steps

### 1. Clone Attempt
Fetch the attempt repository for local analysis.

**Constraints:**
- You MUST clone into `./reviews/{attempt_repo}`
- You MUST verify the clone succeeded before proceeding
- You MUST NOT modify any files in the cloned repo
```bash
gh repo clone brazil-bench/{attempt_repo} ./reviews/{attempt_repo}
```

### 2. Verify Spec Integrity
Confirm the spec.md was not modified from the template.

**Constraints:**
- You MUST compare `spec.md` against the template version
- You MUST fail the evaluation if spec.md was modified
- You SHOULD use a checksum comparison
```bash
gh repo clone brazil-bench/benchmark-template ./reviews/_template --depth 1
diff ./reviews/{attempt_repo}/spec.md ./reviews/_template/spec.md
```

### 3. Run Conformance Tests
Execute the test suite defined in the spec against the implementation.

**Constraints:**
- You MUST run all tests specified in spec.md
- You MUST capture pass/fail counts and output
- You SHOULD timeout tests after 60 seconds each
- You MAY retry flaky tests once

### 4. Measure Code Metrics
Collect quantitative data about the implementation.

**Constraints:**
- You MUST capture: total lines of code, number of files, dependencies
- You SHOULD capture: cyclomatic complexity, test coverage
- You MAY capture: documentation coverage, type hint coverage
```bash
# Lines of code (excluding tests)
find ./reviews/{attempt_repo}/src -name "*.py" | xargs wc -l

# Dependencies
cat ./reviews/{attempt_repo}/pyproject.toml | grep dependencies -A 50
```

### 5. Extract Git Metrics
Analyze the development history for benchmark comparison.

**Constraints:**
- You MUST capture: total commits, time from first to last commit
- You SHOULD capture: number of reverts, force pushes (if detectable)
- You SHOULD extract commit messages mentioning "fix", "revert", "oops"
```bash
cd ./reviews/{attempt_repo}
git log --oneline | wc -l
git log --format="%H %s" | grep -iE "(fix|revert|oops|wrong)"
```

### 6. Analyze Against Spec
Review implementation completeness against spec.md requirements.

**Constraints:**
- You MUST enumerate each requirement in spec.md
- You MUST assess each as: implemented, partial, missing
- You SHOULD note implementation approach for each
- You MUST NOT make subjective quality judgments beyond spec compliance

### 7. Generate Codebase Documentation
Generate comprehensive documentation for the implementation using the codebase-summary SOP.

**Constraints:**
- You MUST run the codebase-summary SOP on the cloned repository
- You MUST output documentation to `{output_dir}/{attempt_repo}-summary/`
- You SHOULD use the generated documentation to inform the final report
- The documentation provides architecture, components, interfaces, and workflow analysis

```
/strands-agents-sops:codebase-summary codebase-path: reviews/{attempt_repo} output_dir: {output_dir}/{attempt_repo}-summary
```

### 8. Generate Report
Produce structured evaluation output.

**Constraints:**
- You MUST write results to `{output_dir}/{attempt_repo}.md`
- You MUST include: attempt name, orchestration pattern, all metrics
- You MUST use consistent format for cross-attempt comparison
- You SHOULD include raw data as appendix

## Output Format
```markdown
# Evaluation: {attempt_repo}

## Summary
- **Pattern:** [swarm|hive|solo|...]
- **Spec Compliance:** X/Y requirements
- **Tests:** X passed, Y failed
- **Documentation:** See `{attempt_repo}-summary/`

## Metrics
| Metric | Value |
|--------|-------|
| Lines of Code | |
| Files | |
| Dependencies | |
| Commits | |
| Duration | |

## Requirements Checklist
- [x] Requirement 1
- [ ] Requirement 2 (partial: notes)
- [ ] Requirement 3 (missing)

## Architecture Summary
(Key insights from generated codebase documentation)

## Git Analysis
...

## Raw Data
...
```

## Troubleshooting

**Clone fails**
- Verify repo exists: `gh repo view brazil-bench/{attempt_repo}`
- Check permissions: repo must be public or you need access

**Tests won't run**
- Check for missing dependencies
- Verify test runner is installed
- Review spec.md for test prerequisites

**Spec diff shows changes**
- Fail the evaluation
- Note the changes in the report
- This invalidates the benchmark comparison

**Codebase documentation fails**
- Verify the strands-agents-sops MCP server is configured
- Check that the codebase-path exists and contains code
- Ensure the output directory is writable
- Try running the SOP standalone first to debug