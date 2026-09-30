---
title: Onboarding v2 (Graded)
metadata:
  robots: index
privacy:
  view: public
slug: onboarding-v2
---
**Graded Onboarding (v2)** verifies **individual customers** incrementally through progressive levels, each with its own limits, instead of requiring a complete KYC submission up front. It uses the `/api/v2/customers/{customer_uuid}/individual/onboarding` endpoints.

<Cards>
  <Card title="Customer Tiers (Graded Onboarding)" href="/docs/graded-onboarding" icon="fa-solid fa-layer-group">
    **Overview & Integration**: How graded onboarding works, how it compares to v1, and how to submit, poll, and correct an onboarding.
  </Card>
  <Card title="Customer Level" href="/docs/customer-levels" icon="fa-solid fa-signal">
    **Levels & Limits**: What each level allows and how limits apply to transfers.
  </Card>
  <Card title="Upgrade Tier" href="/docs/upgrade-level" icon="fa-solid fa-arrow-up">
    **Elevating Customer Levels**: Request a higher level for an existing customer.
  </Card>
</Cards>

Onboarding a business, or verifying an individual in a single call? See [Onboarding v1 (Standard)](doc:onboarding-v1).
