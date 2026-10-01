---
title: Off-ramp
hidden: false
metadata:
  robots: index
x-privacy:
  view: public
---
Convert stablecoin (e.g., USDC) into fiat currency (e.g., USD, HKD) and deliver it to a recipient's bank account.

<Callout icon="👉" theme="info">

**Direct API Reference:**
* [API Reference: Create a Transfer (V2)](doc:createatransferv2)

</Callout>

### Transfer Flow Overview

The off-ramp transfer follows a three-step process: get a quote, retrieve the transfer requirements, then create the transfer.

```mermaid
sequenceDiagram
    participant App as Your App
    participant Harbor as Harbor
    participant Recipient

    Note over App,Recipient: Step 1 – Get Quote
    App->>Harbor: POST /v2/transfers/quotes
    Harbor-->>App: quote_id + pricing

    Note over App,Recipient: Step 2 – Get Requirements
    App->>Harbor: GET /v2/transfers/quotes/{id}/requirements
    Harbor-->>App: JSON Schema

    Note over App,Recipient: Step 3 – Create Transfer
    App->>Harbor: POST /v2/transfers
    Harbor-->>App: transfer object + transfer_instructions

    Note over App,Recipient: Settlement
    App->>Harbor: Customer sends USDC to instruction_address on-chain
    Harbor->>Recipient: Harbor sends fiat to recipient's bank account
```

| Step | API Endpoint | Purpose |
|------|-------------|---------|
| 1 | `POST /v2/transfers/quotes` | Get exchange rate, fees, and a `quote_id` |
| 2 | `GET /v2/transfers/quotes/{quote_id}/requirements` | Get the JSON Schema defining required fields for the transfer |
| 3 | `POST /v2/transfers` | Create the transfer using the `quote_id` and validated payload |

---

### Step 1: Get Quote

#### Request Example

```curl
curl --location 'https://harbor-sandbox.owlpay.com/api/v2/transfers/quotes' \
--header 'Accept: application/json' \
--header 'Content-Type: application/json' \
--header 'X-API-KEY: ••••••' \
--data '{
    "source": {
        "country": "US",
        "chain": "ethereum",
        "asset": "USDC",
        "type": "individual"
    },
    "destination": {
        "country": "HK",
        "asset": "USD",
        "amount": 3000,
        "type": "individual"
    },
    "commission": {
        "amount": 10,
        "percentage": 0
    }
}'
```

#### Response Example

```json
{
    "data": [
        {
            "id": "quote_fo3AirZ3ZYtVayEot5pCoWMPLteMKBtFQfZtPDYv",
            "payment_method": "WIRE",
            "chain": "ethereum",
            "source_country": "US",
            "destination_country": "HK",
            "source_amount": "3419.090909",
            "source_currency": "USDC",
            "destination_amount": "3000.00",
            "destination_currency": "USD",
            "exchange_rate": "0.880000",
            "exchange_pair": "USDC/USD",
            "fiat_settlement_time_min": 1,
            "fiat_settlement_time_max": 3,
            "fiat_settlement_time_unit": "DAYS",
            "source_type": "individual",
            "destination_type": "individual",
            "quote_expire_date": "2025-12-26T17:24:12+00:00",
            "crypto_funds_settlement_expire_date": "2025-12-26T17:24:12+00:00",
            "fees": [
                {
                    "type": "HARBOR_FEE",
                    "amount": "55.125628",
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
            "created_at": "2025-12-26T15:54:12+00:00",
            "updated_at": "2025-12-26T15:54:12+00:00"
        }
    ]
}
```

<br />

### Step 2: Get Requirements by quote_id

#### Request Example

```curl
curl --location 'https://harbor-sandbox.owlpay.com/api/v2/transfers/quotes/{{quote_uuid}}/requirements' \
--header 'x-api-key: ••••••'
```

#### Response Example

