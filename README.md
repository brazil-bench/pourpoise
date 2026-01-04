# pourpoise

Purpose management for this organization

The purpose of this organization is to generate benchmark results that compare approaches to agent development management on the same task.

The standard task is to create an MCP server for a brazilian football statistics knowledge graph, and every benchmark starts with an empty repo that contains the spec, which is created from the benchmark-template repo provided here. The spec works well enough, but has some inconsistencies with the provided sample data, which are deliberately left in the task to see how the agent deals with the ambiguity. The time taken is approximate as it's estimated based on the github check-in times, and is a high side estimate, as the run doesn't always start immediately after a check-in.

A secondary purpose is to explore and develop mechanisms that automate the development management of agents in a purposeful way.

## Current Leaderboard

> 8 attempts evaluated as of 2025-12-16
> Methodology: Effective Tests with Skip Penalty (v3)

| Rank | Attempt | Pattern | Score | Compliance | Effective Tests | Skip% | Duration | Issues |
|------|---------|---------|-------|------------|-----------------|-------|----------|--------|
| 🥇 | 2025-12-14-python-claude-beads-2 | Beads v3 | **93.3** | 16/16 | 59 | 20% | ~23m | 1 open |
| 🥈 | 2025-10-30-python-hive | Hive v1 | **92.4** | 15/16 | 64 | 0% | ~41m | 2 open |
| 🥉 | 2025-12-15-python-claude-ruvector | RuVector | **91.6** | 16/16 | 61 | 0% | ~2h 18m | 0 |
| 4 | 2025-12-01-python-claude-beads | Beads v1 | **77.0** | 16/16 | 18 | 0% | ~11m | 0 |
| 5 | 2025-12-14-python-claude-beads | Beads v2 | 70.1 | 14/16 | 18 | 0% | ~14m | 3 open |
| 6 | 2025-12-13-python-claude-swarm | Swarm v2 | 65.8 | 10/16 | 37 | 3% | ~1h 54m | 5 open |
| 7 | 2025-12-13-python-claude-hive | Hive v2 | 56.5 | 13/16 | ~10 | **84%** | ~37m | 6 open |
| 8 | 2025-09-30-python-swarm | Swarm v1 | 55.7 | 14/16 | 15 | 0% | ~1h 49m | 4 open |

See [results/LEADERBOARD.md](results/LEADERBOARD.md) for detailed analysis.

### Key Insights

- **Best Overall:** Beads v3 (100% compliance, 59 effective tests, 0 fix commits)
- **Most Tests:** Hive v1 (64 effective BDD tests with 0% skip ratio)
- **Full Compliance:** Beads v3, RuVector, and Beads v1 all achieve 16/16
- **Fastest:** Beads v1 (~11 min autonomous)
- **First Zero Issues:** Beads v1 (all 7 issues closed, now 0 open)
- **Innovation:** RuVector uses vector DB instead of Neo4j
- **Warning:** Hive v2 has 84% skip ratio (53 tests never run) - tests were stubs

## Setup

### Prerequisites

```bash
# Install Claude-code
npm install -g @anthropic-ai/claude-code

Install GitHub CLI and authenticate:
# Install GitHub CLI (needed on macOS, but it's better to run on Codespaces that doesn't need gh setup)
brew install gh

# Authenticate with GitHub
gh auth login
```

### Python Environment

```bash
# Create virtual environment
python3 -m venv venv

# Activate virtual environment
source venv/bin/activate  # On macOS/Linux
# venv\Scripts\activate   # On Windows

# Install dependencies
pip install -r requirements.txt
```

### Skills

This repository includes skills in the `skills/` directory that provide Standard Operating Procedures for benchmark management. Available skills:

| Skill | Description | Example |
|-------|-------------|---------|
| `evaluate-attempt` | Evaluate a completed attempt | "evaluate attempt 2025-10-30-python-hive" |
| `compare-attempts` | Generate leaderboard comparing all attempts | "compare attempts" |
| `file-issues` | File GitHub issues for shortcomings found in evaluation | "file issues for 2025-12-13-python-claude-hive" |
| `re-evaluate` | Re-evaluate after issues are fixed | "re-evaluate 2025-12-13-python-claude-hive" |
| `codebase-summary` | Generate documentation for a codebase | "summarize codebase reviews/attempt-name" |
| `pdd` | Prompt-Driven Development planning | "help me design a new feature" |
| `code-task-generator` | Generate code task files | "generate tasks from my PDD plan" |
| `code-assist` | Assist with code implementation | "help me implement this task" |

Skills are invoked using natural language in Claude Code.

## SOP Workflow

The benchmark evaluation system follows a structured workflow with four main SOPs that work together to evaluate, track, and improve attempt repositories.

