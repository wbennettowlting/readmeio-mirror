---
title: Reconciliation
hidden: true
metadata:
  robots: index
privacy:
  view: anyone_with_link
---
In Harbor, a **Reconciliation** object is automatically generated whenever a **Transfer** with a `destination` is successfully paid. A single Reconciliation can include multiple Transfers, making it the central object for settlement and reconciliation processes.

<br />

### Reconciliation and Transfer Relationship

When a Transfer reaches the `completed` state for its destination, Harbor automatically links it to a newly created  Reconciliation.

***

### Settlement Strategy

If you configure the `settlement_strategy` to **`scheduled`**, Harbor will trigger settlement every **12 hours**. During each scheduled settlement:

* A new Reconciliation is created (if eligible Transfers exist).
* Eligible Transfers are batched into the Reconciliation.

***

### Status Updates and Callbacks

Every Reconciliation has a **status lifecycle** (e.g., `pending`, `processing`, `completed`).
When the `reconciliation.status` is updated, Harbor will send a notification to your **callback URL**.

The callback payload includes the `reconciliation.id`, allowing you to:

1. Query the specific Reconciliation details.
2. Retrieve the list of associated Transfers for reconciliation checks.

***

### Example Flow

1. **Transfer Paid** → Harbor creates a Reconciliation.
2. **Scheduled Settlement (every 12h)** → Transfers are grouped.
3. **Status Change** → Harbor triggers callback with `reconciliation.id`.
4. **Your System** → Use `reconciliation.id` to fetch all Transfers for reconciliation.
