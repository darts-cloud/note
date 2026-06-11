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
| やり方・手順・汎用ノウハウを知りたい | `../06_knowledge/` 配下 |

## ボールト構造

```
content/
├── 00_Inbox/        # 新着情報の受け口（自動処理される）
├── 01_Projects/     # 進行中のプロジェクト
├── 02_Ideas/        # アイデア・調査内容
├── 03_Resources/    # 生の第一次資料
├── 04_Context/      # AI 向け文脈ファイル群（このフォルダ）
├── 05_flow/         # アイデア管理（PDCA）
├── 06_knowledge/    # 再利用可能なノウハウ・How-to・ガイドライン
├── 09_memory/       # Claude メモリファイル
├── 10_dairy/        # 日次日記
├── 11_evening-meeting/ # 夕会ログ
└── 99_tools/        # エージェントプロンプト
```
