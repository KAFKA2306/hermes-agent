---
sidebar_position: 3
title: "Discord"
description: "Hermes Agent を Discord bot として安全に接続するための基本設定。"
---

# Discord

詳細・全設定の正本: https://hermes-agent.nousresearch.com/docs/user-guide/messaging/discord

Discord adapter は webhook ではなく Hermes の Messaging Gateway 上で動きます。message は authorization、mention rule、session lookup を通った後、通常の agent loop に入ります。

## 既定の動作

| 場所 | 既定動作 |
| --- | --- |
| DM | 毎 message に応答。`@mention` 不要 |
| Server channel | bot が `@mention` されたときだけ応答 |
| Thread | thread 内で session を分離 |
| Shared channel | user ごとに session を分離 |

共有 channel の session 分離は次が既定です。

```yaml
group_sessions_per_user: true
```

`false` にすると channel 全体で context と実行 slot を共有します。複数 user の token cost、context growth、interrupt が混ざるため、共同 session が必要な場合だけ変更してください。

## Discord 側の準備

Discord Developer Portal で Application / Bot を作成し、少なくとも次を確認します。

- **Message Content Intent**: 必須
- **Server Members Intent**: 必須
- OAuth2 scopes: `bot`, `applications.commands`
- bot が対象 channel を閲覧・送信できる permission

bot が online なのに message 本文を読めない場合は、まず Message Content Intent を確認します。

## Hermes 側の設定

推奨:

```bash
hermes gateway setup
```

Discord を選び、Bot Token と許可する Discord User ID を設定します。

手動設定の最小例:

```bash
# ~/.hermes/.env
DISCORD_BOT_TOKEN=your-bot-token
DISCORD_ALLOWED_USERS=284102345871466496
```

起動:

```bash
hermes gateway
```

credential は Git に commit しません。

## authorization は fail-closed

`DISCORD_ALLOWED_USERS`、`DISCORD_ALLOWED_ROLES`、許可 channel などの scope がなく、allow-all も明示されていない場合、gateway は user を拒否します。

全 user を許可する設定もありますが、private / trusted guild 以外では避けます。

```bash
DISCORD_ALLOW_ALL_USERS=true
```

全 platform 共通の `GATEWAY_ALLOW_ALL_USERS` より、必要なら Discord 固有設定を優先してください。

## mention / thread

代表的な既定値:

```bash
DISCORD_REQUIRE_MENTION=true
DISCORD_THREAD_REQUIRE_MENTION=false
DISCORD_IGNORE_NO_MENTION=true
DISCORD_AUTO_THREAD=true
DISCORD_ALLOW_BOTS=none
```

特定 channel だけ `@mention` 不要にする場合:

```bash
DISCORD_FREE_RESPONSE_CHANNELS=1234567890,9876543210
```

free-response channel は auto-thread を使わず inline reply になります。

複数 bot が同じ thread にいる場合は、意図しない全 bot 応答を避けるため `DISCORD_THREAD_REQUIRE_MENTION=true` を検討します。

## bot-to-bot は使わない

`DISCORD_ALLOW_BOTS=none` が安全な既定です。複数 Hermes bot を互いに自動返信させる topology は、reply mention により loop する可能性があるためサポート対象外です。

## attachment

既定の最大 download size は 32 MiB です。

```bash
DISCORD_MAX_ATTACHMENT_BYTES=33554432
```

`0` は上限なしですが memory cost もなくなるわけではありません。通常は有限の上限を維持します。

## WebSocket health

Discord REST と Gateway WebSocket は別です。REST API が HTTP 200 を返しても、event を受信できるとは限りません。

liveness の既定設定例:

```yaml
discord:
  websocket_liveness_interval_seconds: 15
  websocket_liveness_failure_threshold: 2
  websocket_heartbeat_ack_max_age_seconds: 60
  websocket_max_latency_seconds: 30
```

連続して unhealthy と判定されると adapter は retryable fatal event を出し、gateway reconnect watcher が新しい adapter を作ります。

## 最初の検証

1. `hermes` で通常会話を成功させる
2. `hermes gateway setup` で Discord を設定する
3. `hermes gateway` を起動する
4. DM で応答を確認する
5. server channel で `@mention` 応答を確認する
6. `group_sessions_per_user` が期待どおり分離するか複数 user で確認する
7. 長時間運用では WebSocket liveness と reconnect を確認する

Discord 固有の問題を model/provider 問題と混ぜないことが、安定運用の基本です。
