---
title: Withdrawal Settings
hidden: false
metadata:
  robots: index
x-privacy:
  view: public
---
This API allows developers to retrieve the current withdrawal configurations supported for your application. You can use this endpoint to identify supported **currency pairs**, **destination countries**, **types (Individual/Business)**, and their respective **transaction limits**.

### Endpoint

`GET` `/api/v2/transfers/withdrawal_settings`

### Request Headers

| Header      | Type   | Required | Description                        |
| :---------- | :----- | :------- | :--------------------------------- |
| `x-api-key` | String | Yes      | Your unique Harbor API access key. |

### Request Example

```curl
curl --location 'https://harbor-sandbox.owlpay.com/api/v2/transfers/withdrawal_settings' \
--header 'x-api-key: {{YOUR_API_KEY}}'
```

<br />

### Response Example

#### Data Object Attributes

| Field                  | Type   | Description                                             |
| :--------------------- | :----- | :------------------------------------------------------ |
| `source_currency`      | String | The currency to be withdrawn (e.g., USDC).              |
| `destination_country`  | String | Destination country ISO code (e.g., MX, BR, US, TW).    |
| `destination_currency` | String | The local currency received (e.g., MXN, USD, BRL).      |
| `source_type`          | String | Originator type: `individual` or `business`.            |
| `destination_type`     | String | Recipient type: `individual` or `business`.             |
| `source_amount`        | Object | Limits in source currency (contains `min`, `max`).      |
| `destination_amount`   | Object | Limits in destination currency (contains `min`, `max`). |

***

### Response Example (Simplified)

```json
{
    "data": [
        {
            "source_currency": "USDC",
            "destination_country": "US",
            "destination_currency": "USD",
            "source_type": "business",
            "destination_type": "business",
            "source_amount": {
                "currency": "USDC",
                "min": 18,
                "max": 1000000
            },
            "destination_amount": {
                "currency": "USD",
                "min": 10,
                "max": 1000000
            }
        },
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
        },
        {
            "source_currency": "USDC",
            "destination_country": "TW",
            "destination_currency": "USD",
            "source_type": "business",
            "destination_type": "individual",
            "source_amount": {
                "currency": "USDC",
                "min": 43,
                "max": 1000000
            },
            "destination_amount": {
                "currency": "USD",
                "min": 35,
                "max": 1000000
            }
        },
        {...},
    ]
}
```
