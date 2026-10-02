---
title: Testnet Faucet
excerpt: Claim testnet USDC or USDT on supported blockchains for development and testing.
x-content:
  excerpt: Claim testnet USDC or USDT on supported blockchains for development and testing.
hidden: false
metadata:
  robots: noindex
x-privacy:
  view: public
---
**How it works:**

1. Call the faucet endpoint with a target chain, currency, wallet address, and amount.
2. USDC claims are routed through Circle's sandbox infrastructure; USDT claims are sent directly from OwlPay's own testnet wallet (Circle's sandbox does not support USDT).
3. Testnet funds arrive in your wallet (typically within seconds).

**Key constraints:**

* Available in **sandbox environments only**
* Each application has a daily quota of **500 per chain, per currency**, resetting at midnight UTC.
* Each chain/currency combination's quota is tracked independently (e.g. USDC on Ethereum and USDT on Ethereum have separate quotas).
* **USDT is only available on Ethereum.** USDC is available on all chains listed below.

***

### Supported Chains

| Chain     | `chain` value | Address Format              | Example                                                    | Currencies    |
| --------- | ------------- | ---------------------------- | ---------------------------------------------------------- | ------------- |
| Ethereum  | `ethereum`    | `0x` + 40 hex chars          | `0x1234567890abcdef1234567890abcdef12345678`               | USDC, USDT    |
| Polygon   | `polygon`     | `0x` + 40 hex chars          | `0x1234567890abcdef1234567890abcdef12345678`               | USDC          |
| Arbitrum  | `arbitrum`    | `0x` + 40 hex chars          | `0x1234567890abcdef1234567890abcdef12345678`               | USDC          |
| Avalanche | `avalanche`   | `0x` + 40 hex chars          | `0x1234567890abcdef1234567890abcdef12345678`               | USDC          |
| Optimism  | `optimism`    | `0x` + 40 hex chars          | `0x1234567890abcdef1234567890abcdef12345678`               | USDC          |
| Base      | `base`        | `0x` + 40 hex chars          | `0x1234567890abcdef1234567890abcdef12345678`               | USDC          |
| Stellar   | `stellar`     | `G` + 55 alphanumeric chars  | `GBXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX` | USDC          |
| Solana    | `solana`      | 32-44 base58 chars           | `7EcDhSYGxXyscszYEp35KHN8vvw3svAuLKTzXwCFLtV`              | USDC          |

***

### API Reference

#### Claim Testnet Funds

```
POST /api/v1/applications/faucet
```

**Headers**

| Header         | Required | Description              |
| -------------- | -------- | ------------------------ |
| `X-API-KEY`    | Yes      | Your application API key |
| `Content-Type` | Yes      | `application/json`       |
| `Accept`       | Yes      | `application/json`       |

**Request Body**

| Parameter     | Type   | Required | Description                                                                                                          |
| ------------- | ------ | -------- | -------------------------------------------------------------------------------------------------------------------- |
| `currency`    | string | Yes      | Asset to claim. One of: `USDC`, `USDT`                                                                              |
| `chain`       | string | Yes      | Target blockchain. `ethereum`, `polygon`, `arbitrum`, `avalanche`, `optimism`, `base`, `stellar`, `solana` for USDC — `ethereum` only for USDT |
| `address`     | string | Yes      | Recipient wallet address (10-256 characters)                                                                         |
| `address_tag` | string | No       | Address tag / memo for chains that require it (e.g. Stellar memo). Max 100 characters. Omit for chains without tags. |
| `amount`      | string | Yes      | Amount to claim. Min: `0.01`, Max: `500.00`. Up to 2 decimal places.                                                 |

**Success Response** `200 OK`

```json
{
    "chain": "ethereum",
    "currency": "USDC",
    "address": "0x1234567890abcdef1234567890abcdef12345678",
    "amount": "100.00",
    "daily_used": "100.00",
    "daily_remaining": "400.00",
    "daily_limit": "500.00",
    "reference_id": "b8627ae8-732b-4d25-b947-1df8f4007a29"
}
```

| Field              | Type   | Description                                                              |
| ------------------ | ------ | ------------------------------------------------------------------------ |
| `chain`            | string | The blockchain chain name                                                |
| `currency`         | string | The claimed asset (`USDC` or `USDT`)                                     |
| `address`          | string | The recipient wallet address                                             |
| `amount`           | string | Amount claimed in this request                                           |
| `daily_used`       | string | Total amount claimed today for this chain/currency                       |
| `daily_remaining`  | string | Remaining daily quota for this chain/currency                            |
| `daily_limit`      | string | Daily limit per chain/currency (always `500.00`)                        |
| `reference_id`     | string | Tracking ID for this payout — a Circle payout ID for USDC, or the on-chain transaction hash for USDT |

***

### Examples

#### Claim USDC

```bash
curl -X POST 'https://harbor-sandbox.owlpay.com/api/v1/applications/faucet' \
  -H 'Content-Type: application/json' \
  -H 'Accept: application/json' \
  -H 'X-API-KEY: YOUR_API_KEY' \
  -d '{
    "currency": "USDC",
    "chain": "ethereum",
    "address": "0x1234567890abcdef1234567890abcdef12345678",
    "amount": "100.00"
  }'
```

#### Claim USDT (Ethereum only)

```bash
curl -X POST 'https://harbor-sandbox.owlpay.com/api/v1/applications/faucet' \
  -H 'Content-Type: application/json' \
  -H 'Accept: application/json' \
  -H 'X-API-KEY: YOUR_API_KEY' \
  -d '{
    "currency": "USDT",
    "chain": "ethereum",
    "address": "0x1234567890abcdef1234567890abcdef12345678",
    "amount": "100.00"
  }'
```

#### Claim on Base (USDC)

```bash
curl -X POST 'https://harbor-sandbox.owlpay.com/api/v1/applications/faucet' \
  -H 'Content-Type: application/json' \
  -H 'Accept: application/json' \
  -H 'X-API-KEY: YOUR_API_KEY' \
  -d '{
    "currency": "USDC",
    "chain": "base",
    "address": "0x1234567890abcdef1234567890abcdef12345678",
    "amount": "100.00"
  }'
```

#### Claim on Multiple Chains/Currencies

Each chain/currency combination has its own independent 500 daily quota, so you can claim across chains and currencies freely.

```bash
# 200 USDC on Ethereum
curl -X POST 'https://harbor-sandbox.owlpay.com/api/v1/applications/faucet' \
  -H 'Content-Type: application/json' \
  -H 'Accept: application/json' \
  -H 'X-API-KEY: YOUR_API_KEY' \
  -d '{"currency": "USDC", "chain": "ethereum", "address": "0xabc...def", "amount": "200.00"}'

# 100 USDT on Ethereum (separate quota from USDC above)
curl -X POST 'https://harbor-sandbox.owlpay.com/api/v1/applications/faucet' \
  -H 'Content-Type: application/json' \
  -H 'Accept: application/json' \
  -H 'X-API-KEY: YOUR_API_KEY' \
  -d '{"currency": "USDT", "chain": "ethereum", "address": "0xabc...def", "amount": "100.00"}'

# 300 USDC on Polygon (separate quota)
curl -X POST 'https://harbor-sandbox.owlpay.com/api/v1/applications/faucet' \
  -H 'Content-Type: application/json' \
  -H 'Accept: application/json' \
  -H 'X-API-KEY: YOUR_API_KEY' \
  -d '{"currency": "USDC", "chain": "polygon", "address": "0xabc...def", "amount": "300.00"}'
```

#### Check Remaining Quota

There is no separate quota endpoint. Instead, every successful claim response includes `daily_used`, `daily_remaining`, and `daily_limit`, so you can track your usage after each request.

#### Solana Example

```bash
curl -X POST 'https://harbor-sandbox.owlpay.com/api/v1/applications/faucet' \
  -H 'Content-Type: application/json' \
  -H 'Accept: application/json' \
  -H 'X-API-KEY: YOUR_API_KEY' \
  -d '{
    "currency": "USDC",
    "chain": "solana",
    "address": "7EcDhSYGxXyscszYEp35KHN8vvw3svAuLKTzXwCFLtV",
    "amount": "50.00"
  }'
```

#### Stellar Example (with memo)

Use `address_tag` to pass the Stellar memo. Omit it if your wallet does not require a memo.

```bash
curl -X POST 'https://harbor-sandbox.owlpay.com/api/v1/applications/faucet' \
  -H 'Content-Type: application/json' \
  -H 'Accept: application/json' \
  -H 'X-API-KEY: YOUR_API_KEY' \
  -d '{
    "currency": "USDC",
    "chain": "stellar",
    "address": "GBXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX",
    "address_tag": "123456",
    "amount": "50.00"
  }'
```

***

#### Error Code Reference

| Code | Meaning                                | Action                                               |
| ---- | --------------------------------------- | ----------------------------------------------------- |
| 7001 | Daily limit exceeded                    | Reduce amount or wait for quota reset (midnight UTC)  |
| 7002 | Circle recipient registration failed    | USDC only. Retry the request; contact support if persistent |
| 7003 | Circle payout creation failed           | USDC only. Retry the request; contact support if persistent |
| 7004 | Production environment blocked          | Switch to sandbox/staging environment                 |
| 7005 | USDT chain not supported                | Use `ethereum` as the `chain` for USDT claims          |
| 7006 | Faucet payout failed                    | USDT only. Retry the request; contact support if persistent |

***

### FAQ

<br />

<Accordion title="How long does it take to receive the funds?" icon="fa-solid fa-message-question">
  Testnet payouts typically arrive within a few seconds to a minute, depending on the chain's block confirmation time.
</Accordion>

<Accordion title="When does the daily quota reset?" icon="fa-solid fa-message-question">
  At midnight UTC each day. Each chain/currency combination's quota is tracked independently.
</Accordion>

<Accordion title="Can I claim 500 USDC on Ethereum AND 500 USDT on Ethereum in the same day?" icon="fa-solid fa-message-question">
  Yes. The 500 daily limit applies per chain **and** per currency, so USDC and USDT on the same chain don't share a quota.
</Accordion>

<Accordion title="What happens if a payout fails?" icon="fa-solid fa-message-question">
  You will receive a `7003` (USDC) or `7006` (USDT) error. The amount is **not** deducted from your daily quota if the payout fails. Retry the request.
</Accordion>

<Accordion title="Can I use the same wallet address across multiple chains?" icon="fa-solid fa-message-question">
  Yes, for EVM-compatible chains (Ethereum, Polygon, Arbitrum, Avalanche, Optimism, Base) you can use the same `0x` address. Stellar and Solana require their native address formats.
</Accordion>

<Accordion title="When should I use address_tag?" icon="fa-solid fa-message-question">
  Only for chains that use memos/tags (e.g. Stellar). Do not send it for chains that do not support tags — the request will be rejected.
</Accordion>

<Accordion title="Is there a rate limit on API calls (beyond the daily quota)?" icon="fa-solid fa-message-question">
  The standard OwlPay API rate limits apply. There is no additional per-minute throttle specific to the faucet endpoint.
</Accordion>

<br />

<Callout icon="📘" theme="info">
  **Legacy endpoint:** `POST /api/v1/applications/faucet/usdc` (USDC only, all chains above) remains available for existing integrations and behaves exactly as before. New integrations should use the unified `/api/v1/applications/faucet` endpoint above, which also supports USDT.
</Callout>

<br />