### Workflow Diagram

```
┌─────────────────────────────────────────────────────────────────────────────┐
│                         BENCHMARK EVALUATION LIFECYCLE                       │
└─────────────────────────────────────────────────────────────────────────────┘

  ┌──────────────┐     ┌──────────────┐     ┌──────────────┐     ┌──────────────┐
  │   EVALUATE   │────▶│    FILE      │────▶│     FIX      │────▶│ RE-EVALUATE  │
  │   ATTEMPT    │     │   ISSUES     │     │   ISSUES     │     │              │
  └──────────────┘     └──────────────┘     └──────────────┘     └──────────────┘
         │                    │                    │                    │
         ▼                    ▼                    ▼                    ▼
  ┌──────────────┐     ┌──────────────┐     ┌──────────────┐     ┌──────────────┐
  │ results/     │     │ GitHub       │     │ Attempt      │     │ Updated      │
  │ {repo}.md    │     │ Issues       │     │ Repository   │     │ Report       │
  └──────────────┘     └──────────────┘     └──────────────┘     └──────────────┘
         │                                                              │
         └──────────────────────┬───────────────────────────────────────┘
                                ▼
                         ┌──────────────┐
                         │   COMPARE    │
                         │   ATTEMPTS   │
                         └──────────────┘
                                │
                                ▼
                         ┌──────────────┐
                         │ LEADERBOARD  │
                         │   + README   │
                         └──────────────┘
```

### SOP Descriptions

#### 1. Evaluate Attempt (`evaluate-attempt`)

**Purpose:** Initial evaluation of a completed benchmark attempt against spec requirements.

**Inputs:**
- Attempt repository name (e.g., `2025-12-13-python-claude-hive`)

**Outputs:**
- `results/{attempt_repo}.md` - Detailed evaluation report
- `results/{attempt_repo}-summary/` - Generated codebase documentation

**What it checks:**
| Category | Checks |
|----------|--------|
| Spec Integrity | Verifies spec.md wasn't modified |
| Test Results | Runs tests, captures pass/fail/skip counts |
| Skip Analysis | Detects inflated test counts from `pytest.skip()` |
| Code Metrics | LOC, files, dependencies, complexity |
| Git Timeline | Separates agent vs human commits |
| Spec Compliance | Assesses each requirement (16 total) |
| Documentation | README quality (setup, MCP, examples) |
| Data Strategy | Real Kaggle data vs simulated |

**Example:**
```bash
evaluate attempt 2025-12-13-python-claude-hive
```

---

#### 2. File Issues (`file-issues`)

**Purpose:** Parse evaluation reports and create GitHub issues for shortcomings.

**Inputs:**
- Attempt repository name
- Evaluation report (auto-detected at `results/{repo}.md`)

**Outputs:**
- GitHub issues on `brazil-bench/{attempt_repo}`

**Issue Types Created:**

| Issue Type | Title Prefix | Label | Trigger |
|------------|--------------|-------|---------|
| Missing Requirement | `[Missing]` | `enhancement` | Unchecked spec items |
| Test Quality | `[Test Quality]` | `bug` or `enhancement` | Skip ratio >10%, bad patterns |
| Documentation | `[Docs]` | `documentation` | Missing README elements |
| Quality | `[Quality]` | `enhancement` | Architecture concerns |
| Compliance Summary | `[Compliance]` | `enhancement` | Links all issues together |

**Example:**
```bash
file issues for 2025-12-13-python-claude-hive
```

**Dry run (preview without creating):**
```bash
file issues for 2025-12-13-python-claude-hive --dry-run
```

---

#### 3. Re-Evaluate (`re-evaluate`)

**Purpose:** After issues are fixed, re-evaluate the attempt and update reports.

**Inputs:**
- Attempt repository name
- Optional: `--full-reeval` for complete re-evaluation

**Outputs:**
- Updated `results/{attempt_repo}.md` with Re-Evaluation History
- New issues for any regressions found
- Updated leaderboard if rank changes

**What it does:**

| Step | Description |
|------|-------------|
| 1. Check Changes | Pull latest commits from attempt repo |
| 2. Identify Closed Issues | List issues closed since last evaluation |
| 3. Targeted Re-Eval | Re-check only affected areas |
| 4. Update Report | Add metrics diff and re-evaluation history |
| 5. Recalculate Score | Apply scoring formula with new metrics |
| 6. File Regressions | Create `[Regression]` issues if problems found |

**Example:**
```bash
re-evaluate 2025-12-13-python-claude-hive
```

---

#### 4. Compare Attempts (`compare-attempts`)

**Purpose:** Generate ranked leaderboard from all evaluation reports.

**Inputs:**
- All reports in `results/*.md`

