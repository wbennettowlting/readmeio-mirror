---
title: Onboard Individual
metadata:
  robots: index
x-privacy:
  view: public
---
This guide explains how to onboard an individual customer (KYC) through a direct API integration. Collect individual profiles and identity documents directly inside your own application and submit them to Harbor in a single API request.

***

### 1. Prerequisites & Endpoints

Before submitting individual onboarding, ensure that:

1. The customer has been created as an **individual** customer (`POST /api/v1/customers`).
2. The customer is active and has signed the platform agreement.
3. The customer is not already KYC-verified.

#### Endpoints

| Method  | Path                                             | Purpose                                                                  |
| ------- | ------------------------------------------------ | ------------------------------------------------------------------------ |
| `POST`  | `/api/v1/customers/{{customer_uuid}}/onboarding` | Submit the individual onboarding payload. Returns **202**.               |
| `GET`   | `/api/v1/customers/{{customer_uuid}}/onboarding` | Read the latest onboarding status.                                       |
| `PATCH` | `/api/v1/customers/{{customer_uuid}}/onboarding` | Correct/replace data while status is `action_required`. Returns **202**. |

***

### 2. Reference-Data Lookups (Meta APIs)

When collecting individual onboarding data, some fields (such as occupations, countries, states, or required document parts) only accept specific values from dynamic lists that change over time. 

Before presenting your UI or submitting onboarding data, you must query the corresponding lookup endpoints to get the valid options and validation requirements.

Please refer to the complete [**Onboarding Meta APIs**](/docs/onboarding-lookups) guide for details.

***

### 3. Submitting Individual Onboarding (POST)

The required fields differ depending on the individual's country of residence:

* **US Residents** (`residence.country == "US"`) require `ssn`, `phone_country_code`, `phone_number`, and `proof_of_address`.
* **Non-US Residents** require `has_us_bank_account`; if it is `true`, also `bank_statement`.

***

#### 3.1 Full example (US)

A US individual with every field populated (including the optional `identity_document.back` and the `*_other` free-text fields when `other` is selected):

```json
{
  "individual": {
    "first_name": "Ada",
    "last_name": "Smith",
    "email": "ada@acme.us",
    "nationality": "US",
    "birthday": "1990-01-01",
    "residence": {
      "street": "200 Pine St",
      "sub_street": "Apt 5",
      "city": "Richmond",
      "state": "VA",
      "postal_code": "23219",
      "country": "US"
    },
    "occupation": "legislators_and_senior_officials",
    "source_of_wealth": [
      "salaryBusinessIncome",
      "other"
    ],
    "source_of_wealth_other": "Royalty income",
    "purpose_of_use": [
      "payments",
      "other"
    ],
    "purpose_of_use_other": "Treasury management",
    "ssn": "123456789",
    "phone_country_code": "US",
    "phone_number": "+14045550101",
    "proof_of_address": {
      "country": "US",
      "files": [
        "<base64 bytes>"
      ]
    },
    "identity_document": {
      "type": "PASSPORT",
      "country": "US",
      "front": "<base64 bytes>",
      "back": "<base64 bytes>"
    }
  }
}
```

#### 3.2 Minimal Required Payload (US Resident)

```json
{
  "individual": {
    "first_name": "Ada",
    "last_name": "Smith",
    "email": "ada@acme.us",
    "nationality": "US",
    "birthday": "1990-01-01",
    "residence": {
      "street": "200 Pine St",
      "sub_street": "Apt 5",
      "city": "Richmond",
      "state": "VA",
      "postal_code": "23219",
      "country": "US"
    },
    "occupation": "legislators_and_senior_officials",
    "source_of_wealth": [
      "salaryBusinessIncome"
    ],
    "purpose_of_use": [
      "payments"
    ],
    "ssn": "123456789",
    "phone_country_code": "US",
    "phone_number": "+14045550101",
    "proof_of_address": {
      "country": "US",
      "files": [
        "<base64 bytes>"
      ]
    },
    "identity_document": {
      "type": "PASSPORT",
      "country": "US",
      "front": "<base64 bytes>"
    }
  }
}
```

#### 3.3 Payload Example (Non-US Resident)

```json
{
  "individual": {
    "first_name": "Lee",
    "last_name": "Wong",
    "email": "lee@acme.gb",
    "nationality": "GB",
    "birthday": "1992-02-02",
    "residence": {
      "street": "10 High St",
      "sub_street": "Flat 2",
      "city": "London",
      "state": "ENG",
      "postal_code": "SW1A1AA",
      "country": "GB"
    },
    "occupation": "software_and_applications_developers_and_analysts",
    "source_of_wealth": [
      "salaryBusinessIncome"
    ],
    "purpose_of_use": [
      "payments"
    ],
    "has_us_bank_account": true,
    "bank_statement": {
      "country": "GB",
      "files": [
        "<base64 bytes>"
      ]
    },
    "identity_document": {
      "type": "PASSPORT",
      "country": "GB",
      "front": "<base64 bytes>"
    }
  }
}
```

