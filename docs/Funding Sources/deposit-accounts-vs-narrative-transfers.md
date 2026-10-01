---
title: Deposit Accounts vs. Narrative Transfers
excerpt: >-
  Learn the key architectural and operational differences between Customer
  Deposit Accounts and Narrative-Based Transfers in Harbor.
content:
  excerpt: >-
    Learn the key architectural and operational differences between Customer
    Deposit Accounts and Narrative-Based Transfers in Harbor.
hidden: false
metadata:
  robots: index
privacy:
  view: public
---
> 📘 Regional Availability & Usage
>
> The detailed instructions and workflows outlined in this guide are primarily optimized for **US users (including both businesses and individuals)**. 
> 
> **Non-US users can still perform on-ramps and obtain bank accounts** through Harbor, but the underlying mechanisms, requirements, and usage guidelines may differ. Please consult your OwlPay representative for tailored non-US onboarding instructions.

When integrating fiat-to-crypto (on-ramp) services with Harbor, you can utilize bank accounts in two distinct ways depending on your business model: **Transfer (On-Ramp)** or **Customer Deposit Account**. 

Understanding the differences in account provisioning, fund routing, and transaction mapping is critical for a smooth integration.

---

### 💡 Key Concept Overview

* **Transfer (On-Ramp)**: Uses a **bank account under the Sender's name** with a unique **Narrative** (a 10-digit code). The Customer (Sender) receives wire instructions containing this account details and the narrative. The Customer (Sender) **must** input this narrative into the wire reference field during the bank transfer. Harbor uses this narrative to map the deposit to the pre-created Transfer object.
* **Customer Deposit Account**: Harbor provisions a **dedicated/virtual bank account** for each customer. The customer gets their own unique account details. **No narrative is required** when transferring funds. Harbor identifies the customer directly by the account number, automatically creates a Transfer after the deposit is detected, and routes the funds.

---

### 1. Architectural Flowchart

```mermaid
flowchart TB
    subgraph transfer_flow["Transfer (on-ramp) — Account under Sender's name + Narrative"]
        direction TB
        T1["Application creates Transfer<br/>(quote → POST /transfers)"] --> T2["Harbor returns transfer instructions:<br/>Bank account under Sender's name<br/>+ unique 10-digit narrative"]
        T2 --> T3["Customer (Sender) wires fiat to the bank<br/>(account under Sender's name)<br/>⚠️ MUST include narrative<br/>in wire reference"]
        T3 --> T4["Bank receives the deposit"]
        T4 --> T5["Harbor detects the deposit and<br/>matches it by narrative → Transfer"]
        T5 --> T6["EXISTING Transfer status updated<br/>(on_hold → AML → completed)"]
        T6 --> T7["Crypto sent to destination"]
    end

    subgraph depacc_flow["Customer Deposit Account — Dedicated account, no narrative"]
        direction TB
        D1["Application creates Deposit Account<br/>(POST /customers/:id/deposit_accounts)<br/>status: PROVISIONING"] --> D2["Harbor provisions a DEDICATED<br/>bank account for the customer"]
        D2 --> D3["Account ACTIVE<br/>deposit_instructions:<br/>account_number, routing_number...<br/>❌ no narrative"]
        D3 --> D4["Customer (Sender) wires fiat to the bank<br/>directly to the dedicated account<br/>✅ no reference required"]
        D4 --> D5["Bank receives the deposit"]
        D5 --> D6["Harbor identifies the customer<br/>by the account number itself"]
        D6 --> D7["Transfer AUTO-CREATED<br/>after the deposit is detected"]
        D7 --> D8["Crypto sent to customer's<br/>wallet / external address"]
        D8 -.->|"account is reusable<br/>for future deposits"| D4
    end
```

---

### 2. Sequence Diagrams

```mermaid
sequenceDiagram
    participant C as Customer (Sender)
    participant App as Application (API client)
    participant H as Harbor
    participant B as Bank
    participant R as Receiver

    rect rgb(230, 240, 255)
        note over C,R: Transfer (on-ramp) — Transfer exists FIRST, mapped by narrative
        App->>H: POST /transfers (with quote)
        H->>B: Prepare deposit on bank account under Sender's name
        H-->>App: Transfer instructions<br/>(account details + unique narrative)
        App-->>C: Show wire instructions incl. narrative
        C->>B: Wire USD, narrative in reference field
        B-->>H: Incoming deposit detected
        H->>H: Match deposit → Transfer by narrative
        H->>H: Update EXISTING Transfer status<br/>(on_hold → AML → completed)
        H->>R: Deliver stablecoin (on blockchain)
        H-->>App: Webhook: transfer status updated
    end

    rect rgb(235, 255, 235)
        note over C,R: Deposit Account — account identifies customer, Transfer created AFTER deposit
        App->>H: POST /customers/{id}/deposit_accounts
        H->>B: Provision dedicated account for the customer
        B-->>H: Dedicated account_number issued
        H-->>App: deposit_instructions (status ACTIVE, no narrative)
        C->>B: Wire USD to dedicated account (no reference needed)
        B-->>H: Incoming deposit detected
        H->>H: Identify customer by account number
        H->>H: AUTO-CREATE Transfer, then AML / status flow
        H->>R: Deliver stablecoin (on blockchain)
        H-->>App: Webhook: transfer created & status updated
    end
```

---

### 3. Core Differences

* **Account Type & Ownership**:
  * **Transfer (On-Ramp)**: Bank account under the Sender's name (requires a unique Narrative to map the specific transfer).
  * **Customer Deposit Account**: Dedicated Virtual Account registered for the specific Customer (no narrative needed).

* **Narrative (Wire Reference)**:
  * **Transfer (On-Ramp)**: **Required**. A unique 10-digit number. This is the sole identifier for mapping the deposit.
  * **Customer Deposit Account**: **Not Required**. The unique account number itself identifies the customer.

* **Transfer Creation Timing**:
  * **Transfer (On-Ramp)**: **Before deposit**. Created proactively by the Application client via API.
  * **Customer Deposit Account**: **After deposit**. Automatically created by Harbor once the bank detects incoming funds.

* **Reusability**:
  * **Transfer (On-Ramp)**: **No**. A narrative is one-time use per specific Transfer request.
  * **Customer Deposit Account**: **Yes**. The account is permanently assigned to the customer and can be reused for any future deposits.

* **Mapping Risk**:
  * **Transfer (On-Ramp)**: **Medium/High**. Payers may forget or mistype the narrative, leading to pending deposits that require manual mapping.
  * **Customer Deposit Account**: **Low/None**. There is no reference field to mistype.
