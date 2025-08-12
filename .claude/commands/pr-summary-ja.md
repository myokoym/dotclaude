---
allowed-tools: Bash
description: PR概要を日本語フォーマットで生成し、クリップボードにコピー
---

## コンテキスト
- 現在のブランチ: !`git branch --show-current`
- ベースブランチ: !`git symbolic-ref refs/remotes/origin/HEAD 2>/dev/null | sed 's@^refs/remotes/origin/@@' || echo "main"`

## タスク

Pull Request概要を日本語フォーマットで生成し、クリップボードにコピーします。

### フォーマット仕様：
1. タイトル（[種別]: 要約）
2. 変更理由・背景
3. 影響範囲
4. 動作確認
5. レビュー観点
6. 関連Issue

以下のスクリプトを実行してPR概要を生成：

```bash
#!/bin/bash

# ベースブランチとカレントブランチを取得
BASE_BRANCH=$(git symbolic-ref refs/remotes/origin/HEAD 2>/dev/null | sed 's@^refs/remotes/origin/@@' || echo "main")
CURRENT_BRANCH=$(git branch --show-current)

# ベースブランチにいる場合はエラー
if [ "$CURRENT_BRANCH" = "$BASE_BRANCH" ]; then
    echo "エラー: ベースブランチ ($BASE_BRANCH) にいます。フィーチャーブランチに切り替えてください。" >&2
    exit 1
fi

# コミット情報を取得
COMMITS=$(git log --oneline "$BASE_BRANCH".."$CURRENT_BRANCH" --reverse)
COMMIT_COUNT=$(echo "$COMMITS" | wc -l | xargs)

# 最新のコミットメッセージから種別を推測
LATEST_COMMIT=$(git log -1 --pretty=%s)
if [[ $LATEST_COMMIT =~ ^(feat|fix|docs|style|refactor|test|chore): ]]; then
    PREFIX="${BASH_REMATCH[1]}"
    TITLE_BODY="${LATEST_COMMIT#*: }"
else
    PREFIX="feat"
    TITLE_BODY="$LATEST_COMMIT"
fi

# 変更ファイルリストを取得
FILES=$(git diff --name-only "$BASE_BRANCH"..."$CURRENT_BRANCH")
FILE_COUNT=$(echo "$FILES" | wc -l | xargs)

# 変更の統計を取得
STATS=$(git diff --stat "$BASE_BRANCH"..."$CURRENT_BRANCH" | tail -n 1)

# PR概要を生成
SUMMARY="# [$PREFIX]: $TITLE_BODY

## 概要
<!-- このPRで何を実現するかを1-2文で簡潔に説明 -->
（ここに概要を記載）

## 変更理由・背景
<!-- なぜこの変更が必要なのか、解決したい課題は何か -->
- 

## 変更内容
<!-- 具体的な変更点をリスト形式で記載 -->
$(echo "$COMMITS" | sed 's/^[a-f0-9]\{7\} /- /')

## 影響範囲
<!-- 変更により影響を受ける機能・画面・APIなど -->
- 影響を受ける機能: 不明
- 副作用の可能性: なし

## 動作確認
<!-- 確認した内容にチェック -->
- [ ] 主要機能の動作確認済み
- [ ] エラーケースの確認済み
- [ ] 既存機能への影響なし

### 確認手順
1. 
2. 
3. 

## レビュー観点
<!-- 特に確認してほしいポイントや相談事項 -->
- 

## 関連Issue・タスク
<!-- closes #番号 または ref. #番号 -->
- 

---

### 技術詳細
<details>
<summary>変更ファイル一覧（$FILE_COUNT ファイル）</summary>

\`\`\`
$FILES
\`\`\`
</details>

<details>
<summary>コミット履歴（$COMMIT_COUNT コミット）</summary>

\`\`\`
$COMMITS
\`\`\`
</details>

<details>
<summary>変更統計</summary>

\`\`\`
$STATS
\`\`\`
</details>

---
*ブランチ: \`$CURRENT_BRANCH\` → \`$BASE_BRANCH\`*"

# クリップボードにコピー
if command -v pbcopy >/dev/null 2>&1; then
    # macOS
    echo "$SUMMARY" | pbcopy
    echo "✅ PR概要をクリップボードにコピーしました (macOS)"
elif command -v xclip >/dev/null 2>&1; then
    # Linux with xclip
    echo "$SUMMARY" | xclip -selection clipboard
    echo "✅ PR概要をクリップボードにコピーしました (Linux/xclip)"
elif command -v clip.exe >/dev/null 2>&1; then
    # WSL/Windows
    echo "$SUMMARY" | clip.exe
    echo "✅ PR概要をクリップボードにコピーしました (Windows/WSL)"
else
    # フォールバック: 出力のみ
    echo "$SUMMARY"
    echo ""
    echo "⚠️  クリップボードツールが見つかりません。上記の内容を手動でコピーしてください。"
fi

echo ""
echo "📝 PR概要が生成されました。必要な箇所を編集してからPRを作成してください。"
echo "   特に以下の項目を確認・記入してください："
echo "   - 概要"
echo "   - 変更理由・背景"
echo "   - 動作確認の手順"
echo "   - レビュー観点"
```