**Outputs:**
- `results/LEADERBOARD.md` - Detailed comparison
- Updated README.md leaderboard table

**Scoring Formula:**
```
Score = (Spec % × 50) + (Tests × 30) + (Quality × 15) + (Efficiency × 5)

Skip Penalty = max(0, (skip_ratio - 0.10) × 50)
```

**Example:**
```bash
compare attempts
```

---

### Complete Workflow Example

Here's a typical workflow for evaluating and improving an attempt:

```bash
# Step 1: Initial evaluation
evaluate attempt 2025-12-13-python-claude-hive
# Creates: results/2025-12-13-python-claude-hive.md

# Step 2: File issues for shortcomings
file issues for 2025-12-13-python-claude-hive
# Creates: GitHub issues on brazil-bench/2025-12-13-python-claude-hive

# Step 3: Fix issues in the attempt repo
cd reviews/2025-12-13-python-claude-hive
claude "Fix all open issues. Run: gh issue list to see them."

# Step 4: Re-evaluate after fixes
re-evaluate 2025-12-13-python-claude-hive
# Updates: results/2025-12-13-python-claude-hive.md

# Step 5: Update leaderboard
compare attempts
# Updates: results/LEADERBOARD.md and README.md
```

### Fixing Issues in Attempt Repos

When running Claude in each attempt repo to fix issues, use these prompts:

**For Missing Requirements:**
```
Fix GitHub issue #N - [Missing] {requirement}
Read the issue: gh issue view N
Implement the missing requirement following the spec.
Run tests after implementation.
```

**For Test Quality:**
```
Fix GitHub issue #N - skipped tests
Re-run tests and either:
1. Implement missing functionality so tests pass
2. Remove stub tests that can't run
3. Add proper skip markers with clear reasons
```

**For Documentation:**
```
Fix GitHub issue #N - README documentation
Update README.md to include:
1. Setup Instructions (prerequisites, installation)
2. MCP Server Setup (how to start, connect Claude)
3. Example Q&A (sample questions and responses)
```

**One-liner for all issues:**
```
Fix all open issues in this repo. Run: gh issue list to see them.
Address each one, run tests, and commit when done.
```

### Automation

**Check all repos for closed issues:**
```bash
for repo in $(gh repo list brazil-bench --json name -q '.[].name' | grep -E "^20[0-9]{2}-"); do
  closed=$(gh issue list -R brazil-bench/$repo --state closed --json number -q 'length')
  open=$(gh issue list -R brazil-bench/$repo --state open --json number -q 'length')
  if [ "$closed" -gt 0 ] || [ "$open" -gt 0 ]; then
    echo "$repo: $open open, $closed closed"
  fi
done
```

**Batch re-evaluate all repos with closed issues:**
```bash
for repo in $(gh repo list brazil-bench --json name -q '.[].name' | grep -E "^20[0-9]{2}-"); do
  closed=$(gh issue list -R brazil-bench/$repo --state closed --json number -q 'length')
  if [ "$closed" -gt 0 ]; then
    echo "Re-evaluating $repo..."
    # re-evaluate $repo
  fi
done
```

## Usage

### Environment Setup

**Pourpoise (this repo):** Can be run from a laptop for generating leaderboards, but the evaluations will partially fail as the tests work on Linux but the database setup doesn't work the same way on MacOS. Best to run in a Codespace.

### Creating a New Benchmark Attempt

To create a new attempt repository, use the GitHub web UI:

1. Go to https://github.com/brazil-bench/benchmark-template
2. Click "Use this template" → "Create a new repository"
3. Name the repo following the convention: `YYYY-MM-DD-{language}-{description}`
4. Set the owner to `brazil-bench` and make it public
5. Create the repository

**Naming convention:** `YYYY-MM-DD-{language}-{description}`
- `2025-12-01-python-gpt4` - GPT-4 implementing in Python
- `2025-12-01-python-gemini` - Gemini in Python
- `2025-12-01-typescript-claude` - Claude in TypeScript

**Attempt repositories:** Should be run in a GitHub Codespace for safety and consistency:
1. Open the attempt repo in a Codespace
2. Install your LLM of choice and any other packages and frameworks
3. Run with dangerous permissions enabled (e.g., `claude --dangerously-skip-permissions`)
4. Let the LLM run to completion without intervention
5. Save the prompts and commands used to `prompts.txt`

This isolation ensures:
- Consistent environment across attempts
- Safe execution of LLM-generated code
- No interference from local machine configuration


**Important:** Save your setup, initial command and prompts to `prompts.txt`, which contains the following prompt as a starting point

