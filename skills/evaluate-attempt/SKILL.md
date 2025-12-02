---
name: evaluate-attempt
description: This SOP evaluates a completed brazil-bench attempt against the spec.md requirements, capturing metrics for comparison across orchestration patterns.
type: anthropic-skill
version: "1.0"
---

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
- You MUST attempt to run all tests specified in spec.md
- You MUST capture pass/fail counts and output
- You SHOULD timeout tests after 60 seconds each
- You MAY retry flaky tests once
- If tests fail due to missing dependencies, follow the dependency resolution steps below

#### 3a. Handle Missing Dependencies (Neo4j, etc.)

If tests fail due to missing external dependencies like Neo4j:

**Step 1: Try to start the dependency via Docker**
```bash
# Check if Docker is available
docker --version

# Check for docker-compose files in the repo
ls ./reviews/{attempt_repo}/docker-compose*.yml

# If Neo4j docker-compose exists, start it
docker-compose -f ./reviews/{attempt_repo}/docker-compose.neo4j.yml up -d

# Or start Neo4j directly
docker run -d --name neo4j-eval -p 7474:7474 -p 7687:7687 \
  -e NEO4J_AUTH=neo4j/password \
  neo4j:5

# Wait for Neo4j to be ready
sleep 10
docker logs neo4j-eval 2>&1 | tail -5
```

**Step 2: If Docker unavailable or fails, look for evidence of prior test runs**

Check these sources for test results:
```bash
# Check git history for test-related commits
git log --oneline --all | grep -iE "(test|pass|100%|fix.*test)"

# Check for CI/CD logs or badges
cat ./reviews/{attempt_repo}/README.md | grep -iE "(pass|badge|ci|test)"

# Check prompts.txt for test execution evidence
cat ./reviews/{attempt_repo}/prompts.txt 2>/dev/null | grep -iE "(pytest|test|pass|fail|scenario)"

# Check for pytest cache with results
ls -la ./reviews/{attempt_repo}/.pytest_cache/ 2>/dev/null

# Check for coverage reports
ls -la ./reviews/{attempt_repo}/htmlcov/ ./reviews/{attempt_repo}/coverage.xml 2>/dev/null
```

**Step 3: Document findings in the report**

If tests cannot be run directly, document:
- Why tests couldn't run (missing Neo4j, etc.)
- Evidence found of prior test runs (commit messages, prompts.txt entries)
- Claimed test results from the attempt's documentation
- Mark as "CANNOT VERIFY" with explanation

**Constraints for dependency handling:**
- You MUST try Docker first if available
- You MUST search for evidence if Docker fails
- You MUST NOT claim tests pass without verification
- You SHOULD note the source of any claimed test results
- You SHOULD clean up Docker containers after evaluation: `docker stop neo4j-eval && docker rm neo4j-eval`

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

### 5. Extract Git Metrics and Analyze Development Timeline
Analyze the development history to separate agent-driven work from human interactions.

**Constraints:**
- You MUST capture: total commits, time from first to last commit
- You MUST separate commits into agent-driven vs human-driven phases
- You MUST calculate autonomous duration (agent work only)
- You SHOULD capture: number of reverts, force pushes (if detectable)
- You SHOULD extract commit messages mentioning "fix", "revert", "oops"

#### 5a. Gather Raw Git Data
```bash
cd ./reviews/{attempt_repo}

# Full commit history with timestamps and messages
git log --format="%ai | %H | %s" --reverse

# Count total commits
git log --oneline | wc -l

# Find fix/revert commits
git log --format="%H %s" | grep -iE "(fix|revert|oops|wrong)"

# Get first and last commit times
git log --format="%ai" --reverse | head -1  # First commit
git log --format="%ai" | head -1             # Last commit
```

#### 5b. Identify Development Phases

Analyze commit timestamps and messages to identify distinct phases:

**Phase 1: Setup (Human)**
- Initial commit, repo setup, file uploads
- Typically first 1-3 commits before implementation starts
- Look for: "Initial commit", "Add files", "upload", "setup"

