[English](commit.md) | [日本語](commit.ja.md)

# Git Commit Guidelines

Essential rules for making commits in this project.

## Pre-commit Checks

1. **Review Changes**
   ```bash
   git status
   git diff
   ```

2. **Verify Relevance**
   - Include only related changes in the same commit
   - Separate unrelated changes into different commits

## File Addition Rules

1. **Individual Specification Required**
   ```bash
   # ❌ Prohibited
   git add .
   
   # ✅ Required: Individual specification
   git add specific_file1.js specific_file2.tsx
   ```

2. **Path Limitation by Arguments**
   - When a path is specified in command arguments, only files under that path are targeted
   - Example: `/commit src/` → Only files under src/ directory
   - Example: `/commit main.js components/` → main.js and files under components/ directory
   
   ```bash
   # Path specification example
   git status                    # Check all changes
   git add src/player.js        # Only under specified path
   git add components/main.tsx  # Only under specified path
   git diff --staged           # Verify staging
   ```

3. **Group Related Files**
   - For feature additions, include all related files in the same commit
   - For deletions, include removal of old files in the same commit
   - However, path restrictions from arguments take priority

## Commit Message Format

This project uses bilingual commit messages with English as the primary language and Japanese translation:

### Basic Format
```
<English commit message>

<Japanese translation>
```

### Examples
```
Add user authentication feature

ユーザー認証機能を追加
```

```
Fix database connection timeout issue

データベース接続のタイムアウト問題を修正
```

### Detailed Format
When more details are needed:

```
<English summary>

<English detailed description>

<Japanese summary>

<Japanese detailed description>
```

### Guidelines
- First line: Clear, concise English message (imperative mood)
- Second line: Empty line
- Third line: English detailed description (if needed)
- Fourth line: Empty line (if detailed description exists)
- Fifth line: Japanese translation
- Keep summary messages under 72 characters when possible
- Use conventional commit prefixes: feat:, fix:, docs:, style:, refactor:, test:, chore:

### Detailed Example
```
feat: Add multi-language support for documentation

This commit introduces internationalization support for all documentation files.
- Separated Japanese content into .ja.md files
- Translated all documentation to English as the primary language
- Added language switcher links to README files
- Updated package.json description to English

ドキュメントの多言語対応を追加

このコミットは全ドキュメントファイルの国際化対応を導入します。
- 日本語コンテンツを.ja.mdファイルに分離
- 全ドキュメントを英語（主要言語）に翻訳
- READMEファイルに言語切り替えリンクを追加
- package.jsonのdescriptionを英語に更新
```

## Post-commit Tasks

1. **Update TODO**
   - Update tasks to completed with `TodoWrite`

## Troubleshooting

- **Split commits**: `git reset --soft HEAD~1` to undo, then re-split
- **Fix message**: `git commit --amend`
- **Add forgotten files**: `git add file && git commit --amend --no-edit`

## Important Restrictions

- **`git reset --hard` is strictly prohibited** - This command permanently destroys uncommitted changes
  - Use `git reset --soft` or `git reset --mixed` instead
  - If you need to discard changes, use `git stash` to save them first