# pourpoise

Purpose management for this organization

The purpose of this organization is to generate benchmark results that compare approaches to agent development management on the same task.

The standard task is to create an MCP server for a brazilian football statistics knowledge graph, and every benchmark starts with an empty repo that contains the spec, which is created from the benchmark-template repo provided here.

A secondary purpose is to explore and develop mechanisms that automate the development management of agents in a purposeful way.

## Current Leaderboard

> 5 attempts evaluated as of 2025-12-13

| Rank | Attempt | Pattern | Duration | LOC | Compliance | Fix Commits |
|------|---------|---------|----------|-----|------------|-------------|
| 🥇 | 2025-12-01-python-claude-beads | Beads | ~11 min | 1,826 | 12/16 | 0 |
| 🥈 | 2025-12-13-python-claude-hive | Hive Mind v2 | ~37 min | 6,165 | 13/16 | 0 |
| 🥉 | 2025-10-30-python-hive | Hive Mind v1 | ~41 min | 3,545 | 15/16 | 1 |
| 4 | 2025-09-30-python-swarm | Swarm v1 | ~1h 49m | 8,683 | 14/16 | 7 |
| 5 | 2025-12-13-python-claude-swarm | Swarm v2 | ~1h 54m | 4,227 | 10/16 | 0 |

See [results/LEADERBOARD.md](results/LEADERBOARD.md) for detailed analysis.

### Key Insights

- **Fastest:** Beads (~11 min autonomous, 0 fix commits)
- **Most Complete:** Hive Mind v1 (94% spec compliance, 64 BDD tests)
- **Best Real Data:** Swarm v1 (96MB Kaggle data integrated)
- **Cleanest:** Beads, Hive v2, Swarm v2 (0 fix commits each)

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
| `codebase-summary` | Generate documentation for a codebase | "summarize codebase reviews/attempt-name" |
| `pdd` | Prompt-Driven Development planning | "help me design a new feature" |
| `code-task-generator` | Generate code task files | "generate tasks from my PDD plan" |
| `code-assist` | Assist with code implementation | "help me implement this task" |

Skills are invoked using natural language in Claude Code.

## Usage

### Environment Setup

**Pourpoise (this repo):** Can be run from a laptop for generating leaderboards, but the evaluations will partially fail as the tests work on Linux but the database setup doesn't work the same way on MacOS. Best to run in a Codespace.

**Attempt repositories:** Should be run in a GitHub Codespace for safety and consistency:
1. Open the attempt repo in a Codespace
2. Run with dangerous permissions enabled (e.g., `claude --dangerously-skip-permissions`)
3. Let the LLM run to completion without intervention
4. Save the prompts and commands used to `prompts.txt`

This isolation ensures:
- Consistent environment across attempts
- Safe execution of LLM-generated code
- No interference from local machine configuration

### Creating a New Benchmark Attempt

To create a new attempt repository, use the GitHub web UI:

1. Go to https://github.com/brazil-bench/benchmark-template
2. Click "Use this template" → "Create a new repository"
3. Name the repo following the convention: `YYYY-MM-DD-{language}-{description}`
4. Set the owner to `brazil-bench`
5. Create the repository

**Naming convention:** `YYYY-MM-DD-{language}-{description}`
- `2025-12-01-python-gpt4` - GPT-4 implementing in Python
- `2025-12-01-python-gemini` - Gemini in Python
- `2025-12-01-typescript-claude` - Claude in TypeScript

After creating the repo, launch a new Codespace, install your LLM of choice and, run with a prompt like:
> npx claude-flow@alpha hive-mind spawn "Read brazilian-soccer-mcp-guide.md and implement phases 1,2 and 3 as described and test using BDD GWT structured PyTest. Use Neo4j. maintain a detailed context block comment at the start of every code file. Finally update README.md to describe what was done and push everything to github" --claude


**Important:** Save your initial command and prompts to `prompts.txt` for evaluation.

### Evaluating a Benchmark Attempt

To evaluate an attempt, use the evaluate-attempt skill:

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

## Scoring Methodology

```
Score = (Spec % × 40) + (Tests × 20) + (Efficiency × 20) + (Quality × 20)

Where:
- Spec Compliance % = (schema implemented / total) × 100
- Test Coverage Score = min(100, test_scenarios × 1.5)
- Efficiency Score = 100 - min(100, LOC / 100)
- Code Quality Score = 100 - (fix_commits × 10)
```

## Directory Structure

- `skills/` - Claude Code skills for benchmark management
  - `evaluate-attempt/` - Evaluate a single benchmark attempt
  - `compare-attempts/` - Compare attempts and generate leaderboard
  - `codebase-summary/` - Generate codebase documentation
  - `pdd/` - Prompt-Driven Development planning
  - `code-task-generator/` - Generate code task files
  - `code-assist/` - Code implementation assistance
- `sop/` - Source SOP files (used to generate skills)
- `results/` - Evaluation reports and documentation (generated)
  - `LEADERBOARD.md` - Ranked comparison of all attempts
  - `{attempt_repo}.md` - Individual evaluation report
  - `{attempt_repo}-summary/` - Generated codebase documentation
- `reviews/` - Cloned attempt repositories for evaluation (gitignored)

## Regenerating Skills

To regenerate skills from updated SOPs:

```bash
python -m strands_agents_sops skills --sop-paths ./sop --output-dir ./skills
```