**Phase 2: Agent Implementation**
- Bulk implementation work by the agent
- Characterized by:
  - Rapid succession of commits (minutes apart)
  - Large code changes
  - Messages like "Implement", "Add", "Create"
  - Consistent commit patterns (same author, similar timing)

**Phase 3: Agent Test Iteration**
- Test fixing and iteration by the agent
- Characterized by:
  - Commits mentioning "fix", "test", "pass"
  - Still rapid succession
  - Often shows progression: "Fix X" → "Fix Y" → "100% pass"

**Phase 4: Human Intervention (Post-Completion)**
- Human-driven changes after agent work completes
- Characterized by:
  - Time gaps (hours/days after previous commits)
  - Different commit patterns or author info
  - Messages about data, documentation, cleanup
  - Changes not required by the spec

#### 5c. Heuristics for Identifying Agent vs Human Commits

**Agent commits typically show:**
- Timestamps within minutes of each other
- Consistent formatting in commit messages
- Co-authored-by lines mentioning Claude/AI
- Large, comprehensive changes
- Focus on implementation and tests

**Human commits typically show:**
- Time gaps of hours or days from previous work
- Different commit message style
- Focus on data, docs, or polish
- Smaller, targeted changes
- Work done after "100% tests pass" milestone

```bash
# Look for time gaps > 1 hour between commits (potential phase boundaries)
git log --format="%ai" --reverse | while read ts; do echo "$ts"; done

# Check for co-author lines indicating AI
git log --format="%b" | grep -i "co-authored"

# Check prompts.txt for session boundaries
cat prompts.txt 2>/dev/null | grep -E "^(Done|Session|Agent)"
```

#### 5d. Calculate Duration Metrics

| Metric | How to Calculate |
|--------|------------------|
| **Total Duration** | Last commit - First commit |
| **Agent Duration** | Sum of time during agent phases only |
| **Human Duration** | Sum of time during human phases |
| **Autonomous Duration** | Phase 2 + Phase 3 (implementation + test fixing) |

**Example Timeline Analysis:**
```
09:00:00 - Initial commit (Human Setup)
09:05:00 - Add spec file (Human Setup)
         --- Agent work begins ---
09:15:00 - Implement Phase 1 (Agent)
09:45:00 - Implement Phase 2 (Agent)
10:10:00 - Implement Phase 3 (Agent)
10:25:00 - Fix test issues (Agent)
10:40:00 - 100% tests pass (Agent)
         --- Agent work ends ---
         --- 2 day gap ---
Oct 3     - Add real data (Human)
Oct 3     - Update docs (Human)

Agent Duration: ~1h 25m (09:15 → 10:40)
Human Duration: ~5m setup + later changes
Autonomous Duration: ~1h 25m
```

#### 5e. Document in Report

Include a Development Timeline section in the report:

```markdown
## Development Duration Breakdown

| Phase | Duration | Description |
|-------|----------|-------------|
| **Setup (Human)** | ~5 min | Initial commit, file upload |
| **Phase 1: Implementation** | ~55 min | Agent implements all phases |
| **Phase 2: Test Fixing** | ~30 min | Agent iterates to 100% pass |
| **Total Autonomous** | **~1h 25m** | Agent work only |
| **Phase 3: Human Intervention** | 2 days later | Data and docs added |

### Commit Analysis
- Total commits: 15
- Agent commits: 10 (09:15 - 10:40 on Day 1)
- Human commits: 5 (setup + Day 3 changes)
- Fix commits: 3 (normal iteration, not rework)
```

### 6. Analyze Against Spec
Review implementation completeness against spec.md requirements.

**Constraints:**
- You MUST enumerate each requirement in spec.md
- You MUST assess each as: implemented, partial, missing
- You SHOULD note implementation approach for each
- You MUST NOT make subjective quality judgments beyond spec compliance

#### 6a. Real Data vs Simulated Data Assessment

Determine whether the implementation uses real external data or simulated/mock data.

