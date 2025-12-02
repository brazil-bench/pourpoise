---
name: create-attempt
description: This SOP creates a new brazil-bench attempt repository from the template. The repository provides a clean starting point for evaluating any LLM or agent framework against the Brazilian Soccer MCP benchmark task.
type: anthropic-skill
version: "1.0"
---

# Create Benchmark Attempt

## Overview
This SOP creates a new brazil-bench attempt repository from the template.
The repository provides a clean starting point for evaluating any LLM or
agent framework against the Brazilian Soccer MCP benchmark task.

## Parameters
- **attempt_name** (required): Repository name using format `YYYY-MM-DD-{language}-{description}`
  - Examples: `2025-12-01-python-gpt4`, `2025-12-01-typescript-gemini`, `2025-12-01-python-claude-solo`

## Steps

### 1. Validate Preconditions
Ensure the attempt doesn't already exist.

**Constraints:**
- You MUST verify the repo name doesn't exist in brazil-bench org
- You MUST verify the attempt_name follows the naming convention
- You MUST NOT proceed if preconditions fail

```bash
# Check repo doesn't exist
gh repo view brazil-bench/{attempt_name} 2>&1 | grep -q "Could not resolve" || echo "ERROR: Repo already exists"

# Validate naming format (YYYY-MM-DD-language-description)
echo "{attempt_name}" | grep -qE "^[0-9]{4}-[0-9]{2}-[0-9]{2}-[a-z]+-[a-z0-9-]+$" || echo "WARNING: Name doesn't follow convention"
```

### 2. Create Repository from Template
Instantiate a new repo from benchmark-template.

**Constraints:**
- You MUST use the GitHub template mechanism
- You MUST create as public repo
- You MUST wait for repo creation to complete before proceeding

```bash
gh repo create brazil-bench/{attempt_name} \
  --template brazil-bench/benchmark-template \
  --public \
  --clone
```

### 3. Verify Template Contents
Confirm the spec file is present and unmodified.

**Constraints:**
- You MUST verify `brazilian-soccer-mcp-guide.md` exists (the spec)
- You MUST list the initial contents for the user
- You MUST NOT modify any template files

```bash
cd {attempt_name}
ls -la
cat brazilian-soccer-mcp-guide.md | head -50
```

### 4. Document Next Steps
Provide instructions for running the benchmark.

**Constraints:**
- You MUST output the repository URL
- You MUST explain how to run the benchmark with any LLM
- You MUST remind user to capture prompts.txt for evaluation

## Output

Upon completion, report:

```
=== Attempt Created ===
Repository: https://github.com/brazil-bench/{attempt_name}
Spec file:  brazilian-soccer-mcp-guide.md

=== Next Steps ===
1. Clone the repository or open in your preferred environment
2. Run your LLM/agent with a prompt like:
   "Read brazilian-soccer-mcp-guide.md and implement phases 1, 2, and 3
    as described. Test using BDD PyTest. Use Neo4j for the graph database."
3. Save the initial command and prompts to prompts.txt for evaluation
4. When complete, evaluate with:
   evaluate attempt {attempt_name}
```

## Naming Convention

Use the format: `YYYY-MM-DD-{language}-{description}`

| Component | Description | Examples |
|-----------|-------------|----------|
| Date | When attempt started | 2025-12-01 |
| Language | Implementation language | python, typescript, go |
| Description | LLM or pattern used | gpt4, gemini, claude-hive, llama |

Examples:
- `2025-12-01-python-gpt4` - GPT-4 implementing in Python
- `2025-12-01-python-claude-solo` - Claude Code solo (no orchestration)
- `2025-12-01-typescript-gemini` - Gemini implementing in TypeScript

## Troubleshooting

**Repo creation fails**
- Check org permissions: `gh org list`
- Verify template exists: `gh repo view brazil-bench/benchmark-template`
- Check for name collision with existing repo

**Template is empty or missing spec**
- Verify benchmark-template contains `brazilian-soccer-mcp-guide.md`
- Check template repo: `gh repo view brazil-bench/benchmark-template --json description`

**Permission denied**
- Verify GitHub CLI is authenticated: `gh auth status`
- Check you have write access to brazil-bench org