---
title: Upgrade Level
excerpt: >-
  How to upgrade verified individual customers to higher Graded Onboarding
  levels (Level 2 or Level 3).
x-content:
  excerpt: >-
    How to upgrade verified individual customers to higher Graded Onboarding
    levels (Level 2 or Level 3).
metadata:
  robots: index
---
**Individual Customers Only · API (V2)**

Once an individual customer on the Graded Onboarding (V2) scheme has been successfully onboarded and verified at their initial level, they can request an upgrade to a higher level (Level 2 or Level 3) to increase their transaction limits or unlock advanced capabilities.

***

### 1. Upgrade Endpoint

All level-elevation requests must be sent to the onboarding upgrade endpoint:

`POST /api/v2/customers/{uuid}/individual/onboarding/upgrade`

#### Required Headers
Your request must include standard API headers, including `X-API-KEY`, `Content-Type: application/json`, and an `Idempotency-Key` to prevent duplicate submissions.

> 📘 One Endpoint, No Target Level
> 
> There is a single upgrade route, and you never name the level you are applying for. The endpoint always targets the level directly above the customer's **approved** level, and the body it expects follows from that:
> 
> | Approved level | Target | Body |
> | :--- | :--- | :--- |
> | 1 | 2 | The Level 2 data delta (see §2) |
> | 2 | 3 | A `reason` string, nothing else (see §3) |
> 
> Read the target from `next_level` on `GET /api/v2/customers/{uuid}/individual/onboarding` before you build the payload, instead of inferring it. `next_level` is `null` whenever the route would refuse right now — a review still in flight, or an advanced-verification application already on file.

***

### 2. Upgrading from Level 1 to Level 2

When upgrading a customer from Level 1 (Basic US-resident check) to Level 2 (Full Individual KYC), you submit only the incremental data fields needed to satisfy Level 2 requirements.

#### 🚨 Critical Validation Rule
Because Level 1 already established the customer's country and state of residence (which must be `US` and a valid US state), **you must not re-submit `residence.country` or `residence.state`** in the upgrade payload. They are inherited from the approved Level 1 record.

Sending either one returns **422 Unprocessable Entity** as a standard validation failure — `error_type: general.validation_error`, code `2005` — with the offending field named under `errors`:

```json
{
  "message": "The residence.country field is not collected on the upgrade route because it is inherited from the approved level 1 record. Remove it.",
  "code": 2005,
  "error_type": "general.validation_error",
  "errors": {
    "residence.country": [
      "The residence.country field is not collected on the upgrade route because it is inherited from the approved level 1 record. Remove it."
    ]
  }
}
```

`kyc_level` and `reason` are likewise rejected here: the target level follows from the approved level, so there is nothing to declare.

#### Example Payload: Level 1 ➔ Level 2

```json
{
  "residence": {
    "street": "200 Pine St",
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
}
```

`residence.country` and `residence.state` are deliberately absent — see the rule above. Because a Level 1 customer is always a US resident, this upgrade is always the US variant: `ssn` is required and `tax_id` is rejected. Add `phone_country_code` and `phone_number` only if the customer record does not already carry a phone number.

Response: **202 Accepted** with a status of `processing`.

> 🚧 Transacting Pauses During This Upgrade
> 
> Moving from Level 1 to Level 2 pauses transacting: the response reports `transfers_blocked: true`, and it stays true until the Level 2 review resolves. Plan for this in your UI — the customer was able to transact a moment earlier.

***

### 3. Upgrading from Level 2 to Level 3

Level 3 represents advanced verification for high-limit users. Unlike Level 2, Level 3 does not collect standard structured KYC fields. Instead, it triggers a manual, case-by-case compliance review.

To request elevation from Level 2 to Level 3, submit a JSON payload containing only a descriptive reason for the request (max 500 characters):

#### Example Payload: Level 2 ➔ Level 3

```json
{
  "reason": "Requesting a higher monthly transaction limit for increased company payroll and contractor payout volume."
}
```

Every other field — `kyc_level`, `nationality`, `residence`, and the whole Level 2 set — is rejected here.

Response: **202 Accepted**. The request is recorded and forwarded to Harbor's compliance team, who contact the customer directly:

```json
{
  "data": {
    "status": "requested",
    "requested_at": "2026-09-09T07:40:00+00:00",
    "kyc_level": 2,
    "next_level": null,
    "transfers_blocked": false,
    "rfi_link": null,
    "pending_requirements": []
  }
}
```

Note how this differs from the Level 1 ➔ 2 case:

*   `status` is `requested` — the application is on file, not under automated processing.
*   `requested_at` records when it was filed, and is echoed in the conflict message if a second application arrives too soon.
*   `kyc_level` stays `2`. The level does not change until compliance grants it.
*   `next_level` is `null` while the application stands, because the route will refuse a second one.
*   **`transfers_blocked` stays `false`.** Unlike a Level 1 ➔ 2 upgrade, applying for Level 3 does **not** pause transacting — the customer keeps trading at Level 2 throughout the review.

***

### 4. Specific Error Codes & Conflicts

Upgrade requests may return `409 Conflict` (due to the customer's current onboarding state) or `422` validation errors. The ones specific to this endpoint:

| Status | Code | Situation |
| :--- | :--- | :--- |
| **422** | `2310` | `level_not_eligible_for_elevation` — the customer has **no approved level yet**, or is **already at Level 3**. There is nothing to elevate. |
| **422** | `2005` | `general.validation_error` — the body did not match the target level: inherited fields re-sent, a required Level 2 field missing, or a field the target level does not collect. See `errors` for the fields. |
| **409** | `2313` | `elevation_already_requested` — a Level 3 application was already filed inside the cooldown window (**7 days** by default), whatever its outcome. The message names the date a new one can be sent. |
| **409** | `2312` | `declined_is_final` — the customer has been declined. No upgrade, and no onboarding, is accepted afterwards. |
| **409** | `2302` / `2318` / `2319` | A submission is still open for this customer. Let the current review resolve before applying to elevate. |

> 📘 Consolidated Error Code Reference
> 
> For the full list of numeric error codes and their definitions across the whole API, please refer to the **[Error Codes](doc:error-codes)** guide.