**Real Data Indicators:**
- Data loaders for external sources (Kaggle, APIs, etc.)
- CSV/JSON files in data directory
- API client code with authentication
- Data normalization/mapping logic for external schemas

**Simulated Data Indicators:**
- Hardcoded test fixtures
- Factory/faker-generated data
- Mock data in test files only
- No external data loading code

**Constraints for Real Data Implementations:**
- You MUST note which external data source is used
- You MUST assess schema mapping quality (how well does the implementation adapt external schema to spec schema)
- You MUST distinguish between:
  - **Schema Implemented**: The code defines models matching spec entities
  - **Data Populated**: The data loader can populate those fields from external source
  - **Not Available in Source**: Spec field cannot be populated because external data doesn't include it
- You SHOULD credit implementations that adapt to real-world data constraints
- You SHOULD note any enhancements beyond spec (e.g., additional fields from richer data sources)

**Adjusted Compliance Scoring:**
- If real data is used and a spec field is "Not Available in Source", count it as:
  - **Implemented** if the model/schema supports the field
  - Note the data limitation separately
- Example: If spec requires "attendance" but Kaggle data has no attendance:
  - Check if Match model has attendance field (schema compliance)
  - Note that field would be null with Kaggle data (data limitation)
  - This is NOT a failure - it's a data source constraint

### 7. Generate Codebase Documentation
Generate comprehensive documentation for the implementation using the codebase-summary SOP.

**Constraints:**
- You MUST run the codebase-summary skill on the cloned repository
- You MUST output documentation to `{output_dir}/{attempt_repo}-summary/`
- You SHOULD use the generated documentation to inform the final report
- The documentation provides architecture, components, interfaces, and workflow analysis

```
summarize codebase reviews/{attempt_repo} to {output_dir}/{attempt_repo}-summary/
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
- **Autonomous Duration:** Xh Ym
- **Documentation:** See `{attempt_repo}-summary/`

## Metrics
| Metric | Value |
|--------|-------|
| Lines of Code | |
| Files | |
| Dependencies | |
| Commits (Total) | |
| Commits (Agent) | |
| Commits (Human) | |
| Fix Commits | |

## Development Duration Breakdown

| Phase | Duration | Description |
|-------|----------|-------------|
| **Setup (Human)** | | Initial commit, file upload |
| **Agent Implementation** | | Core implementation work |
| **Agent Test Iteration** | | Test fixing to 100% pass |
| **Total Autonomous** | | Agent work only |
| **Human Intervention** | | Post-completion changes |

### Timeline
```
{timestamp} - {commit message} ({phase})
...
```

### Commit Analysis
- Total commits: X
- Agent commits: X (timespan)
- Human commits: X (description)
- Fix commits: X (context: normal iteration vs rework)

## Requirements Checklist
- [x] Requirement 1
- [ ] Requirement 2 (partial: notes)
- [ ] Requirement 3 (missing)

## Architecture Summary
(Key insights from generated codebase documentation)

## Raw Data
...
```

## Troubleshooting

**Clone fails**
- Verify repo exists: `gh repo view brazil-bench/{attempt_repo}`
- Check permissions: repo must be public or you need access

**Tests won't run due to missing dependencies**
- Try starting Neo4j via Docker (see Step 3a above)
- If Docker unavailable, search for evidence of prior test runs
- Check git commits for "100% pass" or similar messages
- Check prompts.txt for pytest output
- Document as "CANNOT VERIFY" with evidence found

**Neo4j connection errors**
- Verify Neo4j is running: `docker ps | grep neo4j`
- Check credentials match: NEO4J_AUTH=neo4j/password
- Wait for startup: Neo4j needs ~10-15 seconds to initialize
- Check logs: `docker logs neo4j-eval`

**Spec diff shows changes**
- Fail the evaluation
- Note the changes in the report
- This invalidates the benchmark comparison

**Codebase documentation fails**
- Verify the codebase-summary skill is available
- Check that the codebase-path exists and contains code
- Ensure the output directory is writable
- Try running the skill standalone first to debug