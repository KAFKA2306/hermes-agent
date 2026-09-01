https://hermes-agent.nousresearch.com/docs/

# Hermes Agent

Nous Research が開発する自己改善型 AI エージェント [Hermes Agent](https://github.com/NousResearch/hermes-agent) の日本語向け fork です。

この fork では、upstream の実装を正本として追従しながら、利用開始に必要なドキュメントを日本語で短く保守します。詳細な仕様・全機能・最新の provider 一覧は upstream の公式ドキュメントを参照してください。

- 公式サイト: https://hermes-agent.nousresearch.com/
- 公式ドキュメント: https://hermes-agent.nousresearch.com/docs/
- upstream: https://github.com/NousResearch/hermes-agent

## 最短で使う

### Windows / macOS

Hermes Desktop:

https://hermes-agent.nousresearch.com/

### Linux / macOS / WSL2 / Android (Termux)

```bash
curl -fsSL https://hermes-agent.nousresearch.com/install.sh | bash
```

### Windows (PowerShell)

```powershell
iex (irm https://hermes-agent.nousresearch.com/install.ps1)
```

インストール後は provider / model を設定します。

```bash
hermes model
```

Nous Portal を使う場合は次が最短です。

```bash
hermes setup --portal
```

起動:

```bash
hermes
# または
hermes --tui
```

## まず確認すること

1. `hermes` が起動する
2. 選択した model/provider が表示される
3. 通常の会話が1往復以上成功する
4. `hermes doctor` で重大な設定エラーがない
5. その後に Discord、Telegram、cron、skills、MCP などを追加する

基本会話が成立する前に機能を積み重ねない方が、障害点を切り分けやすくなります。

## 主な機能

- CLI / TUI / Desktop
- 複数の LLM provider と custom OpenAI-compatible endpoint
- terminal / file / web / browser などの tools
- persistent memory と skills
- subagent / delegation
- cron / automation
- Discord、Telegram、Slack などの Messaging Gateway
- MCP
- Docker / SSH / Daytona / Modal などの terminal backend

## ドキュメント

利用者向けドキュメントの正本は `website/docs/`、日本語翻訳は `website/i18n/ja/` です。リポジトリ直下の `docs/` は内部設計・契約・RCA などを扱います。

日本語の主要導線:

- `website/i18n/ja/docusaurus-plugin-content-docs/current/index.mdx`
- `website/i18n/ja/docusaurus-plugin-content-docs/current/getting-started/installation.md`
- `website/i18n/ja/docusaurus-plugin-content-docs/current/getting-started/quickstart.md`
- `website/i18n/ja/docusaurus-plugin-content-docs/current/user-guide/cli.md`
- `website/i18n/ja/docusaurus-plugin-content-docs/current/user-guide/configuring-models.md`
- `website/i18n/ja/docusaurus-plugin-content-docs/current/user-guide/messaging/`

詳細・未翻訳ページは公式英語ドキュメントを参照してください。

## ドキュメントをローカル確認する

```bash
cd website
npm ci
npm start -- --locale ja
```

全 locale の build:

```bash
npm run build
```

補助検証:

```bash
npm run lint:diagrams
npm run typecheck
```

## upstream 追従

この repository は `NousResearch/hermes-agent` の fork です。機能仕様、CLI、設定 schema、provider 情報は upstream の現行実装を正本とし、日本語側へ独自仕様を追加しません。

翻訳は、英語本文を丸ごと複製するより、利用開始と運用に必要な要点を日本語で整理し、詳細は正本へリンクする方針です。これにより upstream 更新時の翻訳 drift を抑えます。

## Deployment

公式 production は upstream が管理します。

https://hermes-agent.nousresearch.com/docs/

この fork の CI 成功だけを公式 production 反映とはみなしません。
