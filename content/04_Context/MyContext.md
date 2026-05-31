---
title: MyContext
tags:
  - context
  - ai
---

# AI アライバルプロトコル

このボールトを訪れた AI はまずこのファイルを読むこと。

## 読み込み順序

1. このファイル（MyContext.md）— 全体の目次
2. `AI-Handoff.md` — 他の AI からの引き継ぎログ

## タスク別の読み込みガイド

| タスクの種類 | 読むべきファイル |
|---|---|
| 自己紹介・人物像を知りたい | `ProfessionalIdentity.md` |
| 進行中のプロジェクトを把握したい | `CurrentProjects.md` |
| 文章・コンテンツを書く | `VoiceMessaging.md`, `PhilosophyValues.md` |
| 仕事のやり方・書類フォーマット | `WorkingStyle.md` |
| 機材・ソフトウェア構成を知りたい | `TechnicalSetup.md` |

## ボールト構造

```
content/
├── Inbox/           # 新着情報の受け口（自動処理される）
├── Context/         # AI 向け文脈ファイル群（このフォルダ）
├── 01_dairy/        # 日次日記
├── 02_idea/         # アイデア管理（PDCA）
├── 03_evening-meeting/ # 夕会ログ
├── 04_knowledge/    # 参考・ナレッジ文書
├── memory/          # Claude メモリファイル
└── tools/           # エージェントプロンプト
```
