# Multi-Model Review

Claude Code と GitHub Copilot CLI を組み合わせたマルチモデルコードレビュープラグイン。

## 概要

コードレビューを単一の AI モデルに任せるのではなく、複数のモデルで相互検証することで、より網羅的で信頼性の高いレビューを実現します。

### フロー

```text
ユーザー依頼
    ↓
[Step 1] LLM（Claude等）がコードレビュー
    ↓
[Step 2] GitHub Copilotに精査＋独自レビューを依頼
    ↓
[Step 3] Copilotの回答を受けて再検討
    ↓
[Step 4] 確認事項があれば再度Copilot呼び出し
    ↓
[Step 5] 双方納得の結論をユーザーに回答
```

### 特徴

- **相互検証**: 一方のモデルのレビュー結果を他方が精査＋独自レビュー
- **自律的ファイル参照**: Copilot CLI がエージェントとして必要なファイルを自分で読み込み
- **スコープ指定**: コミット、PR、特定ファイル等の範囲内でレビュー
- **双方向やり取り**: 見解の相違があれば複数回のやり取りで解決
- **統合レビュー**: 双方の視点を統合した最終レビューを提供

## 前提条件

- [Claude Code](https://docs.anthropic.com/en/docs/claude-code) がインストールされていること
- [GitHub Copilot CLI](https://docs.github.com/en/copilot/using-github-copilot/using-github-copilot-in-the-command-line) がインストールされていること

```bash
# Copilot CLIの確認
copilot --version
```

## インストール

### 1. マーケットプレイスとして追加

```claude
/plugin marketplace add https://github.com/mozuq-lab/multi-model-review.git
```

### 2. プラグインをインストール

```claude
/plugin install multi-model-review@multi-model-review
```

## 使い方

### スラッシュコマンド

Claude Code 内で以下のコマンドを実行：

```
/multi-model-review
```

### 自然言語

```
マルチモデルレビューをお願いします
このPRをセカンドオピニオンでレビューして
複数モデルでレビューして
```

### 引数

```
/multi-model-review gpt-5.1          # モデル指定
/multi-model-review --file src/main.py  # ファイル指定
```

## 利用可能な Copilot モデル

| Model                  | Notes                  |
| ---------------------- | ---------------------- |
| `gpt-5`                | デフォルト、バランス型 |
| `gpt-5.1`, `gpt-5.2`   | 新バージョン           |
| `gpt-5.1-codex-max`    | コード特化、高性能     |
| `gpt-5-mini`           | 高速                   |
| `gemini-3-pro-preview` | Google 製              |

## 出力例

```markdown
## Multi-Model Code Review Summary

**Reviewers:** Claude Sonnet + GitHub Copilot gpt-5

### Critical Issues

- [双方合意の問題]

### Warnings

- [警告事項]

### Suggestions

- [改善提案]

### Review Discussion

- **合意点:** 双方が同意した主要な問題
- **議論点:** 見解が分かれた点とその結論
- **補完:** 片方のみが発見した問題
```

## ライセンス

MIT
