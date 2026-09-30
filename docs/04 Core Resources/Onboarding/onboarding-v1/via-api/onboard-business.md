---
title: Onboard business via API
metadata:
  robots: index
privacy:
  view: public
---
This guide explains how to onboard a business customer (KYB) through a direct API integration. You collect company details, associated persons (directors, UBOs, representatives), and corporate documents directly inside your application, and submit them to Harbor in a single API request.

***

### 1. Prerequisites & Endpoints

Before submitting business onboarding, ensure that:

1. The customer has been created as a **business** customer (`POST /api/v1/customers`).
2. The customer has signed the required platform agreement.
3. The customer is not already KYC/KYB verified.

#### Endpoints

| Method  | Path                                             | Purpose                                                                  |
| ------- | ------------------------------------------------ | ------------------------------------------------------------------------ |
| `POST`  | `/api/v1/customers/{{customer_uuid}}/onboarding` | Submit the full business onboarding payload. Returns **202**.            |
| `GET`   | `/api/v1/customers/{{customer_uuid}}/onboarding` | Read the latest onboarding status.                                       |
| `PATCH` | `/api/v1/customers/{{customer_uuid}}/onboarding` | Correct/replace data while status is `action_required`. Returns **202**. |

***

### 2. Reference-Data Lookups (Meta APIs)

When collecting business onboarding data, some fields (such as industries, job titles, and states) only accept specific values from dynamic lists that change over time. 

Before presenting your UI or submitting the corporate onboarding payload, you must query the corresponding lookup endpoints to get the valid options.

Please refer to the complete **[Onboarding Meta APIs](doc:onboarding-lookups)** guide for details.

***

### 3. Submitting Business Onboarding (POST)

Submit the company profile, all required associated persons, and corporate files (base64-encoded) in a single request.

#### Request

```sh
curl -X POST "https://harbor-sandbox.owlpay.com/api/v1/customers/{{customer_uuid}}/onboarding" \
  -H "X-API-KEY: ***" \
  -H "Content-Type: application/json" \
  -H "Accept: application/json" \
  -H "Idempotency-Key: {{Idempotency-Key}}" \
  -d @business-onboarding.json
```

#### Full Payload Example

```json
{
  "company": {
    "name": "Acme US Inc",
    "business_name": "Acme",
    "registration_number": "REG123456",
    "country": "US",
    "incorporation_date": "2020-01-01",
    "type": "corporation",
    "description": "Software publishers",
    "industry": "software_publishers",
    "register_address": {
      "street": "100 Market St",
      "city": "Richmond",
      "state": "VA",
      "postal_code": "23218"
    },
    "business_address": {
      "street": "100 Market St",
      "city": "Richmond",
      "state": "VA",
      "country": "US",
      "postal_code": "23218"
    },
    "phone_country_code": "US",
    "phone_number": "2025550100",
    "email": "ops@acme.us",
    "url": "https://acme.us",
    "has_bearer_shares": false,
    "purpose_of_registration": "receiptAndPayment",
    "estimated_number_of_transactions": 100,
    "estimated_amount_of_transactions": 50000,
    "country_of_operation": [
      "US",
      "CA"
    ],
    "has_vasp_relationship": false,
    "source_of_funds": "BUSINESS_INCOME",
    "source_of_funds_other": null,
    "transfers_funds_for_clients": true,
    "compliance_screening_explanation": "Funds are settlement proceeds from merchant sales.",
    "tax_id": "123456789"
  },
  "associated_persons": [
    {
      "type": "representative",
      "first_name": "Ada",
      "last_name": "Smith",
      "id_type": "PASSPORT",
      "id_number": "A1234567",
      "birthday": "1990-01-01",
      "gender": "F",
      "nationality": "US",
      "position": "director",
      "residential_address": {
        "street": "200 Pine St",
        "city": "Richmond",
        "state": "VA",
        "country": "US",
        "postal_code": "23219"
      },
      "permanent_address": {
        "street": "200 Pine St",
        "city": "Richmond",
        "state": "VA",
        "country": "US",
        "postal_code": "23219"
      },
      "phone": {
        "country": "US",
        "number": "4155550101"
      },
      "email": "ada@acme.us",
      "identity_files": [
        {
          "file_type": "PASSPORT",
          "part": "FRONT",
          "country": "US",
          "file": "<base64 bytes>"
        }
      ]
    },
    {
      "type": "applicant",
      "first_name": "Ben",
      "last_name": "Jones",
      "id_type": "PASSPORT",
      "id_number": "B7654321",
      "birthday": "1988-03-03",
      "gender": "M",
      "nationality": "US",
      "email": "ben@acme.us",
      "residential_address": {
        "street": "300 Bush St",
        "city": "Richmond",
        "state": "VA",
        "country": "US",
        "postal_code": "23219"
      },
      "identity_files": [
        {
          "file_type": "PASSPORT",
          "part": "FRONT",
          "country": "US",
          "file": "<base64 bytes>"
        }
      ]
    }
  ],
  "company_files": [
    {
      "file_type": "corporate_registration_documents",
      "files": [
        "<base64 bytes>"
      ]
    },
    {
      "file_type": "documents_of_corporate_governance_and_amendment",
      "files": [
        "<base64 bytes>"
      ]
    },
    {
      "file_type": "certificate_of_incumbency",
      "files": [
        "<base64 bytes>"
      ]
    },
    {
      "file_type": "proof_of_residential_address_of_the_ubo",
      "files": [
        "<base64 bytes>"
      ]
    },
    {
      "file_type": "proof_of_business_operating_address",
      "files": [
        "<base64 bytes>"
      ]
    },
    {
      "file_type": "organization_chart",
      "files": [
        "<base64 bytes>"
      ]
    }
  ]
}
```

