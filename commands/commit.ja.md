# Git コミットガイドライン

[English](commit.md) | [日本語](commit.ja.md)

このプロジェクトでコミットを行う際の必須ルール。

## 事前チェック

1. **変更内容の確認**
   ```bash
   git status
   git diff
   ```

2. **関連性の確認**
   - 関連する変更のみを同じコミットに含める
   - 無関係な変更は別コミットにする

## ファイル追加ルール

1. **個別指定必須**
   ```bash
   # ❌ 禁止
   git add .
   
   # ✅ 必須：個別指定
   git add specific_file1.js specific_file2.tsx
   ```

2. **引数指定による対象限定**
   - コマンド引数でパスを指定された場合、そのパス配下のファイルのみを対象とする
   - 例：`/commit src/` → src/ディレクトリ配下のファイルのみ
   - 例：`/commit main.js components/` → main.jsと、components/ディレクトリ配下のファイルのみ
   
   ```bash
   # パス指定例
   git status                    # 全体確認
   git add src/player.js        # 指定パス配下のみ
   git add components/main.tsx  # 指定パス配下のみ  
   git diff --staged           # ステージング確認
   ```

3. **関連ファイルをまとめる**
   - 機能追加なら関連する全ファイルを同じコミットに
   - 削除なら古いファイル削除も同じコミットに含める
   - ただし引数でパスが指定された場合は、そのパス制限を優先する

## コミットメッセージフォーマット

このプロジェクトでは、英語を主要言語とし、日本語翻訳を含むバイリンガルコミットメッセージを使用します：

### 基本フォーマット
```
<英語のコミットメッセージ>

<日本語翻訳>
```

### 例
```
Add user authentication feature

ユーザー認証機能を追加
```

```
Fix database connection timeout issue

データベース接続のタイムアウト問題を修正
```

### 詳細フォーマット
詳細が必要な場合：

```
<英語の要約>

<英語の詳細説明>

<日本語の要約>

<日本語の詳細説明>
```

### ガイドライン
- 1行目：明確で簡潔な英語メッセージ（命令形）
- 2行目：空行
- 3行目：英語の詳細説明（必要な場合）
- 4行目：空行（詳細説明がある場合）
- 5行目：日本語翻訳
- 要約メッセージは可能な限り72文字以内に収める
- 従来のコミットプレフィックスを使用：feat:, fix:, docs:, style:, refactor:, test:, chore:

### 詳細例
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

## コミット後作業

1. **TODO更新**
   - `TodoWrite`でタスクを完了済みに更新

## トラブルシューティング

- **コミット分割**：`git reset --soft HEAD~1`で取り消し後、再分割
- **メッセージ修正**：`git commit --amend`
- **ファイル追加忘れ**：`git add file && git commit --amend --no-edit`

## 重要な制限事項

- **`git reset --hard`は原則禁止** - このコマンドはコミットされていない変更を永久に破棄します
  - 代わりに`git reset --soft`または`git reset --mixed`を使用してください
  - 変更を破棄する必要がある場合は、先に`git stash`で保存してください