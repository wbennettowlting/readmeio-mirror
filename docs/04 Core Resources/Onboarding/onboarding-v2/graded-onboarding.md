---
title: Customer Tiers (Graded Onboarding)
content:
  excerpt: >-
    Overview of Graded Onboarding (v2) for individual customers, explaining
    levels, initial submissions, statuses, and updates.
metadata:
  robots: index
---
**Individual Customers Only · API v2**

Harbor's **Graded Onboarding (v2)** provides a frictionless, step-by-step verification flow for individual customers. Instead of requiring a complete and comprehensive KYC submission in a single, all-or-nothing call, Graded Onboarding allows your customers to onboard and transact quickly with minimal initial data, and then sequentially upgrade to higher tiers when needed.

<Callout icon="🚧" theme="warn">
  ### Required Step: Customer Creation First

  Before calling any Graded Onboarding (v2) endpoints, you **must first create an individual customer** object with `type: "individual"` via `POST /api/v1/customers`. This returns the `customer_uuid` used as the path parameter in all onboarding APIs. Please refer to our detailed **[Customer](doc:customer)** guide for creation request and response details.
</Callout>

<Callout icon="💡" theme="default">
  ### Direct Level 2 Entry Allowed

  You can onboard a customer **directly into Level 2** in your initial submission. There is no requirement to start with Level 1 or onboard sequentially; if your customer has all documentation ready, you can submit Level 2 onboarding directly.

  Level 3 cannot be applied for directly, and `kyc_level: 3` is not an accepted value here. Level 3 is reached only by upgrading from an approved Level 2 — see **[Upgrade Tier](doc:upgrade-level)**.
</Callout>

<Cards columns="3">
  <Card title="Submitting Level 1 (US Residents Only)" href="#41-submitting-level-1-us-residents-only" icon="fa-solid fa-flag-usa">
    Jump directly to the Level 1 onboarding JSON payload example for US residents.
  </Card>

  <Card title="Submitting Level 2 (US Residents)" href="#42-submitting-level-2-us-resident" icon="fa-solid fa-user-shield">
    Jump directly to the Level 2 onboarding JSON payload example for US residents.
  </Card>

  <Card title="Submitting Level 2 (Non-US Residents)" href="#43-submitting-level-2-non-us-resident" icon="fa-solid fa-globe">
    Jump directly to the Level 2 onboarding JSON payload example for non-US residents.
  </Card>
</Cards>

***

### 1. V1 vs. V2 Comparison

Before integrating, determine which onboarding flow is right for your application:

| Feature              | Standard Onboarding (v1)                                                         | Graded Onboarding (v2)                                                                   |
| :------------------- | :------------------------------------------------------------------------------- | :--------------------------------------------------------------------------------------- |
| **Target Customers** | Business or Individual Customers                                                 | **Individual Customers Only**                                                            |
| **Submission Model** | All-or-nothing (Full KYC collected upfront)                                      | Incremental (Level 1 → Level 2 → Level 3)                                                |
| **Typical Use Case** | When full, standard individual or corporate verification is required at sign-up. | When you want to "start light" with a low friction sign-up process (Level 1 is US-only). |

<Callout icon="📘" theme="info">
  ### Customer Scheme Isolation

  This is an architectural choice, not a migration path. Customers onboarded on V1 stay on the V1 scheme; customers who start on V2 remain on V2. You can check which onboarding scheme a customer is assigned to by calling:

  `GET /api/v1/customers/{uuid}`

  The response will contain either `"kyc_scheme": "v1"` or `"kyc_scheme": "v2"`.
</Callout>

<Callout icon="📘" theme="info">
  ### Looking for Standard Onboarding (v1)?

  If your individual customer is using the Standard (v1) scheme, or you prefer a single-call full verification flow, please refer to the **[Onboard Individual via API](doc:onboard-individual)** guide instead.
</Callout>

***

### 2. Customer Levels & Limits

