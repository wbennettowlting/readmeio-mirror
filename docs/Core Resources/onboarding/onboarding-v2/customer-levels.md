---
title: Customer Levels
excerpt: >-
  Detailed guide explaining the three onboarding levels (tiers), data collection
  requirements, and transaction limits for individual customers.
x-content:
  excerpt: >-
    Detailed guide explaining the three onboarding levels (tiers), data
    collection requirements, and transaction limits for individual customers.
metadata:
  robots: index
---
**Individual Customers Only · API (V2)**

Individual customers on the Graded Onboarding (V2) scheme can progress through three distinct tiers. Each level unlocks greater capabilities, has specific data collection requirements, and defines the transaction limits and allowed payment methods.

***

### 1. The Three Graded Levels (Tiers)

Each onboarding level has specific data collection and compliance requirements:

#### 🟢 Level 1: Basic Verification (US Residents Only)

*   **Prerequisites & Target:** US residents only (`residence.country == "US"`).
*   **Collected Fields:** Nationality, Country, and State of residence.
*   **Purpose:** Fast-track onboarding. Offers a rapid path to a verified, transacting state without requiring physical documents or SSN/Tax ID upfront.
*   **Key Restrictions & Limits:**
    *   **US Residents Only:** Submitting a non-US residence for Level 1 is forbidden. The request is rejected with **422 Unprocessable Entity** — `error_type: general.validation_error`, code `2005` — carrying a field error on `residence.country`. Apply for `kyc_level: 2` instead for a residence outside the US.
    *   **Nothing Beyond The Three Fields:** Every Level 2 field (street, city, postal code, SSN/Tax ID, occupation, purpose of use, phone, identity document) is **rejected** at Level 1, not ignored.

---

#### 🔵 Level 2: Full Individual KYC

*   **Prerequisites & Target:** US or Non-US residents.
*   **Collected Fields:** Full residential address, Occupation, Purpose of use, Phone number, Identity document (Base64 front, plus back where the document type requires it), and Tax Identifier (SSN for US, local Tax ID for non-US).
*   **Purpose:** Complete standard individual verification.
*   **Key Restrictions & Limits:**
    *   **Tax Identifier Rules:**
        *   **US Residents:** Must provide a 9-digit `ssn`. The `tax_id` field is **forbidden** for US residents (returns 422).
        *   **Non-US Residents:** Must provide a local `tax_id`. The `ssn` field is **forbidden** for non-US residents (returns 422).
    *   **State Constraints:** `residence.state` subdivision code is required for US residents but **forbidden** for non-US residents (returns 422).
    *   **Phone Number:** A phone number is required for US residents. Send `phone_country_code` and `phone_number` only when the customer record does not already carry one; if neither source has a phone number, the request is rejected.
    *   **Identity Document Back Page:** `identity_document.back` becomes mandatory when the document lookup lists `BACK` for the chosen type. Read it from `GET /api/v2/customers/individual/countries/{country}/identity_documents`.
    *   **Transfers Blocked during Upgrade:** When upgrading a verified Level 1 customer to Level 2, transactions are temporarily **blocked** (`transfers_blocked: true`) until the Level 2 onboarding is fully reviewed and verified.

---

#### 🟣 Level 3: Advanced Verification

*   **Prerequisites & Target:** Customer must already be verified at Level 2.
*   **Collected Fields:** Written reason/justification for limits elevation (`reason`).
*   **Purpose:** Manual case-by-case compliance review to grant higher transaction volume thresholds.
*   **Key Restrictions & Limits:**
    *   **Sequential Elevation Only:** The upgrade endpoint always targets the next level above the customer's **approved** level — you never name a target level yourself, and `GET .../individual/onboarding` reports it as `next_level`. A customer approved at Level 1 can therefore only reach Level 2 first. Calling the upgrade endpoint with **no approved level at all**, or when the customer is **already at Level 3**, returns **422** with code `2310` (`level_not_eligible_for_elevation`).
    *   **Cooldown Period:** Only one Level 3 application is accepted per cooldown window (**7 days** by default), **regardless of how the previous one turned out** — a second application inside the window returns **409** with code `2313` (`elevation_already_requested`). The error message names the date from which a new application can be sent.

***

### 2. Limits & Payment Methods by Level

Onboarding tiers dynamically control both the allowed **fiat payment methods** and the **transfer volume limits** (for both on-ramp and off-ramp transactions, calculated independently).

| Onboarding Level | Per-Transaction | Daily Limit | Monthly Limit | Lifetime Limit | Allowed Payment Methods |
| :--- | :--- | :--- | :--- | :--- | :--- |
| **Level 1** | $2,999 | $2,999 | $2,999 | $2,999 | `DEBIT_CARD` only |
| **Level 2** | $9,999 | $9,999 | $299,970 | No Ceiling | Any offered method (e.g., ACH, Wire, RTP, Debit) |
| **Level 3** | No Ceiling | $50,000 (default) | $1,000,000 (default) | No Ceiling | Any offered method (e.g., ACH, Wire, RTP, Debit) |

> ℹ️ *Note on Level 3 Limits*
> 
> The Level 3 figures above are **starting defaults**. Level 3 is the one level whose limits compliance adjusts per customer, case by case, based on the customer's financial profile, transaction history, and platform requirements.
> 
> Level 3 is **not unlimited**. A case-by-case daily limit is set within **$1,000 – $250,000**, and a monthly limit within **$10,000 – $5,000,000**.

> 📘 On-Ramp and Off-Ramp Accumulate Separately
> 
> The daily, monthly and lifetime counters are tracked independently for on-ramp and off-ramp. A Level 2 customer may therefore move $9,999 on-ramp **and** $9,999 off-ramp on the same day.

#### 2.1 Important Limit Rules & Behaviors

##### ➔ Level 1 "Same Numbers" Rule (Trial Allowance)
For Level 1 customers, the limit of **$2,999** is identical across all periods (Per-Transaction, Daily, Monthly, and Lifetime). This functions as a single **"trial allowance"**. Whichever limit is hit first halts further transactions. Since the lifetime limit is also $2,999, a Level 1 customer can only transact up to $2,999 in total before being forced to upgrade to Level 2.

##### ➔ Level 1 Debit Card Restriction
Level 1 customers are strictly restricted to the `DEBIT_CARD` payment method. Other high-limit methods (like `ACH_PULL`, `WIRE`, or `RTP`) are omitted from the response of **Create Quote** (`POST /api/v2/transfers/quotes`) for Level 1 customers, so your checkout never offers a rail the customer's level cannot use.

Two details worth knowing when you integrate this:

*   **The filter needs `on_behalf_of`.** It is applied only when the quote request identifies the customer. A quote without `on_behalf_of`, a KYC-delegated customer, or a customer with no approved level yet is not filtered.
*   **It does not apply to the settings endpoints.** `GET /api/v2/transfers/deposit_settings` and `GET /api/v2/transfers/withdrawal_settings` describe what your **application** supports, take no customer parameter, and are therefore not narrowed by any customer's level.

When this level gate removes every remaining quote, the response says so explicitly — naming the payment methods the customer's level may be quoted — rather than reporting a generic "no quotes available" for the pair or the amount.

##### ➔ Restricted US Subdivisions
Regardless of level, a residence in a subdivision Harbor does not serve is rejected with a **422** field error on `residence.state`, checked before the submission is forwarded. Always drive your state picker from `GET /api/v2/customers/individual/countries/{country}/subdivisions` rather than the full ISO list: the endpoint publishes only the subdivisions that are both licensed and served, so a value it returns will not be rejected on this ground.
