# pourpoise
Purpose management for this organization

The purpose of this organization is to generate benchmark results that compare approaches to agent development management on the same task.

The standard task is to create an MCP server for a brazilian football statistics knowledge graph, and every benchmark starts with an empty repo that contains the spec, which is created from the benchmark-template repo provided here.

A secondary purpose is to explore and develop mechanisms that automate the development management of agents in a purposeful way.

## Setup

### Prerequisites

Install GitHub CLI and authenticate:
```bash
# Install GitHub CLI (macOS)
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

### MCP Server

The `.mcp.json` file is included in this repository and configures the `strands-agents-sops` MCP server automatically. This server provides SOP (Standard Operating Procedure) tools for Claude Code, with SOPs located in the `./sop` directory.

No additional configuration is needed—Claude Code will detect and use the `.mcp.json` file when run from this directory.

## Usage

### Environment Setup

**Pourpoise (this repo):** Can be run from a laptop. Used for creating attempts, running evaluations, and generating leaderboards.

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

To create a new attempt repository for any LLM evaluation:

```
/strands-agents-sops:create-attempt attempt_name: 2025-12-01-python-gpt4
```

This will:
1. Create a new repo from the benchmark-template
2. Verify the spec file is present
3. Provide instructions for running the benchmark

**Naming convention:** `YYYY-MM-DD-{language}-{description}`
- `2025-12-01-python-gpt4` - GPT-4 implementing in Python
- `2025-12-01-python-gemini` - Gemini in Python
- `2025-12-01-typescript-claude` - Claude in TypeScript

After creating, run your LLM with a prompt like:
> "Read brazilian-soccer-mcp-guide.md and implement phases 1, 2, and 3 as described. Test using BDD PyTest. Use Neo4j for the graph database."

**Important:** Save your initial command and prompts to `prompts.txt` for evaluation.

### Evaluating a Benchmark Attempt

To evaluate an attempt, use the evaluate-attempt SOP via the MCP server:

```
/strands-agents-sops:evaluate-attempt attempt_repo: 2025-10-30-python-hive
```

This will:
1. Clone the attempt repository from `brazil-bench/{attempt_repo}`
2. Verify spec integrity against the template
3. Run conformance tests
4. Measure code metrics (LOC, files, dependencies)
5. Extract git metrics (commits, duration, fix/revert history)
6. Analyze implementation against spec requirements
7. Generate codebase documentation summary
8. Generate a report in `results/{attempt_repo}.md`

### Generating Codebase Documentation

To generate comprehensive documentation for a codebase (standalone or as part of evaluation), use the codebase-summary SOP:

```
/strands-agents-sops:codebase-summary codebase-path: reviews/2025-09-30-python-swarm output_dir: results/2025-09-30-python-swarm-summary
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
/strands-agents-sops:compare-attempts
```

This will:
1. Scan all evaluation reports in `results/`
2. Extract and normalize metrics from each report
3. Calculate composite scores based on spec compliance, efficiency, and quality
4. Generate a ranked Top 10 leaderboard
5. Create detailed comparison tables
6. Output analysis and insights to `results/LEADERBOARD.md`

## Directory Structure

- `sop/` - Standard Operating Procedures for evaluation tasks
  - `evaluate-attempt.sop.md` - Evaluate a single benchmark attempt
  - `compare-attempts.sop.md` - Compare attempts and generate leaderboard
  - `create-attempt.sop.md` - Create a new benchmark attempt
- `results/` - Evaluation reports and documentation (generated)
  - `LEADERBOARD.md` - Ranked comparison of all attempts
  - `{attempt_repo}.md` - Individual evaluation report
  - `{attempt_repo}-summary/` - Generated codebase documentation
- `reviews/` - Cloned attempt repositories for evaluation (gitignored)
