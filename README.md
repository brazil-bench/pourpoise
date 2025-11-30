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

### Evaluating a Benchmark Attempt

To evaluate an attempt, use the evaluate-attempt SOP in Claude Code:

```
@sop/evaluate-attempt.sop.md 2025-10-30-python-hive
```

This will:
1. Clone the attempt repository from `brazil-bench/{attempt_repo}`
2. Verify spec integrity against the template
3. Run conformance tests
4. Measure code metrics (LOC, files, dependencies)
5. Extract git metrics (commits, duration, fix/revert history)
6. Analyze implementation against spec requirements
7. Generate a report in `results/{attempt_repo}.md`

## Directory Structure

- `sop/` - Standard Operating Procedures for evaluation tasks
- `results/` - Evaluation reports (generated)
- `reviews/` - Cloned attempt repositories for evaluation (gitignored)
