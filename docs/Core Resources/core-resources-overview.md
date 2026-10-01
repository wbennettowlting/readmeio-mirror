---
title: Overview
excerpt: The main building blocks of a Harbor integration and how they fit together.
hidden: false
---
Core Resources covers the main objects of the Harbor API in depth: their fields, the API operations available for each, and how they connect. If you're new to Harbor, start with the [Quickstart](/docs/quickstart) first.

A typical integration uses these resources in order:

1. **Create a customer** to represent each user in your system.
2. **Onboard the customer** so Harbor can verify their identity (KYC) or business (KYB).
3. **Create transfers** to move funds once the customer is verified.
4. **Subscribe to webhooks** to get notified as customers, onboarding, and transfers change status.

<Cards columns={2}>
  <Card title="Customers" href="/docs/customers" icon="fa-solid fa-user">
    Create and manage customer records, and get the agreement and KYC links to share with each customer.
  </Card>

  <Card title="Onboarding" href="/docs/onboarding" icon="fa-solid fa-id-card">
    Verify customers through a hosted link or directly through the API, using Onboarding (V1) or graded Onboarding (V2).
  </Card>

  <Card title="Transfers" href="/docs/transfers" icon="fa-solid fa-arrow-right-arrow-left">
    Get a quote, then create on-ramp, off-ramp, and on-chain transfers. Covers recipients, RFIs, settlement, and expiration.
  </Card>

  <Card title="Wallets" href="/docs/wallets" icon="fa-solid fa-wallet">
    Manage your application's wallets and blockchain addresses. Available only to US-based applications.
  </Card>

  <Card title="Webhooks" href="/docs/webhooks" icon="fa-solid fa-bell">
    Subscribe to events and receive signed notifications when customers, onboarding, or transfers change.
  </Card>
</Cards>
