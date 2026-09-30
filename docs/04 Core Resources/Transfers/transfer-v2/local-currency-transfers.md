---
title: Transfers with Local Currency
metadata:
  robots: index
privacy:
  view: public
---
OwlPay Harbor now supports cross-border transfers to multiple regions worldwide, allowing developers and businesses to easily convert USDC into local currencies and send funds directly to recipients’ bank accounts.

### Supported Countries & Currencies

OwlPay Harbor supports off-ramp (crypto → fiat) withdrawals to the following non-US regions and local currencies.

<Callout icon="📘" theme="info">
  **Supported Countries & Currencies**

  Please see <Anchor label="Fiat payment methods" target="_blank" href="https://harbor-developers.owlpay.com/docs/fiat-payment-methods">Fiat payment methods</Anchor> for the latest support roadmap.
</Callout>

### Transfer Guides

Select a transfer type to view the full integration guide:

<Cards columns={3}>
  <Card title="On-ramp (Fiat → Stablecoin)" href="/docs/on-ramp" icon="fa-arrow-up">
    Convert fiat currency (e.g. USD) into USDC and send to a blockchain address.
  </Card>

  <Card title="Off-ramp (Stablecoin → Fiat)" href="/docs/off-ramp" icon="fa-arrow-down">
    Convert USDC into local fiat currency (e.g. MXN, BRL, HKD) and settle to a bank account.
  </Card>

  <Card title="On-chain (Stablecoin → Stablecoin)" href="/docs/on-chain" icon="fa-exchange">
    Transfer USDC across different blockchains (e.g. Ethereum → Avalanche).
  </Card>
</Cards>

<br />

### API Flow Overview

The OwlPay Harbor Transfer API uses a two-step process to ensure real-time FX rates and secure fund settlement.

<br />

#### Step 1: Get a Quote

First, call the **Quote API** to obtain an exchange rate and conversion preview:

<Callout icon="📘" theme="info">
  **Source Amount & Destination Amount**

  We support quotes by source amount and by destination amount.
  You can quote how much the recipient receives or how much the sender needs to pay.
</Callout>

<br />

##### Request Example (Source amount):

```curl
curl --location --request POST 'https://harbor-sandbox.owlpay.com/api/v2/transfers/quotes' \
--header 'Content-Type: application/json' \
--header 'Accept: application/json' \
--header 'X-API-KEY: {{API_KEY}}' \
--header 'Idempotency-Key: {{Idempotency-Key}}' \
--data-raw '{
  "source": {
      "chain": "ethereum",
      "country": "US",
      "asset": "USDC",
      "type": "individual",
      "amount": "10000.00"
  },
  "destination": {
      "country": "MX",
      "asset": "MXN",
      "type": "individual"
  },
  "commission": {
      "percentage": "1.00",
      "amount": 0
  }
}'
```

<br />

##### Request Example (Destination amount):

```curl
curl --location --request POST 'https://harbor-sandbox.owlpay.com/api/v2/transfers/quotes' \
--header 'Accept: application/json' \
--header 'Content-Type: application/json' \
--header 'X-API-KEY: {{API_KEY}}' \
--header 'Idempotency-Key: {{Idempotency-Key}}' \
--data '{
    "source": {
        "chain": "ethereum",
        "country": "US",
        "asset": "USDC",
        "type": "individual"
    },
    "destination": {
        "country": "MX",
        "asset": "MXN",
        "type": "individual",
        "amount": 100000
    },
    "commission": {
        "percentage": "1.00",
        "amount": 0
    }
}'

```

<br />

##### Response with Source amount:

```json
{
    "data": [
        {
            "id": "quote_OfwS3Cg9kktEhaYmzW5wsvFMJfIs9aougW19mYYr",
            "payment_method": "SPEI",
            "chain": "ethereum",
            "source_country": "US",
            "destination_country": "MX",
            "source_amount": 1000,
            "source_currency": "USDC",
            "destination_amount": "17941.720000",
            "destination_currency": "MXN",
            "exchange_rate": "17.941720",
            "exchange_pair": "USDC/MXN",
            "fiat_settlement_time_min": 0,
            "fiat_settlement_time_max": 5,
            "fiat_settlement_time_unit": "MINUTES",
            "source_type": "individual",
            "destination_type": "individual",
            "quote_expire_date": "2025-12-22T18:06:16+00:00",
            "crypto_funds_settlement_expire_date": "2025-12-22T19:05:46+00:00",
            "fees": [
                {
                    "type": "HARBOR_FEE",
                    "amount": "51.00000000",
                    "currency": "USDC",
                    "charge_from": "OwlPay Harbor",
                    "payer": "Wallet Service Provider"
                },
                {
                    "type": "COMMISSION_FEE",
                    "amount": "10.00000000",
                    "currency": "USDC",
                    "charge_from": "Wallet Service Provider",
                    "payer": "customer"
                }
            ],
            "created_at": "2025-12-22T18:05:46+00:00",
            "updated_at": "2025-12-22T18:05:46+00:00"
        }
    ]
}
```

<br />

#### Step 2: Get Requirements with Quote (quote_id)

##### Request Example (with quote_id)

```curl
curl --location --request GET 'https://harbor-sandbox.owlpay.com/api/v2/transfers/quotes/{{quote_id}}/requirements' \
--header 'Content-Type: application/json' \
--header 'Accept: application/json' \
--header 'X-API-KEY: {{API_KEY}}'
```

