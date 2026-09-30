---
title: Settlement strategy
hidden: true
metadata:
  robots: index
privacy:
  view: anyone_with_link
---
The **settlement\_strategy** field allows clients to specify how and when funds should be settled after a transfer is successfully received. This field is optional and can be used to configure immediate or scheduled settlement behavior for each transfer.

<br />

| Field Name            | Type     | Required | Allowed Values           | Default     |
| --------------------- | -------- | -------- | ------------------------ | ----------- |
| `settlement_strategy` | `string` | No       | `immediate`, `scheduled` | `immediate` |

<br />

### Allowed Values and Behavior

* immediate: When set to **immediate**, the system will settle the funds as soon as the transfer is confirmed, the scenario is usually a wallet provider who needs to settle the funds to the wallet user immediately after the wallet user deposits funds
* scheduled: When set to scheduled, the settlement will be handled on a fixed schedule defined by the system (e.g., every 24 hours), the usage scenario is usually to collect stablecoins through off-ramp.

<br />

> 🚧 **On-Chain (Stablecoin to Stablecoin)** has not been **scheduled** yet.

Example Request:

```curl
curl --location --request POST 'https://harbor-sandbox.owlpay.com/api/v1/transfer' \
--header 'Content-Type: application/json' \
--header 'Accept: application/json' \
--header 'X-API-KEY: {{API_KEY}}' \
--header 'Idempotency-Key: {{Idempotency-Key}}' \
--data-raw '{
    "on_behalf_of": "{{YOUR_CUSTOMER_UUID_LOCALHOST}}",
    "settlement_strategy": "scheduled",
    "destination": {
        ...,
    },
    "source": {
        ...,
    },
    "commission": {
        ...,
    },
    ...
}'



```