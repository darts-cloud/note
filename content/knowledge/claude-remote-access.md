---
title: AIへのリモートアクセス方法
tags: [knowledge, memory, remote-access]
---

# AIへのリモートアクセス方法

## 概要

AI（Code CLI）に対してリモートから指示・操作する主な方法をまとめる。

---

## 1. AI API

最も直接的な方法。HTTPリクエストでモデルを呼び出す。

```bash
curl https://api.anthropic.com/v1/messages \
  -H "x-api-key: $API_KEY" \
  -H "anthropic-version: 2023-06-01" \
  -H "content-type: application/json" \
  -d '{
    "model": "claude-sonnet-4-6",
    "max_tokens": 1024,
    "messages": [{"role": "user", "content": "Hello"}]
  }'
```

- **用途:** アプリ・スクリプトからAIを呼び出す
- **認証:** `API_KEY` 環境変数

---

## 2. Code CLI（SSH経由）

サーバー上で動いているCode CLIに対してSSHで接続して操作する。

```bash
# リモートマシンにSSH接続してCLIを起動
ssh user@server

# ノンインタラクティブ実行（スクリプトから）
ssh user@server "code-cli --print 'ファイル一覧を出して'"
```

- **用途:** リモートサーバー上のコードベースを操作
- **前提:** サーバーに Code CLI がインストール済み

---

## 3. Discord連携（現在の構成）

Discordのメッセージを受信してAIが自動応答・処理する構成。

```
Discord メッセージ
    ↓
MCP Plugin (plugin:discord:discord)
    ↓
Code CLI セッション
    ↓
fetch_messages / reply ツール
```

- **設定場所:** `~/.claude/settings.json` のMCPサーバー設定
- **スキル:** `/discord-watch` でDiscordを監視
- **用途:** チャットからAIに指示を出す、通知を受け取る

---

## 4. Remote Trigger（スケジュール実行）

スケジュール機能でリモートエージェントを定期実行する。

```bash
# スキルから設定
/schedule

# cronライクな指定で定期実行
# 例: 毎日18時に夕会スキルを実行
```

- **ツール:** `CronCreate` / `RemoteTrigger`
- **用途:** 定期レポート、監視、自動化タスク
- **スキル:** `/loop`、`/schedule`

---

## 5. Web / Desktop App

ブラウザやデスクトップアプリからアクセス。

- **用途:** どこからでもGUIで操作
- **対応:** Mac / Windows デスクトップアプリ、ブラウザ

---

## 6. IDE拡張（VS Code / JetBrains）

エディタに統合してリモートリポジトリ上のコードを操作。

```
VS Code Remote SSH + 拡張
→ リモートサーバーのコードをローカルIDEで編集しながら指示
```

---

## 比較まとめ

| 方法 | リアルタイム性 | 自動化 | セットアップ難度 |
|---|---|---|---|
| AI API | ○ | ◎ | 低 |
| SSH + CLI | ○ | ○ | 中 |
| Discord連携 | ◎ | ◎ | 中（現在使用中） |
| Remote Trigger | △（定期） | ◎ | 中 |
| Web/Desktop | ◎ | △ | 低 |
| IDE拡張 | ◎ | △ | 低 |

---

## 現在の構成

```
Discord → MCP plugin → Code CLI（~/claude）
                              ↓
                    ~/note/content（Obsidian記録）
```
