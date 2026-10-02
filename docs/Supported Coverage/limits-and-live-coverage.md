---
title: Limits and Live Coverage
excerpt: Retrieve the exact withdrawal limits and supported currency pairs for your application.
hidden: false
---
The coverage pages give a general overview. Your exact limits (min/max) and supported currency pairs depend on your application's configuration and on the sender and recipient types (individual or business).

To retrieve the settings that apply to your integration in real time, use [**Get Withdrawal Settings (V2)**](/reference/getwithdrawalsettingsv2).

### Response fields

The API returns a list of supported withdrawal paths. Each path includes:

| Field                  | Description                                                                   |
| :--------------------- | :---------------------------------------------------------------------------- |
| `source_currency`      | The currency being sent (e.g., `USDC`).                                       |
| `destination_country`  | The target country code (ISO 3166-1 alpha-2, e.g., `MX`).                     |
| `destination_currency` | The currency to be received in the target country (e.g., `MXN`, `USD`).       |
| `source_type`          | The sender type: `individual` or `business`.                                  |
| `destination_type`     | The recipient type: `individual` or `business`.                               |
| `source_amount`        | An object containing `min`, `max`, and `currency` for the source amount.      |
| `destination_amount`   | An object containing `min`, `max`, and `currency` for the destination amount. |

### Example response entry

```json
{
  "source_currency": "USDC",
  "destination_country": "MX",
  "destination_currency": "MXN",
  "source_type": "individual",
  "destination_type": "individual",
  "source_amount": {
    "currency": "USDC",
    "min": 18,
    "max": 9998
  },
  "destination_amount": {
    "currency": "MXN",
    "min": 322.68,
    "max": 179233.15
  }
}
```