```
{
    "$schema": "https://json-schema.org/draft/2020-12/schema",
    "type": "object",
    "additionalProperties": false,
    "required": [
        "quote_id",
        "source",
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
        "source": {
            "type": "object",
            "additionalProperties": false,
            "required": [
                "payment_instrument"
            ],
            "properties": {
                "payment_instrument": {
                    "type": "object",
                    "additionalProperties": false,
                    "properties": {
                        "address": {
                            "type": "string"
                        }
                    },
                    "required": [
                        "address"
                    ]
                }
            }
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
                        "account_holder_name": {
                            "type": "string",
                            "title": "Account Holder Name",
                            "maxLength": 140
                        },
                        "bank_name": {
                            "type": "string",
                            "title": "Bank Name",
                            "maxLength": 140
                        },
                        "account_number": {
                            "type": "string",
                            "title": "Account Number",
                            "maxLength": 34
                        },
                        "swift_code": {
                            "type": "string",
                            "title": "SWIFT/BIC",
                            "pattern": "^[A-Z]{4}[A-Z]{2}[A-Z0-9]{2}([A-Z0-9]{3})?$"
                        }
                    },
                    "required": [
                        "account_holder_name",
                        "bank_name",
                        "account_number",
                        "swift_code"
                    ]
                },
                "transfer_purpose": {
                    "type": "string",
                    "enum": [
                        "TRANSFER_TO_OWN_ACCOUNT",
                        "FAMILY_MAINTENANCE",
                        "EDUCATION",
                        "MEDICAL_TREATMENT",
                        "HOTEL",
                        "TRAVEL",
                        "REPAYMENT_OF_LOANS",
                        "TAX_PAYMENT",
                        "PURCHASE_PROPERTY",
                        "PROPERTY_RENTAL",
                        "INSURANCE_PREMIUM",
                        "PRODUCT_INDEMNITY_INSURANCE",
                        "INSURANCE_CLAIMS",
                        "MUTUAL_FUND_INVESTMENT",
                        "INVESTMENT_SHARES",
                        "DONATIONS",
                        "SALARY",
                        "INFO_SERVICE",
                        "ADVERTISING",
                        "ROYALTY_FEES",
                        "BROKER_FEES",
                        "ADVISOR_FEES",
                        "REPRESENTATIVE_EXPENSES",
                        "CONSTRUCTION",
                        "TRANSPORTATION",
                        "EXPORTED_GOODS",
                        "DELIVERY_FEES",
                        "GENERAL_GOODS_OFFLINE"
                    ]
                },
                "is_self_transfer": {
                    "type": "boolean"
                }
            }
        }
    },
    "title": "HK_WIRE",
    "$comment": "{\"bank_title\":\"HK_WIRE\",\"mapping_version\":1}"
}
```

<br />

### Step 3: Create Transfer

#### &#x20;Request Example

```curl
curl --location 'https://harbor-sandbox.owlpay.com/api/v2/transfers' \
--header 'Content-Type: application/json' \
--header 'Accept: application/json' \
--header 'X-API-KEY: ••••••' \
--data '{
    "on_behalf_of": "{{YOUR_CUSTOMER_UUID}}",
    "quote_id": "{{QUOTE_UUID}}",
    "application_transfer_uuid": "{{YOUR_APPLICATION_TRANSFER_UUID}}",
    "source": {
        "payment_instrument": {
            "address": "0x4B20993Bc481177ec7E8f571ceCaE8A9e22C02db"
        }
    },
    "destination": {
        "beneficiary_info": {
            "beneficiary_dob": "1990-01-01",
            "beneficiary_id_doc_number": "12345678",
            "beneficiary_address": {
                "street": "xxx 1234th Ave S",
                "city": "Minneapolis",
                "state_province": "MN",
                "postal_code": "55416",
                "country": "US"
            },
            "beneficiary_name": "John Doe"
        },
        "payout_instrument": {
            "bank_name": "The Hongkong and Shanghai Banking Corporation Limited",
            "account_holder_name": "CHAN TAI MAN",
            "account_number": "123456789012",
            "swift_code": "HSBCHKHH"
        },
        "transfer_purpose": "SALARY",
        "is_self_transfer": true
    }
}'
```

<br />

#### Response Example

```json
{
    "data": {
        "uuid": "transfer_I3eI0LUpUZi7zkewHlWDgIHQlqS3AEBOE2iLt3Ce",
        "object": "transfer",
        "status": "pending_customer_transfer_start",
        "type": "off-ramp",
        "settlement_strategy": "immediate",
        "source_received": false,
        "on_behalf_of": "cus_9QMwIz1aLtIM9zzFUeqnqrl2mZRkjzmWeaszNbiS",
        "source": {
            "chain": "ethereum",
            "payment_instrument": {
                "chain": "ethereum",
                "address": "0x4B20993Bc481177ec7E8f571ceCaE8A9e22C02db"
            },
            "asset": "USDC",
            "amount": "3028.00000000"
        },
        "destination": {
            "asset": "USD",
            "amount": "3000.00000000",
            "payout_instrument": {
                "bank_name": "The Hongkong and Shanghai Banking Corporation Limited",
                "account_holder_name": "CHAN TAI MAN",
                "account_number": "123456789012",
                "swift_code": "HSBCHKHH"
            },
            "is_self_transfer": false,
            "transfer_purpose": "SALARY"
        },
        "application_transfer_uuid": "OWLTING_HARBOR_OFF_RAMP_002051",
        "transfer_instructions": {
            "instruction_chain": "ethereum",
            "instruction_address": "0x020f4fd0e19b1db5e28e3afd9ecf09227ccf6dbf",
            "instruction_memo": null
        },
        "commission": {
            "percentage": "0",
            "amount": "10"
        },
        "fees": [
            {
                "type": "HARBOR_FEE",
                "amount": "15.125628",
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
        "receipt": {
            "initial_asset": "USDC",
            "initial_amount": "3028.00000000",
            "commission_fee": "10.000000",
            "harbor_fee": "15.125628",
            "final_asset": "USD",
            "final_amount": "3000.00000000",
            "exchange_rate": "0.99403600",
            "tracking_number": null
        },
        "meta_data": null,
        "created_at": "2025-12-26T17:04:12+00:00",
        "updated_at": "2025-12-26T17:04:15+00:00"
    }
}
```
