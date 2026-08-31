https://hermes-agent.nousresearch.com/

# Hermes Agent

Nous Research が開発する自己改善型 AI エージェント [Hermes Agent](https://github.com/NousResearch/hermes-agent) の日本語向け fork です。

この fork では、upstream の実装を正本として追従しながら、利用開始に必要なドキュメントを日本語で短く保守します。詳細な仕様・全機能・最新の provider 一覧は upstream の公式ドキュメントを参照してください。

- 公式サイト: https://hermes-agent.nousresearch.com/
- 公式ドキュメント: https://hermes-agent.nousresearch.com/docs/
- upstream: https://github.com/NousResearch/hermes-agent
- この fork: https://github.com/KAFKA2306/hermes-agent

## Hermes Agent でできること

Hermes Agent は、単一のチャット API を包むだけの CLI ではありません。会話・ツール利用・記憶・Skills・自動化・外部メッセージングを一つのエージェント実行環境として扱います。

主な機能は次のとおりです。

- CLI / TUI から対話する
- OpenAI、Anthropic、OpenRouter、Nous Portal、ローカルの OpenAI-compatible endpoint などからモデルを選ぶ
- terminal、file、web、browser などの tools を使う
- 会話をまたいで memory を保持する
- 経験から Skills を作成・再利用する
- cron で定期タスクを実行する
- Telegram、Discord、Slack などへ gateway で接続する
- subagent を分離して並列処理する
- Docker、SSH、Modal、Daytona などの実行環境を使う

機能の詳細は公式ドキュメントを正本とします。

## インストール

### Linux / macOS / WSL2 / Android (Termux)

```bash
curl -fsSL https://hermes-agent.nousresearch.com/install.sh | bash
```

### Windows (PowerShell)

```powershell
iex (irm https://hermes-agent.nousresearch.com/install.ps1)
```

インストーラは Python、uv、Node.js、ripgrep、ffmpeg など必要な依存関係を管理します。個別に手作業で環境を組むより、通常は公式インストーラを使ってください。

## 最短セットアップ

まず CLI で一つの会話が正常に完了するところまで確認します。gateway、cron、Skills などはその後に追加します。

```bash
hermes setup       # 初期設定
hermes model       # provider / model を選択
hermes             # 対話開始
hermes doctor      # 問題の診断
```

Nous Portal を使う場合は、OAuth と Tool Gateway をまとめて設定できます。

```bash
hermes setup --portal
```

モデルや provider を切り替えるだけならコード変更は不要です。

## よく使うコマンド

| コマンド | 用途 |
| --- | --- |
| `hermes` | CLI を開始 |
| `hermes --tui` | TUI を開始 |
| `hermes model` | provider / model を選択 |
| `hermes tools` | tools を設定 |
| `hermes gateway setup` | Telegram / Discord などを設定 |
| `hermes config get` | 設定値を確認 |
| `hermes config set` | 設定値を変更 |
| `hermes doctor` | 環境・設定を診断 |
| `hermes update` | インストール方法に応じて更新 |

## 日本語ドキュメント

Docusaurus の英語本文は `website/docs/` が正本です。日本語は Docusaurus 標準の locale 構造で、正本と同じ doc ID を翻訳します。

- [日本語ドキュメント入口](website/i18n/ja/docusaurus-plugin-content-docs/current/index.mdx)
- [日本語 Quickstart](website/i18n/ja/docusaurus-plugin-content-docs/current/getting-started/quickstart.md)
- [日本語 Installation](website/i18n/ja/docusaurus-plugin-content-docs/current/getting-started/installation.md)
- [英語の全ドキュメント](website/docs/)

翻訳されていない詳細ページは upstream の英語文書を参照してください。内部設計・契約・RCA はリポジトリ直下の `docs/` にあり、利用者向け Docusaurus 文書とは役割を分けています。

## ドキュメントの保守方針

ドキュメントは実装と同じ情報を重複して持たないようにします。

- CLI 名、設定名、パス、provider 名は現在の実装を正本とする
- README は導入とナビゲーションに限定する
- 詳細仕様は `website/docs/` またはコードに寄せる
- 日本語版は直訳で肥大化させず、利用手順を短く保つ
- locale を追加・変更した場合は Docusaurus の全 locale build を CI で検証する
- 古い手順を互換目的なしに残さない

## ドキュメントをローカルで確認する

```bash
cd website
npm ci
npm start
```

全 locale を含めて build する場合:

```bash
npm run build
```

図表 lint と TypeScript の確認:

```bash
npm run lint:diagrams
npm run typecheck
```

## 開発

通常のインストール後、管理されている checkout で開発できます。

```bash
cd "${HERMES_HOME:-$HOME/.hermes}/hermes-agent"
uv pip install -e ".[all,dev]"
scripts/run_tests.sh
```

開発手順の詳細は upstream の [Contributing Guide](https://hermes-agent.nousresearch.com/docs/developer-guide/contributing) を参照してください。

## upstream との関係

このリポジトリは `NousResearch/hermes-agent` の fork です。Hermes Agent 本体の仕様・リリース・公式 production は upstream が正本です。この fork 固有の日本語化を upstream の実装変更と混同しないよう、機能説明は可能な限り公式ドキュメントへリンクします。

## License

MIT License。詳細は [LICENSE](LICENSE) を参照してください。

Hermes Agent は [Nous Research](https://nousresearch.com) により開発されています。
