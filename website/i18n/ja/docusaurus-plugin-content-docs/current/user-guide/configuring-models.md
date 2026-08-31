---
sidebar_position: 3
title: "Model の設定"
description: "Main model と auxiliary model の役割、切り替え、設定の基本。"
---

# Model の設定

詳細・全設定の正本: https://hermes-agent.nousresearch.com/docs/user-guide/configuring-models

Hermes には大きく2種類の model slot があります。

- **Main model**: 通常会話、tool-call loop、最終応答を処理する
- **Auxiliary model**: context compression、vision、web extraction、approval scoring、MCP routing、title generation、skill search などの補助処理を担当する

## 最初の設定

```bash
hermes model
```

Nous Portal を使う場合:

```bash
hermes setup --portal
hermes portal info
```

初期状態で `config.yaml` の `model` が空文字列でも異常ではありません。`hermes setup` または `hermes model` を実行すると、provider / model を持つ構造へ更新されます。

## Main model

代表的な設定例:

```yaml
model:
  provider: openrouter
  default: anthropic/claude-opus-4.7
  base_url: ''
  api_mode: chat_completions
```

Dashboard または `hermes model` で変更できます。Dashboard からの変更は原則として新しい session に適用されます。実行中 session は `/model` で切り替えます。

## Auxiliary model

補助処理は既定で `auto` です。Main model を使い、必要に応じて fallback policy をたどります。

安価・高速な model に分けやすい処理は次のとおりです。

- title generation
- context compression
- web extraction
- approval scoring
- session title
- skill search

vision は Main model が画像入力に対応しない場合に別 model を指定できます。

例:

```yaml
auxiliary:
  compression:
    provider: auto
    model: ''
```

## model を途中で切り替えるとき

長い session の途中で model を切り替えると、次の turn で context compression が必要になる場合があります。また prompt cache は model ごとに分かれるため、切り替え後の最初の request は conversation 全体を再送し、入力 token cost が増えることがあります。

頻繁に model を変えるより、必要なら新しい session を始める方が単純です。

## local / custom endpoint

OpenAI-compatible endpoint は custom provider として使えます。`base_url`、model 名、実際の context length を推論 server と一致させてください。

```bash
hermes model
```

local model の詳細は [FAQ](../reference/faq.md) を参照してください。

## unattended workload

cron や worker など人が approval できない実行では、model の data policy と cost が明示的である必要があります。data-training tier を許可する設定は opt-in です。

```bash
hermes config set security.allow_data_training_tiers_noninteractive true
```

必要性を確認した上で設定してください。