#### Minimal Required-Fields Payload

```json
{
  "company": {
    "name": "Acme US Inc",
    "business_name": "Acme",
    "registration_number": "REG123456",
    "country": "US",
    "incorporation_date": "2020-01-01",
    "type": "corporation",
    "description": "Software publishers",
    "industry": "software_publishers",
    "register_address": {
      "street": "100 Main St",
      "city": "Richmond",
      "state": "VA",
      "postal_code": "23218"
    },
    "business_address": {
      "street": "100 Main St",
      "city": "Richmond",
      "state": "VA",
      "country": "US",
      "postal_code": "23218"
    },
    "phone_country_code": "US",
    "phone_number": "2025550100",
    "email": "ops@acme.us",
    "url": "https://acme.us",
    "has_bearer_shares": false,
    "purpose_of_registration": "receiptAndPayment",
    "estimated_number_of_transactions": 100,
    "estimated_amount_of_transactions": 50000,
    "country_of_operation": [
      "US"
    ],
    "has_vasp_relationship": false,
    "source_of_funds": "BUSINESS_INCOME",
    "transfers_funds_for_clients": false,
    "tax_id": "123456789"
  },
  "associated_persons": [
    {
      "type": "representative",
      "first_name": "Ada",
      "last_name": "Smith",
      "id_type": "PASSPORT",
      "id_number": "A1234567",
      "birthday": "1990-01-01",
      "gender": "F",
      "nationality": "US",
      "position": "director",
      "residential_address": {
        "street": "200 Pine St",
        "city": "Richmond",
        "state": "VA",
        "country": "US",
        "postal_code": "23219"
      },
      "permanent_address": {
        "street": "200 Pine St",
        "city": "Richmond",
        "state": "VA",
        "country": "US",
        "postal_code": "23219"
      },
      "phone": {
        "country": "US",
        "number": "4045550101"
      },
      "identity_files": [
        {
          "file_type": "PASSPORT",
          "part": "FRONT",
          "country": "US",
          "file": "<base64 bytes>"
        }
      ]
    },
    {
      "type": "applicant",
      "first_name": "Ben",
      "last_name": "Jones",
      "id_type": "PASSPORT",
      "id_number": "B7654321",
      "birthday": "1988-03-03",
      "gender": "M",
      "nationality": "US",
      "email": "ben@acme.us",
      "residential_address": {
        "street": "300 Bush St",
        "city": "Richmond",
        "state": "VA",
        "country": "US",
        "postal_code": "23219"
      },
      "identity_files": [
        {
          "file_type": "PASSPORT",
          "part": "FRONT",
          "country": "US",
          "file": "<base64 bytes>"
        }
      ]
    }
  ],
  "company_files": [
    {
      "file_type": "corporate_registration_documents",
      "files": [
        "<base64 bytes>"
      ]
    },
    {
      "file_type": "documents_of_corporate_governance_and_amendment",
      "files": [
        "<base64 bytes>"
      ]
    },
    {
      "file_type": "certificate_of_incumbency",
      "files": [
        "<base64 bytes>"
      ]
    },
    {
      "file_type": "proof_of_residential_address_of_the_ubo",
      "files": [
        "<base64 bytes>"
      ]
    },
    {
      "file_type": "proof_of_business_operating_address",
      "files": [
        "<base64 bytes>"
      ]
    },
    {
      "file_type": "organization_chart",
      "files": [
        "<base64 bytes>"
      ]
    }
  ]
}
```

#### Response (202 Accepted)

