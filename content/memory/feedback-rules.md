---
title: フィードバック・運用ルール
tags: [memory, feedback]
---

# フィードバック・運用ルール

## ノート保存先とGit運用

`~/note/content` 以下を主記憶として使用する。

- このディレクトリはObsidianのVaultであり、かつGitHubリポジトリでもある
- 新しい記憶は `~/note/content/memory/` 以下にmdファイルとして保存する
- Git操作は `gh` CLI（または `git`）を使用してpull → commit → push
- コンフリクトが発生した場合は自分自身（Claude側の内容）を正として解決する（`git checkout --ours` 相当）
- ディレクトリ名・ファイル名はアルファベットにする
- タグ名に「claude」を含めない

## nanoclaw アクセス禁止

`/home/naoki/nanoclaw` ディレクトリのファイルを読んだり参照したりしてはならない。

- nanoclaw に関するタスクでも、このディレクトリ配下のファイルを Read/Grep/Glob/Bash で参照しない
- 必要な情報はユーザーに直接質問する
