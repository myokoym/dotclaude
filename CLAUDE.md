# Claude Code Project Configuration

## Available Commands

### `/pr-summary`
Generate a markdown-formatted Pull Request summary and copy it to clipboard.

**Features:**
- Analyzes commits between current branch and base branch
- Shows changed files and statistics
- Automatically copies to clipboard without leading spaces
- Supports macOS, Linux (xclip), and WSL

**Usage:**
```bash
/pr-summary
```

The command will:
1. Detect your current branch and base branch
2. Generate a comprehensive PR summary including:
   - Branch information (current → base)
   - Number of commits
   - List of commits with messages
   - Modified files list
   - Diff statistics
3. Copy the formatted markdown to your clipboard

**Note:** You must be on a feature branch (not on main/master) to use this command.