```json
{
  "data": {
    "id": "onb_a1b2c3…",
    "customer_id": "cus_1234567890",
    "status": "processing",
    "submitted_at": null,
    "created_at": "2026-06-13T09:00:00.000000Z",
    "updated_at": "2026-06-13T09:00:00.000000Z"
  }
}
```

***

### 4. Field Reference

#### 4.1 `company` (all required unless noted)

| Field                              | Type / format        | Notes                                                                                            |
| ---------------------------------- | -------------------- | ------------------------------------------------------------------------------------------------ |
| `name`                             | string ≤255          | Registered legal name.                                                                           |
| `business_name`                    | string ≤255          | Trading / DBA name.                                                                              |
| `registration_number`              | string ≤255          |                                                                                                  |
| `country`                          | ISO 3166-1 alpha-2   | Country of incorporation.                                                                        |
| `incorporation_date`               | `YYYY-MM-DD`         |                                                                                                  |
| `type`                             | enum                 | See **Company types** below.                                                                     |
| `description`                      | string ≤1000         | Business description.                                                                            |
| `industry`                         | string               | Industry classification. A country-specific slug — see **Industry classification** lookup.       |
| `register_address`                 | object               | `{ street, city, state?, postal_code }`. `state` subdivision `code` from the lookup (e.g. `VA`). |
| `business_address`                 | object               | `{ street, city, state?, country, postal_code }`. `state` is a subdivision `code`.               |
| `phone_country_code`               | string (ISO-2) \| null | Optional. ISO 3166-1 alpha-2 country code of the company phone. Defaults to customer's `phone_country_code`. |
| `phone_number`                     | string \| null       | Optional. Company phone number. Defaults to customer's `phone_number`.                           |
| `email`                            | email ≤255           |                                                                                                  |
| `url`                              | URL ≤255             |                                                                                                  |
| `has_bearer_shares`                | boolean              | Whether the company has bearer shares.                                                           |
| `purpose_of_registration`          | enum                 | `onlyReceiving` \| `receiptAndPayment`.                                                          |
| `estimated_number_of_transactions` | integer ≥0           |                                                                                                  |
| `estimated_amount_of_transactions` | integer ≥0           |                                                                                                  |
| `country_of_operation`             | array of ISO-2       | At least one.                                                                                    |
| `source_of_funds`                  | enum                 | See **Source of funds** below.                                                                   |
| `source_of_funds_other`            | string ≤255 \| null  | **Required when `source_of_funds` is `OTHER`.**                                                  |
| `transfers_funds_for_clients`      | boolean              | Do you transfer funds on behalf of your clients?                                                 |
| `compliance_screening_explanation` | string ≤1000 \| null | Optional.                                                                                        |
| `tax_id`                           | string ≤255          | Company tax / registration id (VAT / EIN, etc.).                                                 |

* **Company types:** `soleProprietorship`, `partnerships`, `limitedLiabilityCompany`, `corporation`, `corporationPublic`, `cooperative`, `nonProfitOrganization`, `stateOwnedCompany`, `trust`, `other`.
* **Source of funds:** `BUSINESS_INCOME`, `CAPITAL_INJECTION`, `BANK_LOAN_OR_OTHER_BORROWINGS`, `INVESTMENT_INCOME`, `GOVERNMENT_GRANT`, `OTHER`.

***

#### 4.2 `associated_persons[]`

Provide associated persons by role:

| `type`           | Role                            | Requirement                                                                                                                                        |
| ---------------- | ------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------- |
| `representative` | Legal representative            | **Required — exactly one.**                                                                                                                        |
| `applicant`      | Applicant                       | **Required — at least one.**                                                                                                                       |
| `equity_holder`  | Ultimate beneficial owner (UBO) | Optional. Anyone owning **>20%** is a UBO; if no one owns >20%, provide the individual who actually controls the company.                          |
| `controller`     | Senior managing official        | Optional. Required when the company is **publicly listed, a non-profit, a government body, or state-owned**, or when **no shareholder owns >20%**. |

**Common fields (all person types):**

| Field                     | Type / format | Notes                                                                              |
| ------------------------- | ------------- | ---------------------------------------------------------------------------------- |
| `type`                    | enum          | `representative` \| `applicant` \| `equity_holder` \| `controller`.                |
| `first_name`, `last_name` | string ≤255   |                                                                                    |
| `id_type`                 | enum          | `NATIONAL_ID` \| `PASSPORT` \| `DRIVER_LICENCE` \| `RESIDENCE_PERMIT`.             |
| `id_number`               | string ≤255   |                                                                                    |
| `birthday`                | `YYYY-MM-DD`  |                                                                                    |
| `gender`                  | `M` \| `F`    |                                                                                    |
| `nationality`             | ISO-2         |                                                                                    |
| `residential_address`     | object        | `{ street, city, state?, country, postal_code }`. `state` is a subdivision `code`. |
| `identity_files`          | array         | At least one (see **Files** below).                                                |

