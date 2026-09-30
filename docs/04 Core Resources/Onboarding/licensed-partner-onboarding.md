---
title: Customer Onboarding for Licensed Partners
hidden: true
metadata:
  robots: index
privacy:
  view: anyone_with_link
---
<Callout icon="📘" theme="info">
  ### **Applicable Audience**

  This document is intended specifically for **partners who hold relevant financial licenses** and independently perform KYC/KYB verification for their end-users (KYC-Delegated mode).
</Callout>

### Request Fields

#### Common Required Fields (Applicable to both `individual` and `business` types)

| Field                    | Type                        | Description                                                                                                                                                             |
| ------------------------ | --------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `type`                   | string                      | `individual` or `business`                                                                                                                                              |
| `email`                  | string                      | Customer email; must be unique within the same Application.                                                                                                             |
| `country`                | string (ISO 3166-1 alpha-2) | `individual` = nationality; `business` = country of incorporation/registration.                                                                                         |
| `identification_number`  | string (max 128)            | `individual` = national ID or passport number; `business` = company registration number, tax ID, or business registration number.                                       |
| `address`                | object                      | `individual` = residential address; `business` = business registered address.                                                                                           |
| `address.city`           | string                      | City                                                                                                                                                                    |
| `address.street`         | string                      | Street address                                                                                                                                                          |
| `address.country`        | string (ISO 3166-1 alpha-2) | Country of the address                                                                                                                                                  |
| `address.postal_code`    | string                      | Postal code / ZIP code                                                                                                                                                  |
| `address.state_province` | string                      | State or province. **Mandatory if the country uses a state/province system** (e.g., US, CA, TW, etc.). Requests with restricted or sanctioned regions will be rejected. |

#### Required Fields for `type: individual`

| Field        | Type                | Description                                           |
| ------------ | ------------------- | ----------------------------------------------------- |
| `first_name` | string (max 255)    | First name                                            |
| `last_name`  | string (max 255)    | Last name                                             |
| `birth_date` | date (`YYYY-MM-DD`) | Birth date. Must be in the past (earlier than today). |

#### Required Fields for `type: business`

| Field                | Type                | Description                                                      |
| -------------------- | ------------------- | ---------------------------------------------------------------- |
| `company_name`       | string (max 255)    | Company name                                                     |
| `incorporation_date` | date (`YYYY-MM-DD`) | Date of incorporation. Must be in the past (earlier than today). |

#### Optional Fields

| Field                       | Type                 | Description                                                                                                                                                                                                                                                      |
| --------------------------- | -------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `middle_name`               | string               | Middle name (`individual`).                                                                                                                                                                                                                                      |
| `phone_country_code`        | string (ISO alpha-2) | Phone country code, e.g., `US`                                                                                                                                                                                                                                   |
| `phone_number`              | string (max 20)      | Phone number                                                                                                                                                                                                                                                     |
| `description`               | string (max 500)     | Internal notes, visible only to the Application.                                                                                                                                                                                                                 |
| `intermediary_fi_reference` | string (max 255)     | In a B2B2X or multi-tier Financial Institution (FI) architecture, this marks the name of the downstream financial institution that collected and submitted this end-user's KYC/KYB data. This is forwarded to the compliance service for FI-level risk tracking. |

> Note: Under delegation mode, `agreement_redirect_url` has no effect (the agreement is automatically accepted).

### Request Examples

#### Individual

```json
{
  "type": "individual",
  "first_name": "John",
  "last_name": "Doe",
  "email": "john.doe@example.com",
  "birth_date": "1990-01-01",
  "country": "TW",
  "identification_number": "A123456789",
  "address": {
    "city": "Taipei",
    "street": "No. 1, Fuxing N. Rd.",
    "country": "TW",
    "postal_code": "10491",
    "state_province": "Taipei"
  }
}
```

#### Business

```json
{
  "type": "business",
  "company_name": "OwlPay Inc.",
  "email": "finance@owlpay.com",
  "incorporation_date": "2015-06-20",
  "country": "US",
  "identification_number": "83-1234567",
  "address": {
    "city": "San Francisco",
    "street": "123 Market St.",
    "country": "US",
    "postal_code": "94105",
    "state_province": "California"
  }
}
```

### Response

- Returns a `201 Created` on success. The customer's `status` will be directly marked as `verified` and will include the field `is_kyc_delegated: true`.

### Data Accuracy Requirements (Important Notice for Partners)

The submitted KYC/KYB data **must be genuine and match the verified details of the actual end-user**. These fields are not just stored for documentation; for every subsequent transaction, this data is used to construct the transaction's **originator** (sender) information and sent to our compliance service for AML screening:

- **Individual**: Name, address, date of birth, nationality, and identification number.
- **Business**: Company name, address, date of incorporation, registration country, and company registration/tax number.

Providing inaccurate or incomplete information will lead to transactions being flagged, held, or rejected by compliance screening.