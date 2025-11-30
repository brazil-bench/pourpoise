# Create Benchmark Attempt

## Overview
This SOP creates a new brazil-bench attempt from the template repository,
configures it for a specific orchestration pattern, and initiates the work
by creating a GitHub issue with the implementation task.

## Parameters
- **attempt_name** (required): Repository name (e.g., `attempt-3`, `solo-baseline`)
- **pattern** (required): Orchestration pattern (`solo`, `swarm`, `hive`, `crew`, `custom`)
- **pattern_config** (optional): Path to custom pattern configuration file
- **assignee** (optional): GitHub username to assign the implementation issue

## Steps

### 1. Validate Preconditions
Ensure the attempt doesn't already exist and the pattern is recognized.

**Constraints:**
- You MUST verify the repo name doesn't exist in brazil-bench org
- You MUST verify the pattern is valid or pattern_config is provided
- You MUST NOT proceed if preconditions fail
```bash
gh repo view brazil-bench/{attempt_name} 2>&1 | grep -q "Could not resolve"
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
  --confirm
```

### 3. Configure Orchestration Pattern
Add pattern-specific files to the new repository.

**Constraints:**
- You MUST add a CLAUDE.md appropriate for the pattern
- You MUST record the pattern in a metadata file
- You SHOULD add any pattern-specific tooling configs
- You MUST NOT modify spec.md
```bash
# Clone to add files
gh repo clone brazil-bench/{attempt_name} ./tmp/{attempt_name}

# Write pattern metadata
cat > ./tmp/{attempt_name}/.brazil-bench.json << EOF
{
  "pattern": "{pattern}",
  "created": "$(date -u +%Y-%m-%dT%H:%M:%SZ)",
  "template_version": "1.0"
}
EOF

# Copy pattern-specific CLAUDE.md
cp ./patterns/{pattern}/CLAUDE.md ./tmp/{attempt_name}/CLAUDE.md

# Push
cd ./tmp/{attempt_name}
git add -A
git commit -m "Configure for {pattern} orchestration pattern"
git push
```

### 4. Create Implementation Issue
Create the issue that will drive the worker agent.

**Constraints:**
- You MUST create exactly one issue titled "Implement MCP Server"
- You MUST include the full implementation SOP in the issue body
- You SHOULD assign to {assignee} if provided
- You MUST label with `brazil-bench` and `{pattern}`
```bash
gh issue create \
  --repo brazil-bench/{attempt_name} \
  --title "Implement MCP Server per spec.md" \
  --body-file ./sops/implement-mcp.sop.md \
  --label "brazil-bench,{pattern}" \
  ${assignee:+--assignee {assignee}}
```

### 5. Record Attempt
Register this attempt in the Pourpoise tracking system.

**Constraints:**
- You MUST append to `./attempts.json`
- You MUST include: repo name, pattern, creation timestamp, issue URL
- You SHOULD trigger any notification webhooks configured
```bash
# Get issue URL
ISSUE_URL=$(gh issue list --repo brazil-bench/{attempt_name} --limit 1 --json url -q '.[0].url')

# Append to tracking
cat ./attempts.json | jq \
  --arg name "{attempt_name}" \
  --arg pattern "{pattern}" \
  --arg created "$(date -u +%Y-%m-%dT%H:%M:%SZ)" \
  --arg issue "$ISSUE_URL" \
  '.attempts += [{"name": $name, "pattern": $pattern, "created": $created, "issue": $issue, "status": "active"}]' \
  > ./attempts.json.tmp && mv ./attempts.json.tmp ./attempts.json

git add attempts.json
git commit -m "Track new attempt: {attempt_name}"
git push
```

### 6. Verify Setup
Confirm the attempt is ready for a worker agent.

**Constraints:**
- You MUST verify repo is accessible
- You MUST verify issue exists and is open
- You MUST verify spec.md matches template
- You SHOULD output a ready confirmation
```bash
echo "=== Verification ==="
gh repo view brazil-bench/{attempt_name} --json name,url
gh issue list --repo brazil-bench/{attempt_name} --state open
echo "Attempt {attempt_name} ready for {pattern} worker"
```

## Output
Upon completion, report:
- Repository URL: `https://github.com/brazil-bench/{attempt_name}`
- Issue URL: (from step 4)
- Pattern: {pattern}
- Status: Ready for worker

## Troubleshooting

**Repo creation fails**
- Check org permissions: `gh org list`
- Verify template exists: `gh repo view brazil-bench/benchmark-template`
- Check for name collision

**Pattern not found**
- Verify `./patterns/{pattern}/CLAUDE.md` exists
- For custom patterns, ensure pattern_config path is valid
- Run `ls ./patterns/` to see available patterns

**Issue creation fails**
- Verify repo was created successfully
- Check `./sops/implement-mcp.sop.md` exists
- Verify GitHub token has issue write permissions