# Hermes Agent Documentation

https://hermes-agent.nousresearch.com/docs/

`website/` は Hermes Agent の利用者向け Docusaurus ドキュメントです。

## 正本

- 利用者向け本文: `website/docs/`
- 翻訳: `website/i18n/<locale>/docusaurus-plugin-content-docs/current/`
- Docusaurus 設定: `website/docusaurus.config.ts`
- navigation: `website/sidebars.ts`
- 内部設計・契約・RCA: リポジトリ直下の `docs/`

`website/docs/` とリポジトリ直下の `docs/` は役割が異なります。利用手順を内部仕様側へ重複して書かないでください。

## セットアップ

このリポジトリは `package-lock.json` を正本として npm を使います。

```bash
cd website
npm ci
```

## ローカル開発

英語:

```bash
npm start
```

日本語:

```bash
npm start -- --locale ja
```

## 検証

全 locale を build します。

```bash
npm run build
```

追加の検証:

```bash
npm run lint:diagrams
npm run typecheck
```

`npm run build:fast` は英語だけを対象にする高速確認用です。locale の追加・翻訳変更を検証するときは使わず、`npm run build` を実行してください。

## 日本語化

日本語 locale は `ja` です。英語の doc ID と同じ相対パスに翻訳を置きます。

例:

```text
website/docs/getting-started/quickstart.md
website/i18n/ja/docusaurus-plugin-content-docs/current/getting-started/quickstart.md
```

翻訳は英語本文の逐語訳ではなく、CLI 名・設定名・コマンド・パスを保持しながら、重複説明を削減して読みやすくします。詳細仕様を日本語側へ複製せず、必要に応じて正本へリンクします。

## 図表

CI は `ascii-guard` で ASCII box diagram を検出します。構造図には Mermaid、単純な比較には Markdown の table / list を使ってください。

## 公開

公式 production:

https://hermes-agent.nousresearch.com/docs/

公式 production の deployment は upstream `NousResearch/hermes-agent` の workflow が管理します。この fork の build 成功を production 反映とみなしません。