<br />

##### Response:

```json
{
    "$schema": "https://json-schema.org/draft/2020-12/schema",
    "type": "object",
    "additionalProperties": false,
    "required": [
        "quote_id",
        "destination",
        "on_behalf_of",
        "application_transfer_uuid"
    ],
    "properties": {
        "on_behalf_of": {
            "type": "string"
        },
        "quote_id": {
            "type": "string"
        },
        "application_transfer_uuid": {
            "type": "string"
        },
        "destination": {
            "type": "object",
            "additionalProperties": false,
            "required": [
                "beneficiary_info",
                "payout_instrument"
            ],
            "properties": {
                "beneficiary_info": {
                    "type": "object",
                    "additionalProperties": false,
                    "properties": {
                        "beneficiary_name": {
                            "type": "string",
                            "title": "Recipient Full Name",
                            "maxLength": 140
                        },
                        "beneficiary_address": {
                            "type": "object",
                            "additionalProperties": false,
                            "properties": {
                                "street": {
                                    "type": "string",
                                    "maxLength": 200
                                },
                                "city": {
                                    "type": "string",
                                    "maxLength": 80
                                },
                                "state_province": {
                                    "type": "string",
                                    "maxLength": 80
                                },
                                "postal_code": {
                                    "type": "string",
                                    "maxLength": 20
                                },
                                "country": {
                                    "type": "string",
                                    "minLength": 2,
                                    "maxLength": 2
                                }
                            },
                            "required": [
                                "street",
                                "city",
                                "country"
                            ]
                        },
                        "beneficiary_dob": {
                            "type": "string",
                            "title": "Recipient Date of Birth"
                        },
                        "beneficiary_id_doc_number": {
                            "type": "string",
                            "title": "Recipient ID Number",
                            "maxLength": 80
                        }
                    },
                    "required": [
                        "beneficiary_dob",
                        "beneficiary_name",
                        "beneficiary_id_doc_number",
                        "beneficiary_address"
                    ]
                },
                "payout_instrument": {
                    "type": "object",
                    "additionalProperties": false,
                    "properties": {
                        "mx_rfc": {
                            "type": "string",
                            "title": "RFC",
                            "pattern": "^[A-Z&Ñ]{3,4}\\d{6}[A-Z0-9]{3}$"
                        },
                        "mx_clabe": {
                            "type": "string",
                            "title": "CLABE",
                            "pattern": "^\\d{18}$"
                        },
                        "mx_debit_card_number": {
                            "type": "string",
                            "title": "Debit Card Number",
                            "pattern": "^\\d{16}$"
                        },
                        "phone_number": {
                            "type": "string",
                            "title": "Phone Number (E.164)",
                            "pattern": "^\\+[1-9]\\d{1,14}$"
                        }
                    },
                    "required": [
                        "mx_clabe"
                    ]
                },
                "transfer_purpose": {
                    "type": "string"
                },
                "is_self_transfer": {
                    "type": "boolean"
                },
                "beneficiary_receiving_wallet_type": {
                    "type": "string"
                },
                "beneficiary_institution_name": {
                    "type": "string"
                }
            }
        }
    },
    "title": "MX_SPEI",
    "$comment": "{\"bank_title\":\"MX_SPEI\",\"mapping_version\":1}"
}
```

<br />

<Callout icon="📘" theme="info">
  This API responses are defined and validated according to **JSON Schema Draft 2020-12**.

  Each response payload strictly follows a predefined schema, including (but not limited to):

  * Data types (type)
  * Required fields (required)
  * Field formats and length constraints (pattern, minLength, maxLength)
  * Conditional structures (oneOf, anyOf, allOf)
  * Nested objects and arrays (properties, items)
</Callout>

<br />

#### Step 3: Create v2 Transfer API with Quote (quote_id) and Requirements

##### Request Example (with quote_id)

```curl
curl --location --request POST 'https://harbor-sandbox.owlpay.com/api/v2/transfers' \
--header 'Content-Type: application/json' \
--header 'Accept: application/json' \
--header 'X-API-KEY: {{API_KEY}}' \
--header 'Idempotency-Key: {{Idempotency-Key}}' \
--data-raw '{
    "on_behalf_of": "{{YOUR_CUSTOMER_UUID}}",
    "quote_id": "{{QUOTE_ID}}",
    "application_transfer_uuid": "{{APPLICATION_TRANSFER_UUID}}",
    "destination": {
        "beneficiary_info": {
            "beneficiary_name": "CHAN TAI MAN",
            "beneficiary_address": {
                "street": "Flat 12A, 8/F, Prosperity Building, 123 Nathan Road",
                "city": "Hong Kong",
                "state_province": "not applicable",
                "postal_code": "00000",
                "country": "HK"
            },
            "beneficiary_id_doc_number": "1234567890",
            "beneficiary_dob": "2000-01-01"
        },
        "payout_instrument": {
            "mx_clabe": "032180000118359719"
        },
        "transfer_purpose": "SALARY",
        "is_self_transfer": false,
        "beneficiary_receiving_wallet_type": "personalWallet",
        "beneficiary_institution_name": "MetaMask"
    }
}'
```

<br />

<br />