The Graded Onboarding flow assigns individual customers to one of three progressive levels (Level 1, Level 2, or Level 3). Each level has its own residential criteria, required document parts, transaction limits, and supported fiat payment methods.

<Callout icon="📘" theme="info">
  ### Detailed Level & Limits Guide

  For a comprehensive breakdown of each onboarding level's requirements, exact transaction limit rules, and restricted payment methods (such as the Level 1 trial allowance and Debit Card limitations), please refer to the **[Customer Level](doc:customer-levels)** guide.
</Callout>

***

### 3. Setup, Endpoints, & Lookups

To avoid duplicate configurations, please refer to our standard integration guidelines:

* **Environments & Authentication:** See **[Authentication](doc:authentication)** for Base URLs, authentication headers (`X-API-KEY`), and the `Idempotency-Key` requirement.
* **Customer Creation:** Before calling any Graded Onboarding endpoint, you must first create an individual customer with `type: "individual"`. Please refer to our detailed **[Customer](doc:customer)** guide for request and response examples for `POST /api/v1/customers`.
* **Reference-Data Lookups:** Some input fields (such as state codes, occupations, and identity document types) only accept dynamically validated options, and guessing them does not work. Call the lookup APIs documented in **[Onboarding Meta APIs](doc:onboarding-lookups)**:

  * `GET /api/v2/customers/individual/occupations`
  * `GET /api/v2/customers/individual/countries/{country}/subdivisions`
  * `GET /api/v2/customers/individual/countries/{country}/identity_documents`

  The identity-document lookup also tells you **which parts each document type requires**. When the response lists `BACK` for the type you intend to send, `identity_document.back` becomes mandatory.

#### Endpoints (v2)

All Graded Onboarding endpoints use the `/api/v2` path prefix:

| Method  | Path                                                     | Scope             | Purpose                                             |
| :------ | :------------------------------------------------------- | :---------------- | :-------------------------------------------------- |
| `POST`  | `/api/v2/customers/{uuid}/individual/onboarding`         | `CUSTOMER_CREATE` | Initial onboarding submission (Level 1 or Level 2). |
| `GET`   | `/api/v2/customers/{uuid}/individual/onboarding`         | `CUSTOMER_READ`   | Poll current onboarding status & details.           |
| `PATCH` | `/api/v2/customers/{uuid}/individual/onboarding`         | `CUSTOMER_CREATE` | Correct and resend a submission that was sent back. |
| `POST`  | `/api/v2/customers/{uuid}/individual/onboarding/upgrade` | `CUSTOMER_CREATE` | Submit a request to upgrade to a higher level.      |

***

### 4. Submitting Initial Onboarding (POST)

Submit the initial onboarding payload for either **Level 1** or **Level 2**.

#### 4.1 Submitting Level 1 (US Residents Only)

```curl
curl -X POST "https://harbor-sandbox.owlpay.com/api/v2/customers/{{customer_uuid}}/individual/onboarding" \
  -H "X-API-KEY: ***" \
  -H "Content-Type: application/json" \
  -H "Accept: application/json" \
  -H "Idempotency-Key: {{Idempotency-Key}}" \
  -d '{
    "kyc_level": 1,
    "nationality": "US",
    "residence": {
      "country": "US",
      "state": "WA"
    }
  }'
```

Response: **202 Accepted** with a status of `processing`.

***

#### 4.2 Submitting Level 2 (US Resident)

```json
{
  "kyc_level": 2,
  "nationality": "US",
  "residence": {
    "country": "US",
    "state": "WA",
    "street": "200 Pine St",
    "sub_street": "Apt 5",
    "city": "Seattle",
    "postal_code": "98101"
  },
  "occupation": "legislators_and_senior_officials",
  "purpose_of_use": ["payments"],
  "ssn": "123456789",
  "phone_country_code": "US",
  "phone_number": "+14045550101",
  "identity_document": {
    "type": "PASSPORT",
    "country": "US",
    "front": "<base64_bytes>"
  }
}
```

**Notes on this payload**

