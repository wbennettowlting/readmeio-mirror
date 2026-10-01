---
title: Payment Lock Time
excerpt: >-
  This guide explains the settlement periods for ACH Pull and Debit Card
  transactions, and how funds are managed during the settlement process.
x-content:
  excerpt: >-
    This guide explains the settlement periods for ACH Pull and Debit Card
    transactions, and how funds are managed during the settlement process.
hidden: false
metadata:
  robots: index
x-privacy:
  view: public
---
### Settlement and Payment Lock Time

When using **ACH Pull** or **Debit Card** payment methods, funds undergo a settlement process before they are fully available for withdrawal or further transfer. This period allows banks to verify and clear the transaction.

***

#### Settlement Periods

The time required for funds to settle depends on the payment method used. Settlement periods are calculated in **business days** (excluding weekends and U.S. federal holidays).

| Payment Method | Settlement Period | Description |
| :--- | :--- | :--- |
| **Debit Card** | **T+1** | Funds settle one business day after the transaction is initiated. |
| **ACH Pull** | **T+2** | Funds settle two business days after the transaction is initiated. |

***

#### Transaction Status during Settlement

During the settlement period, you can track the progress of the transaction via the API using the `status` and `sub_status` fields:

* **Primary Status (`status`):** `pending_customer_transfer_start`
* **Sub-Status (`sub_status`):** `awaiting_settlement`

<Callout icon="📘" theme="info">
  **Note on awaiting_settlement:** 
  When the transfer `sub_status` shows `awaiting_settlement`, it indicates that **we have already successfully debited/pulled the funds** from the customer's bank account (via ACH Pull) or Debit Card. The transaction is simply undergoing the standard bank clearing and settlement process.
</Callout>

***

#### Disbursement Behavior

The behavior of funds during the settlement period depends on the destination of the transfer.

##### External Wallet Address
If the destination is an external blockchain address (e.g., a hardware wallet or an exchange):
* **Disbursement occurs only after settlement is complete.**
* The transfer status will remain in a processing state until the settlement period (T+1 or T+2) has passed.
* Once settled, OwlPay will broadcast the transaction to the blockchain.

##### Internal Wallet (Wallet API)
If the destination is an internal sub-account managed via the Wallet API:
* **Funds are disbursed immediately** to the internal wallet.
* **Payment Lock:** Although the funds appear in the balance, they are "locked." Locked funds **cannot be withdrawn or transferred** to other addresses until the settlement period is complete.
* Once the settlement period (T+1 or T+2) passes, the lock is automatically lifted, and the funds become fully available.

***

#### Example Timeline (ACH Pull)

Consider an ACH Pull transaction initiated on a **Friday**. Since weekends are not business days, the settlement period (T+2) starts on the following Monday.

* **Friday:** Transaction initiated.
* **Saturday & Sunday:** Non-business days (No progress).
* **Monday (T+1):** First business day.
* **Tuesday (T+2):** Second business day. Settlement completes.
* **Wednesday:** Funds are fully unlocked or disbursed to an external address.

<Callout icon="💡" theme="info">
  **Pro Tip:** To minimize waiting time, advise users to initiate transfers early in the week or use Debit Cards for faster T+1 settlement.
</Callout>

***

#### Why are funds locked?

Payment locking is a standard industry practice to protect against **ACH Returns** (e.g., insufficient funds or unauthorized transactions). By waiting for the settlement period to pass, OwlPay ensures that the source funds are cleared before they can be moved out of the ecosystem, maintaining the security and integrity of the platform.
