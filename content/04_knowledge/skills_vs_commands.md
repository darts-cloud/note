# skills と commands の違い

## skills

- AI エージェントや拡張が持つ「高レベルな能力セット」
- 特定の目的・ドメインに特化した振る舞い
- 例: コード生成、Azure デプロイ、Cosmos DB ベストプラクティス
- 通常は「何をするか」にフォーカス

## commands

- エージェントが実際に実行する「具体的な命令」
- ファイル作成、コマンド実行、API 呼び出しなどの処理
- 例: ファイルを作る、ターミナルを実行する、Azure リソースを作成する
- 通常は「どうやって実行するか」にフォーカス

## 違いのポイント

- skills は「能力の定義」
- commands は「能力を発揮するための具体的手段」
- skills が高レベルで役割を持ち、commands はその役割を実現する実装レベル

## 実行のトリガー

- skills のトリガー
  - ユーザーの要求やタスクの意図に応じて、エージェントが適切な能力を選択
  - プロンプトや会話文の内容に基づき、どの skill を使うか決定
  - 例: 「Azure にリソースを作りたい」「コードを修正してほしい」
- commands のトリガー
  - skill が選択された後、その skill の内部で具体的に呼び出される
  - 実際の処理を実行するためのステップとして起動
  - 例: 「ファイルを作成する」「CLI を実行する」「API を呼び出す」

## skills と commands の関係

- skill は「いつ・どの能力を使うか」を決めるエントリポイント
- command は「その能力をどう実行するか」を処理する実行単位
- skill の呼び出しがトリガーとなり、必要な command が動く場合が多い

## Claude Code / Gemini CLI / Codex の違い

| 項目 | Claude Code | Gemini CLI | Codex |
|---|---|---|---|
| 提供元 | Anthropic | Google | OpenAI |
| 主用途 | コード生成・修正・リファクタリング | CLI からモデルを実行 | プログラミング支援・補完 |
| 利用形態 | エディタや API 経由 | ターミナルのコマンド | API、エディタプラグイン |
| 強み | コードの意味理解と大規模コンテキスト | 端末から直接使える手軽さ | プログラミング補助に最適化 |
| 代表的な利用シーン | コード修正、リファクタ、設計支援 | モデル呼び出し、スクリプト実行 | 補完、生成、デバッグ支援 |

## まとめ

- **skills**: 「何をできるか」を定義する能力・役割
- **commands**: 「どうやって実行するか」を指示する具体的な命令
- **Claude Code**: 開発者向けコード生成・編集支援
- **Gemini CLI**: Gemini モデルを CLI から使うためのツール
- **Codex**: OpenAI のプログラミング向け言語モデル

> 簡単に言うと、`skills` は能力の名前や役割、`commands` はその能力を実際に動かすための命令です。

## Skills 定義ファイルの場所

- このドキュメントがあるプロジェクト自体には `SKILL.md` や `Skills.md` の定義ファイルは含まれていません。
- VS Code の拡張機能フォルダ内に、ツールごとの `SKILL.md` や類似の指示書が置かれています。
- プロジェクトフォルダとしてイメージするなら、以下の「拡張機能ルート」配下が対象です。
  - `<VS Code extensions folder>/<extension-id>-<version>/...`
  - 例: `<extensions-root>/github.copilot-chat-0.45.1/assets/prompts/skills/...`

### ツール別の一般的な場所

- ここでの説明は「一般的にどこにスキル定義が置かれるか」の傾向です。
- 実際の保存場所はツールやバージョンによって変わり、必ずしもプロジェクト直下ではありません。

#### GitHub Copilot Chat
- `github.copilot-chat` は VS Code 拡張内にスキル定義を持つことが多いです。
- 一般的な構成:
  - `assets/prompts/skills/` 配下
- 代表的なファイル例:
  - `assets/prompts/skills/get-search-view-results/SKILL.md`
  - `assets/prompts/skills/agent-customization/SKILL.md`
  - `assets/prompts/skills/agent-customization/references/skills.md`

#### Claude Code
- `anthropic.claude-code` は拡張内でプロンプトやUI定義を管理するケースが多いです。
- 一般的な構成:
  - `resources/`
  - `webview/`
- そのため、`SKILL.md` というファイル名ではなく、拡張内アセットとして定義されることがあります。

#### Cursor
- Cursor 系ツールでは、ワークスペース直下の隠しフォルダに定義を置くケースが一般的です。
- 例:
  - `.cursor/skills/`
- この場合、プロジェクトルート以下に `/.cursor/skills/` が存在し、プロジェクト固有のスキル設定を管理します。
- ただし、Cursor の実装によっては拡張内に配置される場合もあるため、ツールのドキュメントを確認してください。

#### Antigravity
- Antigravity 系ツールでは、プロジェクト直下の隠しフォルダにスキル定義を置くのが一般的です。
- 例:
  - `.antigravity/skills/`
- VS Code 拡張として扱う場合もありますが、基本はプロジェクト内の隠しフォルダに配置されることが多いです。

## 補足

- `SKILL.md` / 指示書ファイルはツールや拡張の動作定義に使われます。
- GitHub Copilot Chat などは拡張機能内で管理される一方、Cursor のようにプロジェクト直下の隠しフォルダを使うツールもあります。
- そのため、「このプロジェクトの場所」ではなく、「ツールごとの一般的な配置」を押さえることが重要です。
