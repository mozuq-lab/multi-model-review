# Multi-Model Code Review

LLM のコードレビュー結果を GitHub Copilot CLI に精査させ、双方の視点を統合した最終レビューを提供する。

## レビュー対象

$ARGUMENTS

## Flow

1. まず自分でコードレビューを実施（Critical Issues / Warnings / Suggestions形式）
2. Copilot CLI に精査＋独自レビューを依頼
3. Copilot の回答を受けて再検討
4. 必要に応じて再度 Copilot 呼び出し
5. 双方納得の結論をユーザーに回答

## Step 1: 自分でコードレビュー

対象コードをレビューし、以下の観点で問題点を洗い出す：

- バグ・ロジックエラー
- セキュリティ脆弱性
- パフォーマンス問題
- コード品質・保守性
- ベストプラクティス違反

## Step 2: Copilot CLI で精査＋独自レビュー

以下のコマンドで Copilot CLI を呼び出す：

```bash
copilot -p "<プロンプト>" --add-dir . --allow-all-tools --model gpt-5 -s
```

### プロンプトテンプレート

```text
I performed a code review and want you to both verify my findings AND conduct your own independent review.

## Review Target
<レビュー対象>

## Code Changes
<対象コードまたはgit diffの概要>

## My Review Findings

### Critical Issues
<Critical Issues>

### Warnings
<Warnings>

### Suggestions
<Suggestions>

---

Please:

**Part 1: Verify my review**
1. Reading the actual source files to confirm my findings are accurate
2. Pointing out if any of my findings are incorrect or overstated

**Part 2: Your independent review**
3. Conduct your own code review within the specified Review Target scope
4. List any issues I missed

Respond with:
- Confirmed: Issues you agree with
- Disputed: Issues you disagree with (explain why)
- Your Findings: Issues from your independent review
- Additional context: Relevant information from reading the source
```

## Step 3-4: 再検討と追加確認

Copilot の精査結果を受けて：
- **Confirmed** → 最終レビューに含める
- **Disputed** → 反論を検討、必要なら再度 Copilot に確認
- **Your Findings** → 妥当なら追加

## Step 5: 最終レビュー出力

```markdown
## Multi-Model Code Review Summary

**Reviewers:** [あなたのモデル名] + GitHub Copilot [モデル名]

### Critical Issues
- [双方合意の問題]
- [Copilot が追加で発見した問題]

### Warnings
- [警告事項]

### Suggestions
- [改善提案]

### Review Discussion
- **合意点:** 双方が同意した主要な問題
- **議論点:** 見解が分かれた点とその結論
- **補完:** 片方のみが発見した問題
```

## 利用可能なモデル

| Model                  | Notes                  |
| ---------------------- | ---------------------- |
| `gpt-5`                | デフォルト、バランス型 |
| `gpt-5.1-codex-max`    | コード特化、高性能     |
| `gemini-3-pro-preview` | Google 製              |
