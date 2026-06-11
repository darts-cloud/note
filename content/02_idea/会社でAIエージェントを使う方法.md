---
title: 会社でAIエージェントを使う方法
tags:
  - idea
  - ai
  - work
date: 2026-06-11
status: 検討中
---

# 会社でAIエージェントを使う方法

## 状況

- 会社でClaude Code（AIエージェント）を使いたい
- 使用禁止ポリシーあり（機密情報保護のため）
- 携帯は使用可能

## 選択肢

### 案1: Claude.ai モバイルアプリ（即時可能）

- スマホのClaudeアプリを使う
- **条件**: 会社の機密情報・コードを貼り付けない
- 一般的なプログラムパターンや設計の相談には使える
- ポリシー上の「Claude Code（CLI）」とは別物として解釈できる余地あり

### 案2: GitHub Copilot（会社承認を取りやすい）

- MicrosoftのEnterpriseプランはデータが学習に使われない保証あり
- 多くの企業がClaude Code禁止でもCopilotは許可している
- 会社のIT部門に提案しやすい

### 案3: ローカルLLM（完全オフライン）

- Ollama + LLaMA / Gemma 等をPCやスマホで動かす
- データが外部に出ない → 機密情報を扱っても安全
- スマホアプリ: 「Pocketpal AI」「MiniCPM」など
- **デメリット**: 性能がClaude Codeより落ちる

### 案4: 会社のクラウドサービス経由（Azure / AWS）

- Azure OpenAI / AWS Bedrock は企業向け契約でデータ保護あり
- すでに契約している可能性がある
- IT部門に確認する価値あり

### 案5: Anthropic Claude for Enterprise を提案

- データが学習に使われないAnthropicの企業プラン
- 「Claude Code禁止」の理由が機密保護なら、Enterpriseで解決できると提案

## 推奨アクション

1. **今すぐ**: スマホのClaudeアプリで機密情報を入れない範囲で使う
2. **短期**: IT部門にGitHub Copilot Enterprise or Azure OpenAIを提案
3. **中期**: Claude for Enterprise導入提案を上申する

## 注意

- 「使用禁止」の範囲（社内PCへのインストール禁止？外部サービス全般禁止？）を先に確認
- スマホ使用可能なら、スマホ上でのClaude利用は別ルールである可能性
