---
sidebar_position: 1
title: "Messaging Gateway"
description: "Discord、Telegram、Slack などを Hermes Agent に接続する gateway の基本。"
---

# Messaging Gateway

詳細・全 platform の正本: https://hermes-agent.nousresearch.com/docs/user-guide/messaging

Messaging Gateway は、Discord、Telegram、Slack など複数 platform を一つの Hermes process に接続します。gateway は message routing、session、cron delivery、voice などを扱います。

## 導入順序

まず CLI で model と通常会話を成功させます。その後 gateway を追加します。

```bash
hermes
hermes doctor
hermes gateway setup
```

CLI 自体が動いていない状態で gateway の問題を同時に直そうとすると、provider と platform の障害を切り分けにくくなります。

## 基本コマンド

```bash
hermes gateway                    # foreground で実行
hermes gateway setup              # platform を対話設定
hermes gateway install            # user service を作成
hermes gateway start
hermes gateway stop
hermes gateway status
```

Linux の system service が必要な場合:

```bash
sudo hermes gateway install --system
hermes gateway status --system
```

## session

各 platform adapter は受信 message を session store に渡し、通常の Hermes agent loop を実行します。gateway 経由でも tools、memory、Skills、slash commands を利用できます。

共有 channel では platform ごとの session policy が重要です。Discord の既定動作は [Discord](./discord.md) を参照してください。

## cron と delivery

gateway は cron scheduler と outbound delivery にも使われます。定期タスクを platform へ送る場合は、先に対象 platform の credential と delivery target が正常であることを確認してください。

cron の詳細は [Scheduled Tasks](../features/cron.md) を参照してください。

## systemd watchdog

Linux の systemd service では、event loop 全体が停止した場合の process recovery を opt-in できます。

```yaml
gateway:
  systemd_watchdog_seconds: 120
```

設定後は service unit を再生成します。

```bash
hermes gateway install --force
```

## intentional silence

agent の最終応答が次の token **だけ** の場合、gateway は outbound message を送信しません。

- `[SILENT]`
- `SILENT`
- `NO_REPLY`
- `NO REPLY`

失敗自体を隠す仕組みではありません。error は通常どおり表面化します。

## 問題の切り分け

1. `hermes` で model が応答するか
2. `hermes doctor` に問題がないか
3. `hermes gateway status` で process が生きているか
4. platform credential / allowlist が正しいか
5. platform 固有の event / permission / intent を確認する

Discord は REST が成功していても Gateway WebSocket が壊れている場合があります。Discord 固有の liveness 設定は [Discord](./discord.md) を参照してください。