* `phone_country_code` and `phone_number` are required only when the customer record does not already carry a phone number. A phone number is mandatory for US residents, so if neither the request nor the customer record has one, the request is rejected.
* `residence.sub_street` is genuinely required at Level 2 — an empty string does not satisfy it.

***

#### 4.3 Submitting Level 2 (Non-US Resident)

```json
{
  "kyc_level": 2,
  "nationality": "GB",
  "residence": {
    "country": "GB",
    "street": "10 High St",
    "sub_street": "Flat 2",
    "city": "London",
    "postal_code": "SW1A1AA"
  },
  "occupation": "software_and_applications_developers_and_analysts",
  "purpose_of_use": ["payments"],
  "tax_id": "AB123456C",
  "identity_document": {
    "type": "PASSPORT",
    "country": "GB",
    "front": "<base64_bytes>"
  }
}
```

**Notes on this payload**

* `residence.state` must be **omitted** outside the US. It is rejected, not merely optional.
* Send `tax_id`, not `ssn`. Sending `ssn` for a non-US residence is rejected — see **[Customer Level](doc:customer-levels)** for the full tax-identifier rules.

<Callout icon="📘" theme="info">
  ### Accepted Values for Fixed-Choice Fields

  `purpose_of_use` (array, at least one value) — `investmentTrading`, `savingsHolding`, `payments`, `salaryFunds`, `businessPayments`, `remittancesSupport`, `other`. There is no lookup endpoint for this list; the rejection message names the accepted values.

  `identity_document.type` — `NATIONAL_ID`, `PASSPORT`, `DRIVER_LICENCE`, `RESIDENCE_PERMIT`. Note the British spelling of `DRIVER_LICENCE`. The types actually accepted for a given issuing country are narrower — read them from `GET /api/v2/customers/individual/countries/{country}/identity_documents`.

  `occupation` — slugs only, from `GET /api/v2/customers/individual/occupations`. An unrecognised slug is always rejected.
</Callout>

<Callout icon="🚧" theme="warn">
  ### Fields From The V1 Contract Are Rejected, Not Ignored

  The graded contract does not collect `source_of_wealth`, `source_of_wealth_other`, `proof_of_address`, `bank_statement`, or `has_us_bank_account`, and it does not accept the V1 `individual` wrapper or an `applicant` key. Sending any of them returns **422** rather than silently dropping the data — for KYC input, a rejection is safer than a discarded field. Use `kyc_level` for the level number (`level`, `tier`, and `verification_tier` are rejected).
</Callout>

***

### 5. Polling Status & Lifecycle

You can check the onboarding progress by polling the GET endpoint or subscribing to webhooks.

#### 5.1 Onboarding Statuses

The response JSON shape contains a top-level `status`:

```json
{
  "data": {
    "status": "processing",
    "kyc_level": null,
    "next_level": null,
    "transfers_blocked": true,
    "rfi_link": null,
    "pending_requirements": []
  }
}
```

`kyc_level` is the **approved** level, not the level applied for — it stays `null` until a reviewer approves one. `next_level` is the level the upgrade endpoint would accept right now, and is `null` while a review is in flight or an advanced-verification application is already on file.

| Status            | Meaning                                                                                                                                                              | Developer Action                                                                                  |
| :---------------- | :------------------------------------------------------------------------------------------------------------------------------------------------------------------- | :------------------------------------------------------------------------------------------------ |
| `processing`      | Data is being validated.                                                                                                                                             | Poll or wait for webhook.                                                                         |
| `action_required` | Something must be fixed and resent. Covers both a request for more information mid-review **and a correctable rejection**. Check `pending_requirements` for details. | Prompt the user to correct the input and send **PATCH**.                                          |
| `submitted`       | Sent for review; awaiting the verdict.                                                                                                                               | Wait.                                                                                             |
| `verified`        | Selected level is successfully approved.                                                                                                                             | Customer is active at this level.                                                                 |
| `declined`        | **Final.** A verdict was issued on the person. No resubmission of any kind is accepted.                                                                              | Stop. Do not resubmit — every further attempt returns **409** (`declined_is_final`, code `2312`). |
| `failed`          | Systemic or document processing error.                                                                                                                               | Resend with **PATCH** on the same path.                                                           |

