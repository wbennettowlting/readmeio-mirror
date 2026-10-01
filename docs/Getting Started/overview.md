---
title: Overview
x-content:
  excerpt: Welcome to the Harbor Integration Guide!
metadata:
  robots: index
x-privacy:
  view: public
---
This document is designed specifically for **developers**, providing a comprehensive guide to integrate with our API services. It includes instructions for submitting customer information, managing transaction statuses, and more.




Contact us to get your API KEY (SANDBOX / PRODUCTION): [contact-us@owlting.com](mailto:contact-us@owlting.com)

<Callout icon="📘" theme="info">

**API Environments:**
* **Sandbox:** `https://harbor-sandbox.owlpay.com`
* **Production:** `https://harbor.owlpay.com`

</Callout>




### Customer Management

<Cards>
  <Card title="Customer Onboarding" href="/docs/onboarding" icon="fa-user-check">
    Onboard and verify your customers (individual or business profiles) via hosted links or direct API integration.
  </Card>
  <Card title="Customer" href="/docs/customers" icon="fa-user">
    Manage customer data and their verification statuses.
  </Card>
  <Card title="Link a Bank Account" href="/docs/link-bank-account" icon="fa-building-columns">
    Connect and link U.S. bank accounts for ACH Pull transactions.
  </Card>
  <Card title="Link a Debit Card" href="/docs/link-debit-card" icon="fa-credit-card">
    Bind a debit card to purchase cryptocurrency directly.
  </Card>
  <Card title="Customer's Deposit Account" href="/docs/deposit-accounts" icon="fa-wallet">
    Provision dedicated bank accounts for customers to auto-on-ramp fiat to crypto.
  </Card>
</Cards>

### Transfer Types

<Cards>
  <Card title="Get a Quote" href="/docs/quotes" icon="fa-calculator">
    Get exchange rates, fees, and settlement amounts before creating a transfer.
  </Card>
  <Card title="Fiat → Stablecoin" href="/docs/on-ramp" icon="fa-arrow-right">
    On-ramp: convert fiat currency to stablecoin.
  </Card>

  <Card title="Stablecoin → Fiat" href="/docs/off-ramp" icon="fa-arrow-left">
    Off-ramp: convert stablecoin back to fiat currency. Includes transfers with local currency.
  </Card>

  <Card title="Stablecoin → Stablecoin" href="/docs/on-chain" icon="fa-repeat">
    On-chain swap between stablecoins.
  </Card>
</Cards>

### Reference

<Cards>
  <Card title="Supported Blockchains & Stablecoins" href="/docs/stablecoins-and-blockchains" icon="fa-cubes">
    A complete list of blockchain networks and stablecoins supported by Harbor.
  </Card>

  <Card title="Wallet API" href="/docs/wallets" icon="fa-wallet">
    Create and manage blockchain wallets for receiving and sending crypto assets.
  </Card>
  <Card title="Webhook Subscriptions" href="/docs/webhooks" icon="fa-bell">
    Subscribe to real-time event notifications for transfers and customers.
  </Card>
  <Card title="Status & State Definitions" href="/docs/status-definitions" icon="fa-info-circle">
    Detailed reference for Customer, Transfer, and Sub-status definitions.
  </Card>
  <Card title="Error Codes" href="/docs/error-codes" icon="fa-triangle-exclamation">
    Reference for onboarding and transfer-related error codes.
  </Card>
</Cards>

<br />

Should you have any questions or need assistance, please don't hesitate to contact us at [contact-us@owlting.com](mailto:contact-us@owlting.com).
