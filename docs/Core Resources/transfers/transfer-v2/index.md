---
title: Transfer (V2)
metadata:
  robots: index
x-privacy:
  view: public
---
### Overview (V2) Transfer)

The **(V2) Transfer API** enables transfers where the sender funds the transfer in one currency (e.g., **USDC**) and the recipient receives the payout in a **local fiat currency** (e.g., **BRL, MXN, HKD**), depending on the available corridor and payment method.

_**If you would like to link an existing bank account in order to fund a transfer ( ACH PULL) please refer to <Anchor label="Linking a Bank Account." target="_blank" href="/docs/copy-of-customer">Linking a Bank Account.</Anchor>**_

To initiate a (V2) transfer, whether on-ramping or off-ramping, you must first request a quote via the **Quote API**:

1. **Call Quote API** to obtain:
   * The **exchange rate (pricing)** and any applicable fees
   * A **quote_id** that represents the locked pricing context (subject to the quote validity window)

2. **Use the returned `quote_id`** to:
   * Create a (V2) transfer request
   * Retrieve the required **JSON Schema (Draft 2020-12)** for the transfer payload, so your integration can validate the request structure and required fields before submission

This quote-first flow ensures that:

* FX pricing is explicit and consistent for both parties
* Transfer requests are validated against the correct corridor-specific requirements
* Regulatory / payout-instrument requirements are applied accurately based on route configuration

***

#### Typical Flow

1. **Create Quote** → receive `quote_id`
2. **Get Requirements Schema** (by `quote_id`) → receive JSON Schema (Draft 2020-12)
3. **Create Transfer (V2)** using `quote_id` + payload validated by the schema
