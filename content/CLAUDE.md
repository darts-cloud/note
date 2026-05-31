# CLAUDE.md

このファイルは、リポジトリで作業する Claude Code (claude.ai/code) へのガイダンスを提供します。

## 基本方針

- **やりとりは必ず日本語で返答すること**
- **作業開始前に `operations-guide.md`（ルート直下）を読み込み、運用方針を把握すること**

## 概要

Obsidian ボールト兼個人ナレッジ管理システムで、[Quartz](https://quartz.jzhao.xyz/) を使ってデジタルガーデンとして公開しています。`content/` ディレクトリが Obsidian ボールトのルートであり、git リポジトリのルートでもあります。

## ローカルプレビュー

ビルドコマンドは**親ディレクトリ（`../`）**から実行します：

```bash
cd ../quartz && npm i
npx quartz build -d ../content --serve
```

## ディレクトリ構造

| ディレクトリ | 用途 | ファイル命名 |
|---|---|---|
| `00_template/` | Obsidian テンプレート（日記テンプレート等） | — |
| `01_dairy/` | 日次日記エントリ | `YYYYMMDD.md` |
| `02_idea/` | アイデア管理（PDCA ワークフロー） | トピックごとのサブディレクトリ |
| `03_evening-meeting/` | 夕会ログ | `YYYY-MM-DD.md` |
| `04_knowledge/` | 参考・ナレッジ文書 | — |
| `memory/` | Claude AI メモリファイル（サイトコンテンツには含まない） | — |
| `tools/` | エージェントプロンプト（`/schedule` ルーティン等） | — |

## フロントマター規約

すべての Markdown ファイルに YAML フロントマターを付与します：

```yaml
---
title: "<タイトル>"
tags:
  - <カテゴリタグ>
---
```

- 日記エントリ: `tags: [diary]`
- 夕会ログ: `tags: [evening-meeting]`
- 棚卸しレポート: `tags: [inventory]`

## アイデアワークフロー（`02_idea/`）

詳細ルールは `02_idea/CLAUDE.md` を参照。概要：

- アイデアはトピックごとにサブディレクトリを作成し、5 つの番号付きファイルを順に作成する：`1.案出し.md` → `2.迷っている点.md` → `3.AIとの深掘り.md` → `4.課題.md` → `5.決定内容.md`
- ステータスはトピックディレクトリを移動して管理：`1_PLAN/` → `2_DO/` → `3_CHECK/` → `4_COMPLATE/`
- **`1.案出し.md` はユーザーが記述するファイルのため、既存の内容を消さず追記のみ行うこと**
- ワークフローはテキストと Mermaid 図の両方で記述すること（Mermaid は概要のみ、詳細は書きすぎない）

## 日記エントリ（`01_dairy/`）

完了タスクは `- [x]`、持ち越しタスクは `- [ ]` 形式で記載します。

## スケジュール棚卸し

`tools/inventory-prompt.md` を `/schedule` ルーティンのプロンプトとして使用します。未完了タスク・アイデア進捗・直近の git 活動をスキャンし、レポートを `memory/inventory-YYYYMMDD.md` に書き出します。