***

### 4. Field Reference

#### `individual` Field Reference

| Field                                                                  | Required    | Notes                                                                                                                                   |
| ---------------------------------------------------------------------- | ----------- | --------------------------------------------------------------------------------------------------------------------------------------- |
| `first_name`, `last_name`                                              | Yes         | Max 50 each.                                                                                                                            |
| `email`                                                                | Yes         |                                                                                                                                         |
| `nationality`                                                          | Yes         | ISO 3166-1 alpha-2; must not be a prohibited country.                                                                                   |
| `birthday`                                                             | Yes         | `YYYY-MM-DD`; must be 18+.                                                                                                              |
| `residence.street` / `sub_street` / `city` / `postal_code` / `country` | Yes         | `country` ISO-2; not a prohibited country.                                                                                              |
| `residence.state`                                                      | Conditional | ISO 3166-2 subdivision `code` from lookup (e.g. `WA`, `ENG`). Omit for countries with no subdivisions.                                  |
| `occupation`                                                           | Yes         | Occupation slug from occupations lookup.                                                                                                |
| `source_of_wealth`                                                     | Yes         | Array, ≥1. Values: `salaryBusinessIncome`, `rentalIncome`, `pensionInheritanceGift`, `investmentIncome`, `other`.                       |
| `source_of_wealth_other`                                               | Conditional | Required if `source_of_wealth` contains `other`.                                                                                        |
| `purpose_of_use`                                                       | Yes         | Array, ≥1. Values: `investmentTrading`, `savingsHolding`, `payments`, `salaryFunds`, `businessPayments`, `remittancesSupport`, `other`. |
| `purpose_of_use_other`                                                 | Conditional | Required if `purpose_of_use` contains `other`.                                                                                          |
| `identity_document.type`                                               | Yes         | `NATIONAL_ID` \| `PASSPORT` \| `DRIVER_LICENCE` \| `RESIDENCE_PERMIT`.                                                                  |
| `identity_document.country`                                            | Yes         | Issuing country, ISO-2.                                                                                                                 |
| `identity_document.front`                                              | Yes         | Base64; jpg/pdf/png; ≤8 MB.                                                                                                            |
| `identity_document.back`                                               | Conditional | Required when the document type needs both sides.                                                                                       |
| `ssn`                                                                  | US only     | 9 digits, no hyphens.                                                                                                                   |
| `phone_country_code`                                                   | US only     | Country code in ISO 3166-1 alpha-2 format (e.g. `US`).                                                                                  |
| `phone_number`                                                         | US only     | Full mobile phone number in E.164 format (e.g. `+14045550101`).                                                                         |
| `proof_of_address`                                                     | US only     | `{ country, files }` — 1–5 base64 files, each ≤8 MB, issued within 90 days.                                                            |
| `has_us_bank_account`                                                  | Non-US only | boolean.                                                                                                                                |
| `bank_statement`                                                       | Non-US      | `{ country, files }` — required if `has_us_bank_account` is `true`; ≤3 base64 files, each ≤8 MB.                                       |

***

### 5. Fixing `action_required`

When status is `action_required`, the onboarding carries `pending_requirements`:

```json
{
  "data": {
    "id": "onb_a1b2c3…",
    "status": "action_required",
    "pending_requirements": [
      {
        "source": "provider_error",
        "description": "individual.identity_document.front is blurred"
      }
    ]
  }
}
```

**Correct the data with `PATCH`.** Send the updated `individual` object. The `individual` section you include **replaces the stored data entirely**.

```sh
curl -X PATCH "https://harbor-sandbox.owlpay.com/api/v1/customers/{{customer_uuid}}/onboarding" \
  -H "X-API-KEY: {{API_KEY}}" \
  -H "Content-Type: application/json" \
  -H "Accept: application/json" \
  -H "Idempotency-Key: {{Idempotency-Key}}" \
  -d '{
    "individual": {
      /* …include ALL individual fields, including corrected files… */
    }
  }'
```

The response is `202`; the onboarding goes back to `processing` and is automatically re-checked.

> 📘 Rules for PATCH
>
> * Allowed **only** while status is `action_required`.
> * The provided section must be **complete and valid on its own**, because it replaces the stored section entirely.
