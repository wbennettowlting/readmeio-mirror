---
title: Commission Fees
hidden: false
metadata:
  robots: index
privacy:
  view: public
---
Harbor provides support for setting transaction commissions to ensure revenue generation on every **Transfer**.

<br />

### Commission

| Field      | Type   | Description                                               |
| :--------- | :----- | :-------------------------------------------------------- |
| percentage | String | The commission rate as a percentage (e.g., 1.05 = 1.05%). |
| amount     | String | The fixed commission fee (e.g., 1.00 for $1).             |

```
commission_fee = (total * commission.percentage) + commission.amount
```

***

### Example Calculation

* commission: percentage = 1.05%, amount = 1.00 USD
* source.amount = 100.00 USD

```
commission_fee = 100(total) * 1.05%(commsion.percentage) + 1.00(commission.amount) = 2.05
```

***

#### 1. Source Amount Model (source.amount is fixed)

The payer sends a fixed source amount.  
The destination amount is calculated after deducting commission.

```
destination.amount = source.amount - commission_fee
```

**Restriction:**  
A maximum commission rate is enforced because the destination amount cannot be negative.

***

#### 2. Destination Amount Model (destination.amount is fixed)

The recipient receives a fixed destination amount.  
The required source amount increases to cover the commission.

```
source.amount = destination.amount + commission_fee
```

**No technical limit:**  
The system can always compute a valid source.amount, so no maximum commission limit is required.

<Callout icon="📘" theme="info">
  **Markup Percentage Limitation**

  When the transaction uses the **source.amount** (i.e., the payer sends a fixed amount), the system will enforce a maximum markup limit.
  This is because excessive markup may result in an invalid or negative destination amount.

  For the **destination.amount**, no technical markup limit is required, as the payer’s required amount will automatically adjust to cover the markup.
</Callout>

***

### On-Chain (Stablecoin-to-Stablecoin) Transfers

On-chain transfers (e.g., USDC on Ethereum → USDC on Solana) can also involve a **blockchain network fee**, when it is billed to the customer rather than absorbed by OwlPay. It is deducted from the same principal as your commission, so the two models above need one small refinement for on-chain transfers.

#### Source Amount Model (refined)

`commission_fee` is calculated the same way — only the destination amount also nets out the network fee:

```
commission_fee = source.amount * commission.percentage + commission.amount

destination.amount = source.amount - commission_fee - network_fee
```

**Example:**

* source.amount = 500.00 USDC
* commission: percentage = 3%, amount = 0
* network fee: 5.00 USDC

```
commission_fee = 500 * 3% + 0 = 15.00
destination.amount = 500 - 15.00 - 5.00(network_fee) = 480.00
```

#### Destination Amount Model (refined)

When you fix the **destination.amount**, the required source.amount has to gross up for the network fee before applying your commission rate:

```
source.amount = (destination.amount + network_fee) / (1 - commission.percentage / 100)

commission_fee = source.amount * commission.percentage + commission.amount
```

**Example:**

* destination.amount = 500.00 USDC
* commission: percentage = 3%, amount = 0
* network fee: 5.00 USDC

```
source.amount = (500 + 5.00) / (1 - 0.03) = 520.618556
commission_fee = 520.618556 * 3% + 0 = 15.618556
```

<Callout icon="📘" theme="info">
  **Why isn't `commission_fee` simply `destination.amount * commission.percentage`?**

  Your commission is a share of what the payer actually sends (`source.amount`), not of what the recipient receives. When you fix the destination amount, OwlPay first works out how much the payer needs to send to also cover the network fee — then calculates your commission from that real `source.amount`. This ensures your commission reflects the full principal moved, not a smaller amount that ignores the network fee.
</Callout>

#### Exchange Rate

The `exchange_rate` returned with each quote is `destination.amount` divided by the payer's principal **after your commission is removed** (i.e., excluding your commission from the base, since that's revenue you're taking, not part of the conversion):

```
exchange_rate = destination.amount / (source.amount - commission_fee)
```

**Using the Source Amount Model example above:**

