# File Issues from Evaluation

## Overview
This SOP parses evaluation reports from brazil-bench attempts and creates individual
GitHub issues on each attempt's repository for every shortcoming, warning, or
improvement opportunity identified during the review.

## Parameters
- **attempt_repo** (required): Repository name (e.g., `2025-12-13-python-claude-hive`)
- **evaluation_path** (optional, default: `./results/{attempt_repo}.md`): Path to evaluation report
- **dry_run** (optional, default: `false`): If true, show issues that would be created without creating them
- **labels** (optional, default: `evaluation,automated`): Comma-separated labels to apply

## Steps

### 1. Load Evaluation Report
Read the evaluation report for the specified attempt.

**Constraints:**
- You MUST verify the evaluation report exists before proceeding
- You MUST parse markdown sections to identify shortcomings
- You MUST NOT proceed if the report is missing or malformed

```bash
# Verify report exists
ls ./results/{attempt_repo}.md

# Read the report content
cat ./results/{attempt_repo}.md
```

### 2. Verify Repository Access
Confirm the attempt repository exists and is writable.

**Constraints:**
- You MUST verify the repo exists on GitHub
- You MUST check that issues are enabled on the repo
- You SHOULD check for existing issues to avoid duplicates
- You SHOULD use default GitHub labels (no custom labels required)

```bash
# Verify repo exists
gh repo view brazil-bench/{attempt_repo}

# List existing issues to check for duplicates
gh issue list -R brazil-bench/{attempt_repo} --limit 100

# List available labels (use default GitHub labels)
gh label list -R brazil-bench/{attempt_repo}
```

**Default GitHub Labels to Use:**

| Issue Type | Default Label | Reason |
|------------|---------------|--------|
| Missing requirement | `enhancement` | It's a feature that needs to be added |
| Test quality issue | `bug` | Tests not working as expected |
| Compliance summary | `enhancement` | Tracking improvements needed |
| Quality concern | `enhancement` | Code improvement request |
| Documentation gap | `documentation` | Missing or incomplete docs |

Using default labels avoids permission issues (creating custom labels requires admin access).

### 3. Extract Shortcomings from Report
Parse the evaluation report to identify all issues to file.

**Systematic Extraction Commands:**

Use these commands to programmatically extract shortcomings:

```bash
# Extract missing requirements (unchecked items)
grep -E "^- \[ \]" ./results/{attempt_repo}.md

# Extract test quality warnings
grep -A 20 "## Test Quality Warning\|## Test Skip Analysis" ./results/{attempt_repo}.md

# Extract weaknesses section
grep -A 10 "## Weaknesses" ./results/{attempt_repo}.md

# Extract compliance score
grep -E "Spec Compliance.*[0-9]+/[0-9]+" ./results/{attempt_repo}.md

# Extract skip ratio
grep -E "Skip Ratio.*[0-9]+%" ./results/{attempt_repo}.md
```

**Categories of Issues to Extract:**

#### 3a. Missing Requirements
Look in the "Requirements Checklist" section for unchecked items:

**Pattern:** Lines starting with `- [ ]` indicate missing/partial requirements

```bash
# Extract all missing requirements
grep -E "^- \[ \]" ./results/{attempt_repo}.md
```

```markdown
### Missing/Partial
- [ ] MCP server implementation (tools not exposed as MCP endpoints)
- [ ] Query performance benchmarks (< 2s simple, < 5s aggregate)
```

**Issue Template:**
```
Title: [Missing] {requirement description}
Label: enhancement
Body:
## Requirement
{description}

## Notes from Evaluation
{any additional context from the report}

## Suggested Fix
{if available}

---
Filed from evaluation: results/{attempt_repo}.md
```

#### 3b. Test Quality Warnings
Look for the "Test Quality Warning" or "Test Skip Analysis" sections:

**Patterns to detect:**
- Skip ratio > 20%
- Tests with "not yet implemented" messages
- Integration tests that can't run

**Issue Template:**
```
Title: [Test Quality] {description of test issue}
Label: bug
Body:
## Issue
{description of the test quality problem}

## Details
{skip breakdown, affected files, etc.}

## Impact
- Skip Ratio: {X}%
- Effective Tests: {Y} (vs {Z} total)

## Suggested Fix
{recommendations}

---
Filed from evaluation: results/{attempt_repo}.md
```

