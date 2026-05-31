---
title: Claude Codeをリモートで使う方法
tags: [knowledge, memory, remote-access, claude-code]
---

## Claude Codeをリモートで使う方法：概要

Claude Code（CLIツール）に対して、スマートフォンや外部環境からリモートで操作する主な手法とワークフローをまとめます。

---

## パターン別ワークフロー

### パターン1：外出先のスマホから記事・コードを修正したい

**→ SSH + ヘッドレスモード**

```bash
# スマホ（Termux等）から手元のサーバーにSSH接続
ssh user@your-server

# サーバー上でClaude Codeをノンインタラクティブ実行
claude -p "content/knowledge/remote-access.md の誤字を修正して"
```

**手順**:
1. スマホに **Termux**（Android）または **iSH / Blink**（iOS）をインストール
2. `ssh user@your-server` でサーバーへログイン
3. `claude -p "指示"` で自然言語で指示を出す
4. 結果を確認・必要なら追加指示

**メリット**: PCなしでもフリック入力だけでドキュメント・コード操作が可能

---

### パターン2：ブラウザだけでClaude Codeを使いたい

**→ Claude Code on the Web（claude.ai/code）**

1. ブラウザで `claude.ai/code` を開く
2. Anthropicアカウントでログイン（Pro/Max/Team/Enterpriseプラン必須）
3. Anthropic管理のクラウドVM上でセッションが起動する

**特徴**:
- ブラウザを閉じてもセッションが継続
- スマホのブラウザからも監視・操作可能
- GitHubリポジトリと連携したPR自動修正が可能

---

### パターン3：PCで起動したClaude Codeをスマホから監視・操作したい

**→ Remote Control機能**

```bash
# PCのClaude Codeでリモートコントロールを有効化
claude remote-control --verbose
# → QRコードが表示される
```

スマホの **Claude公式アプリ** または `claude.ai/code` でQRコードをスキャンするとセッションに接続できます。

**注意事項**:
- PCのプロセスが起動し続けている必要がある
- ネットワーク断絶が10分以上続くとセッション終了
- claude.aiサブスクリプション必須（APIキーでは不可）

---

### パターン4：CI/CDやGitHub Actionsで自動実行したい

**→ GitHub Actions連携**

```yaml
# .github/workflows/claude-review.yml
- uses: anthropics/claude-code-action@v1
  with:
    anthropic_api_key: ${{ secrets.ANTHROPIC_API_KEY }}
    prompt: "このPRのセキュリティ問題をレビューして"
    claude_args: "--max-turns 5 --model claude-opus-4-7"
```

**セットアップ**:
1. Claude Codeで `/install-github-app` を実行
2. または https://github.com/apps/claude からGitHub Appをインストール
3. リポジトリのSecretsに `ANTHROPIC_API_KEY` を追加

**用途**: PRレビュー自動化・ドキュメント生成・テスト実行

---

### パターン5：スクリプトや自動化に組み込みたい

**→ ヘッドレスモード（`claude -p`）**

```bash
# 基本
claude -p "src/以下のTODOコメントを一覧にして"

# JSON形式で出力（パイプ処理向き）
claude -p "依存パッケージの脆弱性を調べて" --output-format json

# 許可ツールを明示（CI推奨）
claude -p "バグを修正して" --allowedTools "Bash,Read,Edit"

# ストリーミング出力
claude -p "README.mdを更新して" --output-format stream-json --verbose
```

**cronでの定期実行例**:
```bash
# 毎日朝8時にドキュメントの整合性チェック
0 8 * * * cd ~/note && claude -p "content/以下のリンク切れを確認して報告して" >> ~/logs/claude.log 2>&1
```

---

### パターン6：APIを直接叩いてアプリやスクリプトに組み込みたい

**→ Anthropic Messages API**

```bash
curl https://api.anthropic.com/v1/messages \
  -H "x-api-key: $ANTHROPIC_API_KEY" \
  -H "anthropic-version: 2023-06-01" \
  -H "content-type: application/json" \
  -d '{
    "model": "claude-opus-4-7",
    "max_tokens": 1024,
    "messages": [{"role": "user", "content": "Hello"}]
  }'
```

**利用可能なモデル（2026年4月現在）**:

| モデル | 用途 |
|---|---|
| `claude-opus-4-7` | 最高性能・複雑なタスク |
| `claude-sonnet-4-6` | バランス型（Claude Code自身が使用） |
| `claude-haiku-4-5-20251001` | 軽量・高速 |

---

### パターン7：チャットUIからClaude Codeに指示したい（Discord等）

**→ MCP（Model Context Protocol）サーバー経由**

公式のDiscord連携は**存在しない**が、MCPサーバーを自作・利用することで実現できます。

