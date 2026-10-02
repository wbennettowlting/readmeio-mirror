---
title: Quotes
hidden: false
metadata:
  robots: index
x-privacy:
  view: public
---
### Overview

In **Transfer API (V2)**, all transfers **must be preceded by a successful Quote API call**.

The Quote API is responsible for:

* Locking pricing and fees
* Determining settlement routes
* Calculating commissions and platform fees
* Defining expiration windows for execution

<br />

<Callout icon="👉" theme="info">

**Direct API Reference:**
* [API Reference: Create a Transfer Quote (V2)](/reference/createatransferquotev2)

</Callout>

***

### Step 1: Create a Quote

Before initiating a transfer, you must first request a quote.

#### Quote Request Example

```curl
{
    "source": {
        "country": "US",
        "chain": "ethereum",
        "asset": "USDC",
        "type": "individual"
    },
    "destination": {
        "chain": "avalanche",
        "asset": "USDC",
        "amount": 3000,
        "type": "individual"
    },
    "commission": {
        "amount": 10,
        "percentage": 0
    }
}
```

***

### Standard Quote Response Example

> 📊 **Array Notice**
> 
> Note that the response `data` field is an **Array**. A single quote request can yield multiple quote objects for different payment and routing methods.

```json JSON
{
    "data": [
        {
            "id": "quote_BoWt0rEkZgjEFkAKJSVAqyZqZxfy3gllaipYhJHH",
            "payment_method": "ON_CHAIN",
            "chain": "ethereum",
            "source_country": null,
            "destination_country": null,
            "source_amount": "3010.000000",
            "source_currency": "USDC",
            "destination_amount": "3000.000000",
            "destination_currency": "USDC",
            "destination_chain": "avalanche",
            "exchange_rate": "1.000000",
            "exchange_pair": "USDC/USDC",
            "crypto_settlement_time_min": 5,
            "crypto_settlement_time_max": 20,
            "crypto_settlement_time_unit": "MINUTES",
            "source_type": "individual",
            "destination_type": "individual",
            "quote_expire_date": "2025-12-26T18:38:42+00:00",
            "crypto_funds_settlement_expire_date": "2025-12-26T18:38:42+00:00",
            "fees": [
                {
                    "type": "HARBOR_FEE",
                    "amount": "55.160875",
                    "currency": "USDC",
                    "charge_from": "OwlPay Harbor",
                    "payer": "Wallet Service Provider"
                },
                {
                    "type": "COMMISSION_FEE",
                    "amount": "10.000000",
                    "currency": "USDC",
                    "charge_from": "Wallet Service Provider",
                    "payer": "customer"
                }
            ],
            "created_at": "2025-12-26T17:08:42+00:00",
            "updated_at": "2025-12-26T17:08:42+00:00"
        }
    ]
}
```

***

### Quote Response Example with Dynamic Customer Limits

If you supply the `on_behalf_of` parameter (the customer UUID), the API automatically calculates and appends the **`customer_limits`** structure, showing daily, weekly, and dynamic remaining balances for your customer's profile.

```json JSON
{
    "data": [
        {
            "id": "quote_BoWt0rEkZgjEFkAKJSVAqyZqZxfy3gllaipYhJHH",
            "payment_method": "ON_CHAIN",
            "chain": "ethereum",
            "source_country": null,
            "destination_country": null,
            "source_amount": "3010.000000",
            "source_currency": "USDC",
            "destination_amount": "3000.000000",
            "destination_currency": "USDC",
            "destination_chain": "avalanche",
            "exchange_rate": "1.000000",
            "exchange_pair": "USDC/USDC",
            "crypto_settlement_time_min": 5,
            "crypto_settlement_time_max": 20,
            "crypto_settlement_time_unit": "MINUTES",
            "source_type": "individual",
            "destination_type": "individual",
            "quote_expire_date": "2025-12-26T18:38:42+00:00",
            "crypto_funds_settlement_expire_date": "2025-12-26T18:38:42+00:00",
            "fees": [
                {
                    "type": "HARBOR_FEE",
                    "amount": "55.160875",
                    "currency": "USDC",
                    "charge_from": "OwlPay Harbor",
                    "payer": "Wallet Service Provider"
                },
                {
                    "type": "COMMISSION_FEE",
                    "amount": "10.000000",
                    "currency": "USDC",
                    "charge_from": "Wallet Service Provider",
                    "payer": "customer"
                }
            ],
            "customer_limits": {
                "per_transaction_limit": "100000.000000",
                "daily_limit": "500000.000000",
                "weekly_limit": "2000000.000000",
                "monthly_limit": "5000000.000000",
                "current_remaining": "489500.000000"
            },
            "created_at": "2025-12-26T17:08:42+00:00",
            "updated_at": "2025-12-26T17:08:42+00:00"
        }
    ]
}
```
