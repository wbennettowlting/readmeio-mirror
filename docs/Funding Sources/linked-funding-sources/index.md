---
title: Linked Funding Sources
metadata:
  robots: index
content:
  excerpt: >-
    Learn how to link a U.S. bank account (ACH Pull) or debit card as a funding
    source for on-ramp transfers through the Harbor API.
privacy:
  view: public
---
Harbor supports two ways for your customers to fund on-ramp transfers directly from their financial accounts — linking a **bank account** via ACH Pull, or binding a **debit card**.

<Cards columns={2}>
  <Card title="Link a Bank Account" href="/docs/link-bank-account" icon="fa-solid fa-building-columns">
    Connect a U.S. bank account via a secure widget and initiate ACH Pull transfers to purchase stablecoin.
  </Card>

  <Card title="Link a Debit Card" href="/docs/link-debit-card" icon="fa-solid fa-credit-card">
    Bind a debit card through a hosted binding page and use it as a funding source for on-ramp transfers.
  </Card>
</Cards>

### Comparison

|                        | Bank Account (ACH Pull)         | Debit Card                                     |
| :--------------------- | :------------------------------ | :--------------------------------------------- |
| **Payment Method**     | `ACH_PULL`                      | `DEBIT_CARD`                                   |
| **Linking Flow**       | Bank connection widget          | Card binding page                              |
| **Source ID Prefix**   | `clbacc_`                       | `card_`                                        |
| **Transfer Parameter** | `source_linked_bank_account_id` | `source_linked_card_id`                        |
| **Transaction Limits** | No per-transaction limit        | Per-transaction, daily, weekly, monthly limits |
| **Settlement Period**  | T+2 (Business Days)             | T+1 (Business Day)                             |

<Callout icon="📋" theme="info">
  For the complete list of supported card networks, card types, and country coverage, please refer to the [Debit Card Support Coverage](/docs/debit-card-coverage) guide.
</Callout>

### Settlement & Payment Lock Time

Transfers initiated from linked funding sources are subject to settlement periods and payment lock times. For detailed information on how this affects your funds availability, please refer to the [Settlement & Payment Lock Time](/docs/payment-lock-time) guide.

### Prerequisites

Both methods require the following before a customer can link a funding source:

1. Enable the corresponding payment method (`ACH_PULL` or `DEBIT_CARD`) for your Application
2. Complete KYC verification for the customer (status: `VERIFIED`)
3. Complete bank compliance onboarding for the customer (The Bank Onboarding status: `ONBOARDED`) (for Link a Bank Account).