**Conditionally required, by type:**

| Type             | Additionally required                                                                                                                  |
| ---------------- | -------------------------------------------------------------------------------------------------------------------------------------- |
| `representative` | `position`, `phone`, `permanent_address`                                                                                               |
| `applicant`      | `email`                                                                                                                                |
| `controller`     | `position`                                                                                                                             |
| `equity_holder`  | `equity`, `has_control`, `is_signer`, `relationship_established_at`, `tax_jurisdiction_country`, `email`, `phone`, `permanent_address` |

**Optional / conditional field shapes:**

| Field                         | Type / format                                                 |
| ----------------------------- | ------------------------------------------------------------- |
| `position`                    | A job-title `value` from the job-titles lookup.               |
| `email`                       | email ≤255                                                    |
| `phone`                       | `{ country, number }` (`country` ISO-2, `number` digits only) |
| `permanent_address`           | `{ street, city, state?, country, postal_code }`              |
| `equity`                      | number 0–100 (percent)                                        |
| `has_control`                 | boolean                                                       |
| `is_signer`                   | boolean                                                       |
| `relationship_established_at` | `YYYY-MM-DD`                                                  |
| `tax_jurisdiction_country`    | ISO-2                                                         |

***

#### 4.3 Files

##### Identity files (`associated_persons[].identity_files[]`)

| Field       | Type              | Notes                                                                  |
| ----------- | ----------------- | ---------------------------------------------------------------------- |
| `file_type` | enum              | `NATIONAL_ID` \| `PASSPORT` \| `DRIVER_LICENCE` \| `RESIDENCE_PERMIT`. |
| `part`      | `FRONT` \| `BACK` |                                                                        |
| `country`   | ISO-2             | Issuing country.                                                       |
| `file`      | base64 string     | The raw document bytes, base64-encoded.                                |

##### Company files (`company_files[]`)

| Field       | Type                    | Notes                                                         |
| ----------- | ----------------------- | ------------------------------------------------------------- |
| `file_type` | enum                    | One entry per `file_type` (must be distinct). See list below. |
| `files`     | array of base64 strings | One or more documents for that type.                          |

**Company file types** (The first 6 are **required on every submission**):

| `file_type`                                       | Required | Meaning                                    |
| ------------------------------------------------- | -------- | ------------------------------------------ |
| `corporate_registration_documents`                | **Yes**  | Company registration documents             |
| `documents_of_corporate_governance_and_amendment` | **Yes**  | Corporate governance & amendment documents |
| `certificate_of_incumbency`                       | **Yes**  | Certificate of incumbency                  |
| `proof_of_residential_address_of_the_ubo`         | **Yes**  | Proof of residential address of the UBO    |
| `proof_of_business_operating_address`             | **Yes**  | Proof of business operating address        |
| `organization_chart`                              | **Yes**  | Organization chart                         |
| `initial_capital`                                 | No       | Optional                                   |
| `income_source`                                   | No       | Optional                                   |
| `business_license`                                | No       | Optional                                   |
| `executive_documents`                             | No       | Optional                                   |
| `financial_documents`                             | No       | Optional                                   |
| `compliance_documents`                            | No       | Optional                                   |
| `other_documentation`                             | No       | Optional                                   |

##### Size limits

* **Per file:** ≤ 8 MB (decoded).
* **Whole request:** ≤ 50 MB (decoded total across all files).

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
        "description": "companyUrl is invalid"
      }
    ]
  }
}
```

**Correct the data with `PATCH`.** You may replace **any** section — `company`, `associated_persons`, and/or `company_files` — not just the flagged fields. Each section you include **replaces that section wholesale**; sections you omit are left untouched.

```sh
curl -X PATCH "https://harbor-sandbox.owlpay.com/api/v1/customers/{{customer_uuid}}/onboarding" \
  -H "X-API-KEY: {{API_KEY}}" \
  -H "Content-Type: application/json" \
  -H "Accept: application/json" \
  -H "Idempotency-Key: {{Idempotency-Key}}" \
  -d '{
    "company": {
      "url": "https://www.acme.tw"
      /* …include ALL other company fields too — the section is replaced wholesale… */
    }
  }'
```

The response is `202`; the onboarding goes back to `processing` and is automatically re-checked.

> 📘 Rules for PATCH
>
> * Allowed **only** while status is `action_required`.
> * A provided section must be **complete and valid on its own**, because it replaces the stored section entirely.
> * To change an associated person, include the full `associated_persons` array in the order you want; persons you drop are removed.
