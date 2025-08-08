---
allowed-tools: Bash
description: Generate PR summary in markdown and copy to clipboard
---

## Context
- Current branch: !`git branch --show-current`
- Base branch: !`git symbolic-ref refs/remotes/origin/HEAD 2>/dev/null | sed 's@^refs/remotes/origin/@@' || echo "main"`

## Your task

Generate a comprehensive Pull Request summary in markdown format and copy it to the clipboard.

The summary should include:
1. Branch information (current → base)
2. Number of commits
3. List of commits with messages
4. Modified files list
5. Diff statistics

Execute this bash script to generate and copy the PR summary:

```bash
#!/bin/bash

# Get the base branch (usually main or master)
BASE_BRANCH=$(git symbolic-ref refs/remotes/origin/HEAD 2>/dev/null | sed 's@^refs/remotes/origin/@@' || echo "main")
CURRENT_BRANCH=$(git branch --show-current)

# Check if we're on the base branch
if [ "$CURRENT_BRANCH" = "$BASE_BRANCH" ]; then
    echo "Error: Already on the base branch ($BASE_BRANCH). Please switch to a feature branch." >&2
    exit 1
fi

# Get commit messages since branching from base
COMMITS=$(git log --oneline "$BASE_BRANCH".."$CURRENT_BRANCH" --reverse)

# Get the diff stats
STATS=$(git diff --stat "$BASE_BRANCH"..."$CURRENT_BRANCH")

# Get the list of changed files
FILES=$(git diff --name-only "$BASE_BRANCH"..."$CURRENT_BRANCH")

# Create the markdown summary
SUMMARY="## PR Summary

### Branch
\`$CURRENT_BRANCH\` → \`$BASE_BRANCH\`

### Changes
$(echo "$COMMITS" | wc -l | xargs) commit(s)

### Commits
\`\`\`
$COMMITS
\`\`\`

### Modified Files
\`\`\`
$FILES
\`\`\`

### Statistics
\`\`\`
$STATS
\`\`\`"

# Copy to clipboard based on OS
if command -v pbcopy >/dev/null 2>&1; then
    # macOS
    echo "$SUMMARY" | pbcopy
    echo "✅ PR summary copied to clipboard (macOS)"
elif command -v xclip >/dev/null 2>&1; then
    # Linux with xclip
    echo "$SUMMARY" | xclip -selection clipboard
    echo "✅ PR summary copied to clipboard (Linux/xclip)"
elif command -v clip.exe >/dev/null 2>&1; then
    # WSL/Windows
    echo "$SUMMARY" | clip.exe
    echo "✅ PR summary copied to clipboard (Windows/WSL)"
else
    # Fallback: just output
    echo "$SUMMARY"
    echo ""
    echo "⚠️  Clipboard tool not found. Summary printed above."
fi
```