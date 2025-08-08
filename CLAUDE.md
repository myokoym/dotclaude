# Claude Code Project Configuration

## Important: Slash Command Format Rules

**All Claude Code slash commands MUST be created as Markdown (.md) files, NOT shell scripts (.sh) or other formats.**

According to the [official documentation](https://docs.anthropic.com/en/docs/claude-code/slash-commands):
- Commands are stored in `.claude/commands/` directory
- Command files MUST have `.md` extension
- Command name is derived from the filename (without .md extension)
- Files support frontmatter for metadata (allowed-tools, description, model)
- Can include bash command execution, file references, and arguments via placeholders

### Correct Format Example:
```markdown
---
allowed-tools: Bash
description: Your command description
---

## Context
- Current status: !`command to run`

## Your task
Instructions for the command...
```

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