#### 3c. Spec Compliance Issues
Look for compliance scores below 100%:

**Patterns:**
- Spec Compliance: X/16 where X < 16
- Partial implementations noted

**Issue Template:**
```
Title: [Compliance] Spec compliance at {X}% ({Y}/16 requirements)
Label: enhancement
Body:
## Current Status
{X}/16 requirements met ({percentage}%)

## Missing Requirements
{list of unchecked requirements}

## Partial Implementations
{list of partial implementations with notes}

---
Filed from evaluation: results/{attempt_repo}.md
```

#### 3d. Architecture/Quality Issues
Look for concerns in "Architecture Summary", "Weaknesses", or "Areas of Note":

**Patterns:**
- Bullet points under "Weaknesses" heading
- Notes about missing MCP endpoints
- Performance concerns
- Code structure issues

**Issue Template:**
```
Title: [Quality] {description}
Label: enhancement
Body:
## Issue
{description}

## Context
{relevant context from the evaluation}

## Recommendation
{suggested improvement}

---
Filed from evaluation: results/{attempt_repo}.md
```

### 4. Create Issues
File each extracted shortcoming as a separate GitHub issue.

**Constraints:**
- You MUST create one issue per shortcoming (not a combined issue)
- You MUST apply appropriate labels (after ensuring they exist in Step 2)
- You MUST include reference to the evaluation report
- You MUST check for duplicates before creating (match on title)
- You MUST create detail issues BEFORE the summary issue (so you can reference issue numbers)
- You SHOULD group related issues with a common prefix
- You MAY skip issues if identical ones already exist

**Issue Creation Order:**
1. Create all detail issues first ([Missing], [Test Quality], [Quality], etc.)
2. Note the issue numbers assigned
3. Create the [Compliance] summary issue last, referencing the detail issues

```bash
# Check if issue already exists
gh issue list -R brazil-bench/{attempt_repo} --search "{issue_title}" --json title

# Create issue using HEREDOC for multi-line markdown body
gh issue create -R brazil-bench/{attempt_repo} \
  --title "[Missing] MCP server implementation" \
  --label "enhancement" \
  --body "$(cat <<'EOF'
## Requirement

The spec requires an MCP server with tools exposed as endpoints.

## Current State

Query engine exists but not exposed as MCP tools.

## Suggested Fix

Add MCP server wrapper using `fastmcp` or `mcp` package.

---
Filed from evaluation: [results/{attempt_repo}.md](https://github.com/brazil-bench/pourpoise/blob/main/results/{attempt_repo}.md)
EOF
)"

# If labels don't exist, create without labels (they can be added later)
gh issue create -R brazil-bench/{attempt_repo} \
  --title "{issue_title}" \
  --body "$(cat <<'EOF'
{issue_body_markdown}
EOF
)"
```

**Cross-Referencing Issues:**

When creating the summary [Compliance] issue, reference related detail issues:

```markdown
## Missing Requirements

1. **MCP server implementation** - see #2
2. **Query performance benchmarks** - see #3
3. **Cross-file queries verification** - see #4

## Additional Issues

- **Test Quality:** 84% skip ratio - see #1
```

**Issue Creation Rules:**

| Shortcoming Type | Title Prefix | Default Label |
|------------------|--------------|---------------|
| Missing requirement | `[Missing]` | `enhancement` |
| Test quality issue | `[Test Quality]` | `bug` |
| Spec compliance | `[Compliance]` | `enhancement` |
| Architecture/quality | `[Quality]` | `enhancement` |
| Performance concern | `[Performance]` | `enhancement` |
| Documentation gap | `[Docs]` | `documentation` |

#### 4a. Summary Issue Template

After creating all detail issues, create a [Compliance] summary issue that links them together:

