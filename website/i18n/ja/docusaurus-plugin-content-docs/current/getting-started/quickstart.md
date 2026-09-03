---
sidebar_position: 1
title: "Hermes Agent Quickstart"
description: "インストールから最初の会話、診断までを最短経路で確認する。"
---

# Hermes Agent Quickstart

このページでは、Hermes Agent を「インストールできた」ではなく「実際に一つの会話を完了できた」状態まで進めます。

## 1. インストール

### Linux / macOS / WSL2 / Android (Termux)

```bash
curl -fsSL https://hermes-agent.nousresearch.com/install.sh | bash
```

### Windows (PowerShell)

```powershell
iex (irm https://hermes-agent.nousresearch.com/install.ps1)
```

詳細は [Installation](./installation.md) を参照してください。

## 2. provider / model を一つ選ぶ

```bash
hermes model
```

最初は routing や fallback を組まず、一つの provider / model だけを正常動作させます。

Nous Portal を使う場合は次の設定が最短です。

```bash
hermes setup --portal
```

API key を個別に設定する場合や、OpenAI、Anthropic、OpenRouter、Hugging Face、AWS Bedrock、Google AI Studio、ローカル endpoint などを使う場合は [Providers](../integrations/providers.md) を参照してください。

### ローカルモデル

OpenAI-compatible endpoint を使えます。model 名、base URL、context length が実サーバー側と一致していることを確認してください。

Hermes は長い tool-calling workflow を前提としているため、公式 Quickstart では **64K tokens 以上の context** を要求しています。小さい context の local model を無理に接続するより、推論サーバー側の context 設定を先に確認してください。

## 3. 最初の会話を成功させる

CLI:

```bash
hermes
```

TUI:

```bash
hermes --tui
```

最初の prompt は、結果を確認しやすいものにします。

```text
このリポジトリを5項目で要約し、主要な entrypoint を示してください。
```

正常な状態では、少なくとも次を確認できます。

- 選択した provider / model が表示される
- provider error なしで応答が返る
- 必要なら terminal / file などの tool を使える
- 2 turn 以上、会話を継続できる

ここが失敗している間は gateway、cron、Skills、voice、routing を追加しません。

## 4. 診断する

```bash
hermes doctor
```

設定値を確認する場合:

```bash
hermes config get
```

model を変更する場合:

```bash
hermes model
```

問題を直すときは、install → provider → model → tools → gateway の順に層を分けて確認します。

## 5. session 再開を確認する

```bash
hermes --continue
# または
hermes -c
```

直前の session に戻れることを確認します。複数 profile / machine を使う場合は、どの profile に保存された session かも確認してください。

## 6. 必要な機能だけ追加する

### tools

```bash
hermes tools
```

platform ごとの tool access を調整します。

### Telegram / Discord / Slack など

```bash
hermes gateway setup
```

CLI が正常に動いた後で messaging platform を接続します。platform ごとの手順は [Messaging Gateway](../user-guide/messaging/) を参照してください。

### sandbox

Docker や remote host に terminal execution を分離できます。

```bash
hermes config set terminal.backend docker
# または
hermes config set terminal.backend ssh
```

### Skills / cron

CLI または gateway の基礎動作を確認した後で追加します。

```bash
hermes skills
```

cron の詳細は [Cron](../user-guide/features/cron.md) を参照してください。

## 7. 次に読む

- [Configuration](../user-guide/configuration.md): config 全体
- [CLI](../user-guide/cli.md): CLI / TUI
- [Tools](../user-guide/features/tools.md): toolset と権限
- [Memory](../user-guide/features/memory.md): 永続 memory
- [Skills](../user-guide/features/skills.md): Skills の作成・利用
- [Security](../user-guide/security.md): command approval と isolation
- [FAQ](../reference/faq.md): troubleshooting

## 基本原則

最短で安定させるには、機能を一度に増やさず、各段階で実際の成功条件を確認します。

1. `hermes` が起動する
2. model が応答する
3. tool が必要な範囲で動く
4. session が保存・再開できる
5. その後に gateway / cron / Skills を追加する

この順序なら、障害が起きても原因を一つの層に絞れます。
