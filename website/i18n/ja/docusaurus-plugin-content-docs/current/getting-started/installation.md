---
sidebar_position: 2
title: "Installation"
description: "Linux、macOS、WSL2、Windows、Android (Termux) に Hermes Agent を導入する。"
---

# Installation

通常は公式インストーラを使います。Python や Node.js の環境を先に手作業で組む必要はありません。

## 推奨: Hermes Desktop

Windows / macOS では、CLI と Desktop をまとめて使う場合は公式サイトから Hermes Desktop installer を利用できます。

https://hermes-agent.nousresearch.com/

## CLI のインストール

### Linux / macOS / WSL2 / Android (Termux)

```bash
curl -fsSL https://hermes-agent.nousresearch.com/install.sh | bash
```

### Windows (PowerShell)

```powershell
iex (irm https://hermes-agent.nousresearch.com/install.ps1)
```

CLI 導入後に Desktop を追加する場合:

```bash
hermes desktop
```

## インストーラが管理するもの

通常の install path では、インストーラが次を管理します。

- repository checkout
- Python 3.11
- uv
- Node.js
- ripgrep
- ffmpeg
- virtual environment
- `hermes` launcher
- 初期 provider 設定への導線

OS ごとの追加依存や browser automation の詳細は英語の [Installation](https://hermes-agent.nousresearch.com/docs/getting-started/installation) を正本とします。

## 配置

代表的な per-user install は次の配置です。

| 種類 | パス |
| --- | --- |
| code | `~/.hermes/hermes-agent/` |
| launcher | `~/.local/bin/hermes` |
| data / config | `~/.hermes/` |

root install や service user では配置が異なります。system-wide install が必要な場合は英語の Installation guide を確認してください。

## インストール後

shell を再読込して起動します。

```bash
source ~/.bashrc
# zsh の場合
source ~/.zshrc

hermes
```

初期設定と診断:

```bash
hermes setup
hermes model
hermes tools
hermes doctor
```

messaging platform を使う場合:

```bash
hermes gateway setup
```

Nous Portal を使う場合:

```bash
hermes setup --portal
```

## 最小 prerequisites

Linux / macOS 系の公式 install path では Git が必要です。Linux では `curl` と `xz-utils` も確認してください。

Debian / Ubuntu の例:

```bash
sudo apt install git curl xz-utils
```

Desktop の native module build が必要な場合は `build-essential` も必要です。

```bash
sudo apt install build-essential
```

Python、uv、Node.js、ripgrep、ffmpeg は通常インストーラに任せます。

## service user / sudo なしで使う場合

Hermes 本体は unprivileged user でも導入できます。ただし Playwright / Chromium の system libraries は管理者権限が必要になる場合があります。

Debian / Ubuntu では、管理者が一度だけ dependency を用意し、その後 service user が通常インストーラを実行する構成に分けられます。

```bash
sudo npx playwright install-deps chromium
```

browser automation が不要なら install 時に skip できます。

```bash
curl -fsSL https://hermes-agent.nousresearch.com/install.sh | bash -s -- --skip-browser
```

systemd の user service を logout 後も動かす場合は、service user の linger 設定が必要です。

```bash
sudo loginctl enable-linger <service-user>
```

## 確認

```bash
hermes doctor
```

`hermes` が見つからない場合は、まず shell の再読込と `~/.local/bin` の PATH を確認します。

provider / API key の問題は、直接ファイルを編集する前に次で設定状態を確認してください。

```bash
hermes model
hermes config get
```

## 更新

```bash
hermes update
```

Hermes は install layout から更新方法を判定します。更新後に問題がある場合は `hermes doctor` で現在の状態を再確認してください。

次は [Quickstart](./quickstart.md) で、最初の provider / model と実会話を検証します。
