# コーディング規約

## .gitignore の設定

[.gitignore](https://github.com/Dioptra-jp/.github/blob/main/.gitignore)をベースに作成することを推奨する．

作成時は以下のルールに従って設定：

- 環境固有のファイル（IDE 設定、OS の一時ファイルなど）は無視する
- ビルド成果物やキャッシュファイルは常に無視する
- 依存関係のディレクトリ（`node_modules`、`vendor`など）はリポジトリに含めない
- 機密情報（API キー、パスワード、証明書）は必ず`.gitignore`に追加する
- プロジェクト固有の`.gitignore`テンプレートは[GitHub のリポジトリ](https://github.com/github/gitignore)を参照

## ブランチ命名

ルール: [Conventional Branch](https://conventional-branch.github.io/) を採用
フォーマット: `<type>/<description>-<optional ticket>`

type の例:

- main: メインブランチ
- develop(dev): 次期リリースのための開発ブランチ
- feature/: 新機能のため
- bugfix/: バグ修正のため
- hotfix/: 緊急修正のため
- release/: リリース準備のためのブランチ
- chore/: 依存関係やドキュメント更新などのコード以外のタスクのため

ticket は projects の課題番号 (#000)を記載．
短い説明だけでも意味が通るようにする．

ブランチ名の例:

- feature/add-login-123
- fix/null-pointer-456
- chore/update-deps
- release/v1.2.0

## コミット

ルール: [Conventional Commits](https://www.conventionalcommits.org/ja/v1.0.0/) を採用

コミットは履歴を明確に保ち，コードレビュー・デバッグ・リリース管理を効率化するために，意味のある最小単位でコミットを行うこと．

基本原則:

- コミットは小さく，意味のある単位で行う
- 1 コミット = 1 目的
- 関係のない変更を 1 つのコミットにまとめない
- レビューしやすい粒度を意識する

コミットは以下のフォーマットに従う．
フォーマット:

```
<type>[optional scope]: <description>

[optional body]

[optional footer(s)]
```

type の例:

- feat: 新機能
- fix: バグ修正
- docs: ドキュメントのみの変更
- style: フォーマット等のコード差分を伴わない変更
- refactor: リファクタリング
- perf: パフォーマンス改善
- test: テスト追加／修正
- chore: ビルドプロセスや補助ツールの変更

type, scope の後ろに`!`が付与されているコミットは API の破壊的変更を意味する．

footer の例:

- BREAKING CHANGE: `!`と同様に破壊的変更を示す
- Refs: コミットの SHA や関連するコミットを参照する際に使用
- Closes: issue を閉じる際に使用

<details>
<summary>コミットメッセージの例集</summary>

---

### シンプルな機能追加（スコープあり）

```
feat(auth): add OAuth2 login flow
```

### 機能追加（スコープなし）

```
feat: enable dark mode for UI
```

### 機能追加 + 本文 + 関連 issue 終了

```
feat(profile): add user profile page

Added profile view, edit form and avatar upload.

- Added validation
- Adopted async upload

Closes: #789
```

### バグ修正（簡潔）

```
fix(parser): avoid crash on empty input
```

### バグ修正 + 本文 + テスト追加の注記

```
fix(session): restore session handling for legacy tokens

Cause: Session restoration failed because the token parsing logic allowed empty tokens.
Fix: Added a fallback in the parsing logic and added a unit test.

Refs: 3f4a2bc
```

### 破壊的変更（`!`を使用）

```
fix!: change password hashing to argon2

Password hashes changed from bcrypt -> argon2. Existing users must reset their passwords or run a migration tool.

BREAKING CHANGE: password hashes must be migrated. See docs/migrations/password-hash.md
```

### 破壊的変更（フッターで明示）

```
refactor(db): move user fields to tenant-scoped table

A refactor involving major schema changes.

- Move users -> tenants_users
- Include migration script

BREAKING CHANGE: DB schema changed. Run db:migrate and follow the migration guide.
Closes: #456
```

### パフォーマンス改善

```
perf(cache): reduce expensive query frequency

- Cache calls to the slow API
- Set TTL to 5 minutes
```

### ドキュメントのみ

```
docs(contributing): update commit message examples
```

### スタイル/リンターのみ（コード変更なし）

```
style: run prettier across repo
```

### テスト追加

```
test(auth): add integration tests for login flow

- Cover success/failure cases
    Co-authored-by: Contributor <contrib@example.com>
```

### 依存関係の更新（雑務）

```
chore(deps): bump library-x to v2.4.0

- No breaking changes
- Pay attention to the release notes
```

### CI / ビルド

```
ci: add GitHub Actions workflow for lint & test
build(docker): reduce image size by using multi-stage build
```

### 差し戻し

```
revert: revert "feat(profile): add user profile page"

This reverts commit 7a1b2c3 due to a regression in avatar upload handling.
```

### ホットフィックス（緊急）

```
fix(hotfix): restore session cookie domain for production

Emergency fix. Merge to main and deploy immediately.
```

---

</details>

コミットメッセージは日本語も許容される．
description は命令形で簡潔に記述し，body が必要な場合は空行で本文を追加．
メッセージの記述には copilot による自動生成も積極的に活用することを推奨する．
自動生成されたメッセージは必ず内容を確認し，必要に応じて修正すること．

## プルリクエスト

PR 発行時には，テンプレートの使用を推奨する．
他者にレビューを依頼する際には使用を必須とする．

発行手順:

1. 自身でテストを行い動作確認を行う
2. Copilot によるコードレビューを実施する
3. Copilot のレビュー結果に基づいて必要な修正を行う
4. レビュワーをアサインする
5. レビュワーからのフィードバックに基づいて修正を行う
6. すべての修正が完了し，承認された後レビュワーが PR をマージしクローズする

発行する PR に適したテンプレートを以下より選択:

- [pull_request_template.md](pull_request_template.md) (デフォルト)
- [PULL_REQUEST_TEMPLATE/docs_pr.md](PULL_REQUEST_TEMPLATE/docs_pr.md)

<!--

## マージとリリース

- 原則: Squash and merge を推奨（コミット履歴を PR 単位でまとめる）
- リリースタグは Semantic Versioning (MAJOR.MINOR.PATCH)
- changelog は自動生成（Conventional Commits ベースのツール活用を推奨）

## 自動化とチェック

- pre-commit でフォーマット，静的解析を実行
- commit-msg hook で commitlint を実行しメッセージを検証
- GitHub Actions: push/PR 時に linter/test/build/security-scan を実行
- ブランチ保護: 必須チェック（status checks），必須レビュー数を設定
  -->

## 運用上の注意点とエッジケース

- 小さな修正（typo 等）でも commit message は意味が分かるようにする．
- 緊急の hotfix は `hotfix/` ブランチを切り，後で main にリベース/マージする手順を明記．
- 既存コミットを書き換えるのはリスクが高いため原則行わない．必要ならば小規模な履歴書き換え手順（`git-filter-repo` 等）を別途作成し，許可された場合のみ実施
