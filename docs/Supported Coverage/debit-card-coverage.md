---
title: Debit Card Coverage
x-content:
  excerpt: >-
    Learn about the current regional, network, and card type support scope for
    Debit Card (VDC) transactions through the Harbor API.
metadata:
  robots: index
x-privacy:
  view: public
---
The Debit Card (Visa Direct to Card / VDC) feature supports Account Funding Transactions (AFT/Pull) for on-ramping and Original Credit Transactions (OCT/Push) for off-ramping, subject to specific network and regional support.

### Supported Countries & Regions

Our Debit Card coverage is currently focused on the U.S. market, with plans to expand internationally.

* **AFT (Pull - On-Ramp):** Currently accepts **only** cards issued by **U.S. banks**.
* **OCT (Push - Off-Ramp):** Currently accepts **only** cards issued by **U.S. banks**. Additional country support will be rolled out in phases.

<Callout icon="🌐" theme="info">
  We are actively working on expanding card-acquiring and payout countries. Stay tuned for future updates!
</Callout>

---

### Supported Card Networks

We support transactions from the following major card networks and ATM networks:

* **Visa**
* **Mastercard**
* **Pulse**
* **Star**
* **NYCE**

---

### Supported Card Types

The following card types are eligible for use on Harbor. Credit cards are strictly unsupported.

* **Debit**: Standard debit cards issued by banks that are linked directly to a cardholder's checking account.
* **Prepaid**: Reloadable cards pre-funded with money (such as gift cards or loaded payroll cards).
* **Combo**: Multi-functional cards combining credit, debit, or ATM functions on a single physical card.
* **Charge Card**: Cards without a pre-set spending limit where the balance must be paid in full at the end of each statement cycle.
* **Deferred Debit**: Cards where transactions are temporarily accumulated and debited from the bank account at a later specified date.
* **Visa Plus**: ATM network cards branded under Visa's Plus system for global debit and cash routing.

<Callout icon="⚠️" theme="warning">
  Credit cards are **not** supported for Debit Card (VDC) transactions. Please ensure your users connect an eligible debit or prepaid card.
</Callout>