**フロー**:
```
Discordメッセージ
    ↓
Discord Bot（Webhook受信）
    ↓
MCPサーバー（ローカル or クラウド）
    ↓
Claude Code（ヘッドレスモード）
    ↓
結果をDiscordへ返信
```

**設定（`~/.claude/claude_desktop_config.json`）**:
```json
{
  "mcpServers": {
    "discord": {
      "command": "node",
      "args": ["/path/to/discord-mcp-server.js"]
    }
  }
}
```

**注意**: サードパーティMCPサーバーはプロンプトインジェクションのリスクがあるため、信頼できるソースのみ使用すること。

---

### パターン8：Claude Codeの完了・エラーをリモートで通知受け取りたい

**→ Hooks（イベントフック）**

Hooksは`settings.json`に設定するシェルコマンドで、Claude Codeの各イベントに応じて自動実行されます。長時間の処理をリモートで走らせるときに「終わったら通知」する用途に最適です。

**設定ファイル（`~/.claude/settings.json`）**:
```json
{
  "hooks": {
    "Stop": [
      {
        "matcher": "",
        "hooks": [
          {
            "type": "command",
            "command": "curl -s -X POST https://discord.com/api/webhooks/YOUR_WEBHOOK_URL -H 'Content-Type: application/json' -d '{\"content\": \"Claude Code が完了しました\"}'"
          }
        ]
      }
    ],
    "Notification": [
      {
        "matcher": "",
        "hooks": [
          {
            "type": "command",
            "command": "curl -s -X POST https://discord.com/api/webhooks/YOUR_WEBHOOK_URL -H 'Content-Type: application/json' -d '{\"content\": \"Claude Codeからの通知\"}'"
          }
        ]
      }
    ]
  }
}
```

**主なイベント**:

| イベント | タイミング |
|---|---|
| `Stop` | Claude Codeがレスポンスを終えたとき |
| `Notification` | 入力待ちや許可待ちになったとき |
| `PreToolUse` | ツール実行前 |
| `PostToolUse` | ツール実行後 |
| `SubagentStop` | サブエージェント完了時 |

**活用パターン**:
```bash
# 通知用スクリプト例（~/bin/notify-discord.sh）
#!/bin/bash
MESSAGE="${1:-Claude Codeが完了しました}"
curl -s -X POST "$DISCORD_WEBHOOK_URL" \
  -H "Content-Type: application/json" \
  -d "{\"content\": \"$MESSAGE\"}"
```

```json
{
  "hooks": {
    "Stop": [
      {
        "matcher": "",
        "hooks": [
          {
            "type": "command",
            "command": "~/bin/notify-discord.sh '作業完了！'"
          }
        ]
      }
    ]
  }
}
```

**注意**: Hooksのコマンドはセキュリティ上の重要な処理を行うため、外部入力を直接コマンドに渡さないこと（コマンドインジェクションのリスク）。

---

## IDE拡張でのリモート開発

### VS Code Remote Development

VS Codeのリモート開発機能（SSH/WSL/Dev Container）を利用する場合、リモートホスト側に Claude Code CLI をインストールすれば、IDE上でそのままClaude Codeが使えます。

```bash
# リモートホストにClaude Codeをインストール
npm install -g @anthropic-ai/claude-code
```

### JetBrains IDEs

リモート開発時は**リモートホスト側にもJetBrainsプラグインのインストールが必要**。

---

## パターン比較まとめ

| パターン | 方法 | リアルタイム | 自動化 | スマホ適性 | 難易度 |
|---|---|---|---|---|---|
| スマホから直接操作 | SSH + `claude -p` | ○ | ○ | ◎ | 低 |
| ブラウザのみ | claude.ai/code | ◎ | △ | ◎ | 最低 |
| PC連動監視 | Remote Control | ◎ | × | ◎ | 低 |
| CI/CD自動化 | GitHub Actions | △ | ◎ | × | 中 |
| スクリプト組込み | Headless `-p` | ○ | ◎ | △ | 低 |
| アプリ開発 | Messages API | ○ | ◎ | △ | 中 |
| チャットUI | MCP + Discord | ◎ | ◎ | ◎ | 高 |
| 完了通知受け取り | Hooks | △ | ◎ | ◎ | 低 |

---

## 現在の構成図

```
[各デバイス（スマホ・PC・ブラウザ）]
        ↓
(SSH / claude.ai/code / Remote Control / MCP)
        ↓
[Claude Code CLI]  ←→  [Anthropic API]
        ↓
[~/note/content/ (Obsidian記録)]
```

## 参考
- [Claude Code 公式ドキュメント](https://docs.anthropic.com/ja/docs/claude-code)
- Qiita記事: https://qiita.com/nogataka/items/e3a87b16ebb32b3a7281