```
> npx claude-flow@alpha hive-mind spawn "Read brazilian-soccer-mcp-guide.md and implement phases 1,2 and 3 as described and test using BDD GWT structured PyTest. Use Neo4j. maintain a detailed context block comment at the start of every code file. Finally update README.md to describe what was done and push everything to github" --claude
```

### Evaluating a Benchmark Attempt

To evaluate an attempt, use the evaluate-attempt skill from the pourpoise repo:

```
> evaluate attempt 2025-10-30-python-hive
```

This will:
1. Clone the attempt repository from `brazil-bench/{attempt_repo}`
2. Verify spec integrity against the template
3. Run conformance tests (or find evidence of prior runs)
4. Measure code metrics (LOC, files, dependencies)
5. Extract git metrics and analyze development timeline
6. Separate agent-driven vs human-driven commits
7. Assess data strategy (real Kaggle vs simulated)
8. Analyze implementation against spec requirements
9. Generate a report in `results/{attempt_repo}.md`

#### Data Strategy Assessment

The evaluation distinguishes between:
- **Simulated Data:** Sample/mock data for testing
- **Real Kaggle Data:** Integration with actual Brazilian soccer datasets

Schema compliance is measured separately from data population, crediting implementations that properly model spec entities even when external data doesn't include all fields.

#### Timeline Analysis

The evaluation identifies development phases:
- **Setup (Human):** Initial commit, repo setup
- **Agent Implementation:** Core implementation work
- **Agent Test Iteration:** Test fixing to 100% pass
- **Human Intervention:** Post-completion changes

### Generating Codebase Documentation

To generate comprehensive documentation for a codebase:

```
> summarize codebase reviews/2025-09-30-python-swarm
```

This generates structured documentation including:
- `index.md` - Knowledge base index (primary context file for AI assistants)
- `architecture.md` - System design and patterns
- `components.md` - Component documentation
- `interfaces.md` - API and tool documentation
- `data_models.md` - Entity models and relationships
- `workflows.md` - Process flows and procedures
- `dependencies.md` - Package dependencies
- `review_notes.md` - Documentation gaps and recommendations

### Comparing Attempts (Leaderboard)

To compare all evaluated attempts and generate a ranked leaderboard:

```
> compare attempts
```

This will:
1. Scan all evaluation reports in `results/`
2. Extract and normalize metrics from each report
3. Calculate composite scores based on spec compliance, efficiency, and quality
4. Generate a ranked Top 10 leaderboard
5. Create detailed comparison tables with data strategy assessment
6. Output analysis and insights to `results/LEADERBOARD.md`

## Scoring Methodology (v3)

```
Score = (Spec % × 50) + (Tests × 30) + (Quality × 15) + (Efficiency × 5)

Where:
- Spec Compliance % = (implemented / total) × 100
- Test Score = min(100, EFFECTIVE_TESTS × 1.5)  [excludes skipped tests]
- Quality = 100 - (fix_commits × 10) - skip_penalty
- Skip Penalty = max(0, (skip_ratio - 0.10) × 50)  [penalizes >10% skipped]
- Efficiency = 100 - min(100, LOC / 100)
```

**Priority Order:** Compliance > Effective Tests > Quality > Skip Ratio > Duration

The methodology uses "effective tests" (passed + failed, excluding skipped) rather than total tests to prevent inflated test counts from `pytest.skip("not yet implemented")` stubs.

## Directory Structure

- `skills/` - Claude Code skills for benchmark management
  - `evaluate-attempt/` - Evaluate a single benchmark attempt
  - `compare-attempts/` - Compare attempts and generate leaderboard
  - `file-issues/` - File GitHub issues from evaluation reports
  - `re-evaluate/` - Re-evaluate attempts after issues are fixed
  - `codebase-summary/` - Generate codebase documentation
  - `pdd/` - Prompt-Driven Development planning
  - `code-task-generator/` - Generate code task files
  - `code-assist/` - Code implementation assistance
- `sop/` - Source SOP files (used to generate skills)
  - `evaluate-attempt.sop.md` - Initial evaluation SOP
  - `compare-attempts.sop.md` - Leaderboard generation SOP
  - `file-issues.sop.md` - Issue filing SOP
  - `re-evaluate.sop.md` - Re-evaluation lifecycle SOP
- `results/` - Evaluation reports and documentation (generated)
  - `LEADERBOARD.md` - Ranked comparison of all attempts
  - `{attempt_repo}.md` - Individual evaluation report
  - `{attempt_repo}-summary/` - Generated codebase documentation
- `reviews/` - Cloned attempt repositories for evaluation (gitignored)
- `tasks/` - Benchmark task definitions
  - `beads/spec.md` - The benchmark specification

## Regenerating Skills

To regenerate skills from updated SOPs:

```bash
python -m strands_agents_sops skills --sop-paths ./sop --output-dir ./skills
```
