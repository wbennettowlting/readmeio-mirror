---
title: Overview
content:
  excerpt: Welcome to the Harbor Integration Guide!
metadata:
  robots: index
privacy:
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
  <Card title="Customer Onboarding" href="https://harbor-developers.owlpay.com/docs/onboarding-overview" icon="fa-user-check">
    Onboard and verify your customers (individual or business profiles) via hosted links or direct API integration.
  </Card>
  <Card title="Customer" href="https://harbor-developers.owlpay.com/docs/customer#/" icon="fa-user">
    Manage customer data and their verification statuses.
  </Card>
  <Card title="Link a Bank Account" href="https://harbor-developers.owlpay.com/docs/link-bank-account" icon="fa-building-columns">
    Connect and link U.S. bank accounts for ACH Pull transactions.
  </Card>
  <Card title="Link a Debit Card" href="https://harbor-developers.owlpay.com/docs/link-debit-card" icon="fa-credit-card">
    Bind a debit card to purchase cryptocurrency directly.
  </Card>
  <Card title="Customer's Deposit Account" href="https://harbor-developers.owlpay.com/docs/deposit-accounts" icon="fa-wallet">
    Provision dedicated bank accounts for customers to auto-on-ramp fiat to crypto.
  </Card>
</Cards>

### Transfer Types

<Cards>
  <Card title="Get a Quote" href="https://harbor-developers.owlpay.com/docs/quotes" icon="fa-calculator">
    Get exchange rates, fees, and settlement amounts before creating a transfer.
  </Card>
  <Card title="Fiat → Stablecoin" href="https://harbor-developers.owlpay.com/docs/on-ramp" icon="fa-arrow-right">
    On-ramp: convert fiat currency to stablecoin.
  </Card>

  <Card title="Stablecoin → Fiat" href="https://harbor-developers.owlpay.com/docs/off-ramp" icon="fa-arrow-left">
    Off-ramp: convert stablecoin back to fiat currency. Includes transfers with local currency.
  </Card>

  <Card title="Stablecoin → Stablecoin" href="https://harbor-developers.owlpay.com/docs/on-chain" icon="fa-repeat">
    On-chain swap between stablecoins.
  </Card>
</Cards>

### Reference

<Cards>
  <Card title="Supported Blockchains & Stablecoins" href="https://harbor-developers.owlpay.com/docs/stablecoins-and-blockchains#/" icon="fa-cubes">
    A complete list of blockchain networks and stablecoins supported by Harbor.
  </Card>

  <Card title="Wallet API" href="https://harbor-developers.owlpay.com/docs/wallet-overview" icon="fa-wallet">
    Create and manage blockchain wallets for receiving and sending crypto assets.
  </Card>
  <Card title="Webhook Subscriptions" href="https://harbor-developers.owlpay.com/docs/webhooks-overview" icon="fa-bell">
    Subscribe to real-time event notifications for transfers and customers.
  </Card>
  <Card title="Status & State Definitions" href="https://harbor-developers.owlpay.com/docs/status-definitions" icon="fa-info-circle">
    Detailed reference for Customer, Transfer, and Sub-status definitions.
  </Card>
  <Card title="Error Codes" href="https://harbor-developers.owlpay.com/docs/error-codes" icon="fa-triangle-exclamation">
    Reference for onboarding and transfer-related error codes.
  </Card>
</Cards>

<br />

Should you have any questions or need assistance, please don't hesitate to contact us at [contact-us@owlting.com](mailto:contact-us@owlting.com).