```bash
gh issue create -R brazil-bench/{attempt_repo} \
  --title "[Compliance] Spec compliance at {X}% ({Y}/16 requirements)" \
  --label "enhancement" \
  --body "$(cat <<'EOF'
## Current Status

{Y}/16 requirements met ({X}% compliance)

## Missing Requirements

1. **{requirement_1}** - see #{issue_number_1}
2. **{requirement_2}** - see #{issue_number_2}
...

## Implemented Requirements

### Category 1 (N/M)
- [x] Requirement that passed
- [x] Another passing requirement
...

## Additional Issues

- **Test Quality:** {skip_ratio}% skip ratio - see #{test_quality_issue}

## Benchmark Score

Current score: **{score}** (rank {rank} of 8)

To improve ranking, address the issues linked above.

---
Filed from evaluation: [results/{attempt_repo}.md](https://github.com/brazil-bench/pourpoise/blob/main/results/{attempt_repo}.md)
EOF
)"
```

This summary issue serves as an index to all other filed issues and provides context for prioritization.

### 5. Generate Summary
Output a summary of all issues created.

**Constraints:**
- You MUST list all issues created with their URLs
- You MUST note any issues skipped due to duplicates
- You MUST include the total count

**Output Format:**
```markdown
## Issues Filed for {attempt_repo}

### Created ({N} issues)
| # | Title | URL |
|---|-------|-----|
| 1 | [Missing] MCP server implementation | {url} |
| 2 | [Test Quality] 84% tests skip unconditionally | {url} |
| ... | ... | ... |

### Skipped (duplicates)
- {title} - already exists as #{existing_issue_number}

### Summary
- Total shortcomings identified: {X}
- Issues created: {Y}
- Issues skipped (duplicates): {Z}
```

## Output Format

The SOP produces:
1. GitHub issues on the attempt repository
2. A summary markdown output showing all issues filed

```markdown
# Issue Filing Summary: {attempt_repo}

## Repository
brazil-bench/{attempt_repo}

## Evaluation Report
results/{attempt_repo}.md

## Issues Created

### Missing Requirements ({count})
| Issue | Title | URL |
|-------|-------|-----|
| #1 | [Missing] MCP server implementation | https://... |

### Test Quality ({count})
| Issue | Title | URL |
|-------|-------|-----|
| #2 | [Test Quality] 84% skip ratio | https://... |

### Compliance ({count})
...

### Quality ({count})
...

## Statistics
- Total issues created: {N}
- Skipped (duplicates): {M}
- Evaluation score: {score}
```

## Examples

### Example 1: Filing issues for Hive v2

```bash
# File issues from the Hive v2 evaluation
file issues for 2025-12-13-python-claude-hive
```

Expected issues:
1. `[Missing] MCP server implementation - tools not exposed as MCP endpoints`
2. `[Missing] Query performance benchmarks (< 2s simple, < 5s aggregate)`
3. `[Missing] Cross-file queries verification`
4. `[Test Quality] 84% of tests skip unconditionally (53/63 tests)`
5. `[Compliance] Spec compliance at 81% (13/16 requirements)`

### Example 2: Dry run to preview issues

```bash
# Preview issues without creating them
file issues for 2025-12-13-python-claude-hive --dry-run
```

### Example 3: Filing with custom labels

```bash
# Add custom labels
file issues for 2025-12-14-python-claude-beads-2 --labels "evaluation,v3-methodology,needs-review"
```

## Troubleshooting

**No evaluation report found**
- Run the evaluate-attempt skill first
- Check the path: `./results/{attempt_repo}.md`

**Permission denied on repository**
- Verify you have write access: `gh repo view brazil-bench/{attempt_repo}`
- Check if issues are enabled on the repo

**Duplicate detection failing**
- Issues are matched by exact title
- If titles differ slightly, duplicates may be created
- Use `gh issue list -R repo --search "keyword"` to check

**Labels don't exist**
- Use default GitHub labels (`enhancement`, `bug`, `documentation`) which exist on all repos
- Avoid custom labels as creating them requires admin access (HTTP 403 error)
- If a label doesn't exist, omit the `--label` flag entirely
- The title prefix (e.g., `[Missing]`, `[Test Quality]`) provides categorization without labels

**Too many issues**
- Use `--dry-run` first to preview
- Consider grouping minor issues into a single "Minor improvements" issue
- Focus on high-impact shortcomings first