<Callout icon="🚧" theme="warn">
  ### `action_required` vs. `declined`

  A review ends in one of two ways, and the distinction decides what your integration should do next:

  * A **correctable rejection** converges to `action_required`. The customer may fix the data and resend with PATCH.
  * A **declinature** converges to `declined`. This is final: the customer record is marked DECLINED, and any subsequent onboarding or upgrade attempt returns **409 Conflict** with code `2312` (`declined_is_final`).

  There is no `rejected` status on this API. If you integrated against an earlier version of this page that listed one, branch on `action_required` and `declined` instead.
</Callout>

***

#### 5.2 Webhooks

We strongly recommend subscribing to the following webhook events to track real-time onboarding state transitions:

* `customer_onboarding.submitted`
* `customer_onboarding.action_required`
* `customer_onboarding.verified`
* `customer_onboarding.declined`
* `customer_onboarding.failed`

The event prefix is `customer_onboarding.` with an underscore — it is deliberately distinct from the `customer.*` events. There is no `customer_onboarding.rejected` event; a correctable rejection is announced as `customer_onboarding.action_required`, and a final one as `customer_onboarding.declined`.

For setup, see **[Webhook Subscription](doc:subscription-apis)**.

***

### 6. Fixing Validation Errors (PATCH)

When a submission has been sent back — status `action_required`, or `failed` after a processing error — correct the data and resubmit using a **PATCH** request.

```curl
curl -X PATCH "https://harbor-sandbox.owlpay.com/api/v2/customers/{{customer_uuid}}/individual/onboarding" \
  -H "X-API-KEY: ***" \
  -H "Content-Type: application/json" \
  -H "Accept: application/json" \
  -H "Idempotency-Key: {{Idempotency-Key}}" \
  -d '{
    "kyc_level": 2,
    "nationality": "US",
    "residence": {
      "country": "US",
      "state": "WA",
      "street": "200 Pine St (Corrected)",
      "sub_street": "Apt 5",
      "city": "Seattle",
      "postal_code": "98101"
    },
    "occupation": "legislators_and_senior_officials",
    "purpose_of_use": ["payments"],
    "ssn": "123456789",
    "identity_document": {
      "type": "PASSPORT",
      "country": "US",
      "front": "<base64_bytes>"
    }
  }'
```

<Callout icon="🚨" theme="default">
  ### Critical Rule for PATCH

  The PATCH payload **replaces the stored onboarding data entirely**. Therefore, you **must resend all fields** (including un-changed fields and base64 files), not just the fields being corrected.

  _Note: You cannot change the target&#x20;_`kyc_level`_&#x20;using PATCH; it is locked to the level of the initial submission. A different level is a different application, not a correction — use the upgrade endpoint instead._
</Callout>

<Callout icon="📘" theme="info">
  ### Once An Onboarding Exists, PATCH Is The Only Way To Resend

  A second **POST** for the same customer is refused. Which conflict you receive depends on where the existing submission stands:

  | Existing status                                                    | POST returns                                        |
  | :----------------------------------------------------------------- | :-------------------------------------------------- |
  | `processing`                                                       | **409** — code `2319` (`submission_processing`)     |
  | `action_required`                                                  | **409** — code `2318` (`action_required_in_flight`) |
  | `submitted`                                                        | **409** — code `2302` (`already_active`)            |
  | `verified` or `failed`, where a verification record already exists | **409** — code `2311` (`must_update_existing`)      |
  | any status, once the customer is declined                          | **409** — code `2312` (`declined_is_final`)         |

  Sending PATCH while the submission is still `processing` or `submitted` is likewise refused, with **409** code `2305` (`update_not_allowed_while_processing`) — wait for the review to resolve first.
</Callout>