```
exchange_rate = 480.00 / (500.00 - 15.00) = 480.00 / 485.00 = 0.98969072
```

**Using the Destination Amount Model example above:**

```
exchange_rate = 500.00 / (520.618556 - 15.618556) = 500.00 / 505.00 = 0.99009900
```

<br />

#### On-Chain Example Code (`source.amount` fixed):

```curl
curl --location 'https://harbor-sandbox.owlpay.com/api/v2/transfers/quotes' \
--header 'Accept: application/json' \
--header 'Content-Type: application/json' \
--header 'X-API-KEY: {{API_KEY}}' \
--data '{
    "source": {
        "chain": "ethereum",
        "asset": "USDC",
        "amount": 500,
        "type": "individual"
    },
    "destination": {
        "chain": "solana",
        "asset": "USDC",
        "type": "individual"
    },
    "commission": {
        "percentage": 3,
        "amount": 0
    }
}'
```

<br />

```json
{
    "data": [
        {
            "payment_method": "ON_CHAIN",
            "chain": "ethereum",
            "source_amount": "500.000000",
            "source_currency": "USDC",
            "destination_amount": "480.000000",
            "destination_currency": "USDC",
            "destination_chain": "solana",
            "exchange_rate": "0.98969072",
            "exchange_pair": "USDC/USDC",
            "fees": [
                {
                    "type": "HARBOR_FEE",
                    "amount": "6.500000",
                    "currency": "USDC",
                    "charge_from": "OwlPay Harbor",
                    "payer": "Wallet Service Provider"
                },
                {
                    "type": "COMMISSION_FEE",
                    "amount": "15.000000",
                    "currency": "USDC",
                    "charge_from": "Wallet Service Provider",
                    "payer": "customer"
                },
                {
                    "type": "BLOCKCHAIN_NETWORK_FEE",
                    "amount": "5.000000",
                    "currency": "USDC",
                    "charge_from": "owlting",
                    "payer": "customer"
                }
            ],
            ...
        }
    ]
}
```

The payer sends the fixed `source_amount` (500.000000 USDC). `commission_fee` (15.000000) is deducted along with the network fee, so the recipient receives the remaining `destination_amount` (480.000000 USDC).

<br />

#### On-Chain Example Code (`destination.amount` fixed):

```curl
curl --location 'https://harbor-sandbox.owlpay.com/api/v2/transfers/quotes' \
--header 'Accept: application/json' \
--header 'Content-Type: application/json' \
--header 'X-API-KEY: {{API_KEY}}' \
--data '{
    "source": {
        "chain": "ethereum",
        "asset": "USDC",
        "type": "individual"
    },
    "destination": {
        "chain": "solana",
        "asset": "USDC",
        "amount": 500,
        "type": "individual"
    },
    "commission": {
        "percentage": 3,
        "amount": 0
    }
}'
```

<br />

```json
{
    "data": [
        {
            "payment_method": "ON_CHAIN",
            "chain": "ethereum",
            "source_amount": "520.618556",
            "source_currency": "USDC",
            "destination_amount": "500.000000",
            "destination_currency": "USDC",
            "destination_chain": "solana",
            "exchange_rate": "0.99009900",
            "exchange_pair": "USDC/USDC",
            "fees": [
                {
                    "type": "HARBOR_FEE",
                    "amount": "6.500000",
                    "currency": "USDC",
                    "charge_from": "OwlPay Harbor",
                    "payer": "Wallet Service Provider"
                },
                {
                    "type": "COMMISSION_FEE",
                    "amount": "15.618556",
                    "currency": "USDC",
                    "charge_from": "Wallet Service Provider",
                    "payer": "customer"
                },
                {
                    "type": "BLOCKCHAIN_NETWORK_FEE",
                    "amount": "5.000000",
                    "currency": "USDC",
                    "charge_from": "owlting",
                    "payer": "customer"
                }
            ],
            ...
        }
    ]
}
```

The payer sends `source_amount` (520.618556 USDC) and the recipient receives exactly the requested `destination_amount` (500.000000 USDC). `commission_fee` (15.618556) is your revenue share, calculated from the real `source_amount` as shown above.
