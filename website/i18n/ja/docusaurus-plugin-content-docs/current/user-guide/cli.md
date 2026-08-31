---
sidebar_position: 1
title: "CLI / TUI"
description: "Hermes Agent の CLI、TUI、session、worktree の主要操作。"
---

# CLI / TUI

詳細・全オプションの正本: https://hermes-agent.nousresearch.com/docs/user-guide/cli

Hermes Agent は terminal から対話できます。通常は `hermes`、TUI は `hermes --tui` を使います。

## 基本

```bash
hermes                       # 対話 session
hermes --tui                 # TUI
hermes chat -q "Hello"       # 1回だけ問い合わせ
hermes chat --query-file prompt.txt
hermes chat --query-file - < prompt.txt
```

model / provider / toolset を明示する場合:

```bash
hermes chat --model "anthropic/claude-sonnet-4"
hermes chat --provider nous
hermes chat --provider openrouter
hermes chat --toolsets "web,terminal,skills"
```

## session の再開

```bash
hermes --continue
hermes --resume <session_id>
hermes --resume latest
hermes --resume latest --in ./dir
```

`--continue` は直近の CLI session を再開します。複数 project を扱う場合は、どの directory の session を再開するかを明示した方が安全です。

## Skills を読み込む

```bash
hermes -s hermes-agent-dev,github-auth
hermes chat -s github-pr-workflow -q "open a draft PR"
```

## git worktree で分離する

複数 agent を同じ repository で並行実行するときは worktree を使えます。

```bash
hermes -w
hermes -w -z "Fix issue #123"
```

状態確認と安全な整理:

```bash
hermes worktree list
hermes worktree prune --dry-run
hermes worktree prune
```

pruner は未commitの tracked change、unique unpushed commit、実行中 session を保護します。削除前に `--dry-run` で対象を確認できます。

## plugin

```bash
hermes plugins install owner/repository --no-enable
hermes plugins list
hermes plugins enable <plugin-name>
hermes plugins disable <plugin-name>
hermes plugins update <plugin-name>
hermes plugins remove <plugin-name>
```

portable plugin は install しただけでは有効化されません。

## context の確認

CLI の status bar では model、context 使用量、cost、session 時間、context compression 回数、background task 数などを確認できます。

長い session では context 使用量を見ながら進めます。model を途中で切り替えると prompt cache が無効になり、次の turn で全 context を再送する場合があります。

## 危険な操作

`--yolo` または `/yolo` は approval prompt を迂回します。通常運用では有効にせず、必要なら [Security](./security.md) を確認してください。

次に model を設定する場合は [Configuring Models](./configuring-models.md) を参照してください。
