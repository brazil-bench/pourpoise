# pourpoise
Purpose management for this organization

The purpose of this organization is to generate benchmark results that compare approaches to agent development management on the same task.

The standard task is to create an MCP server for a brazilian football statistics knowledge graph, and every benchmark starts with an empty repo that contains the spec, which is created from the benchmark-template repo provided here.

A secondary purpose is to explore and develop mechanisms that automate the development management of agents in a purposeful way.

## Setup

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

The `.mcp.json` configures the `strands-agents-sops` MCP server which provides SOP (Standard Operating Procedure) tools for Claude Code. The SOPs are located in the `./sop` directory.

To use with Claude Code, ensure the MCP server is enabled:
```bash
claude mcp add strands-agents-sops strands-agents-sops mcp --sop-paths ./sop
```

## Directory Structure

- `sop/` - Standard Operating Procedures for evaluation tasks
- `results/` - Evaluation reports (generated)
- `reviews/` - Cloned attempt repositories for evaluation (gitignored)
