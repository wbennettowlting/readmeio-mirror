---
title: Onboarding (V1)
metadata:
  robots: index
privacy:
  view: public
slug: onboarding-v1
---
**Standard Onboarding (V1)** verifies a customer in a single, complete submission. It supports both **business (KYB)** and **individual (KYC)** customers, and uses the `/api/v1/customers/{customer_uuid}/onboarding` endpoints.

Choose how you want to collect your customers' information:

<Cards>
  <Card title="Onboard via Hosted Link" href="/docs/hosted-link-onboarding" icon="fa-link">
    **No-Code / Low-Code Integration**: Redirect customers to Harbor's secure hosted forms to sign agreements, fill out profiles, upload documents, and complete biometric checks.
  </Card>
  <Card title="Onboard via API" href="/docs/via-api" icon="fa-code">
    **Direct API Integration**: Collect customer profiles and documents in your own application and submit them to Harbor in a single API payload.
  </Card>
</Cards>

Looking for progressive, tier-based verification for individual customers? See [Onboarding (V2) (Graded)](doc:onboarding-v2).
