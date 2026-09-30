---
title: Status & State Definitions
metadata:
  robots: index
privacy:
  view: public
---
### Status Definitions

#### Customer Status

| **Status**                  | **Description**                                                                                                                               |
| :-------------------------- | :-------------------------------------------------------------------------------------------------------------------------------------------- |
| **unfinished**              | Form not completed.                                                                                                                           |
| **finished**                | Form completed, ready for verification, but has not yet been submitted.                                                                       |
| **verifying**               | Under verification.                                                                                                                           |
| **verified**                | Verified and active.                                                                                                                          |
| **rejected**                | The onboarding application is rejected due to errors in the submitted form or files (e.g. invalid formatting, illegible documents). The customer is allowed to modify the incorrect fields on their onboarding form and resubmit it for review. |
| **declined**                | Application rejected, resubmission not allowed.                                                                                               |
| **deactivated**             | Previously usable but later suspended.                                                                                                        |
| **request_for_information** | Additional supporting documents or clarifications are required by the compliance team to proceed with review (e.g. proof of business relationship). The customer does not need to edit the main onboarding form; they provide the specific requested details via the RFI process. |

<br />

> 📘 Difference between **rejected** and **request_for_information**
>
> - **rejected (Form/File Errors):** Occurs when the submitted form data or documents are incorrect or invalid. The customer must correct the specific onboarding form fields and resubmit the entire application.
> - **request_for_information (Supplementary Compliance Request):** Occurs when the form itself is valid, but compliance officers need supplementary information or specific extra documents to clear checks. The customer only needs to provide the requested details without altering or redoing the core onboarding form.

<br />

#### Customer State

The `state` field is a higher-level, customer-facing status derived from the underlying internal customer `status`. Providing the `state` field in the Customer object makes it much easier to understand the current onboarding and integration status at a glance. It simplifies integration by grouping internal status details into actionable states.

| **State** | **Mapped from `status`** | **Description** |
| :--- | :--- | :--- |
| **not_started** | `unfinished` | KYC has not been started yet. |
| **in_progress** | `finished` | Customer has filled out the form but has not yet submitted it for verification. |
| **pending_review** | `verifying` | Submitted and awaiting review. |
| **verified** | `verified` | Approved and able to transact. |
| **update_required** | `rejected` / `request_for_information` | Customer action is required before proceeding. |
| **declined** | `declined` | Permanently rejected; resubmission not allowed. |
| **suspended** | `deactivated` | Previously verified but now blocked. |

<br />

> 📘 Request for Information (RFI) Priority
>
> The mapping between `status` and `state` is not strictly one-to-one:
> - **RFI Priority**: If a customer is flagged for a Request for Information (RFI), the `state` will return as `update_required` regardless of their current underlying `status`.
> - **Example Case**: A customer who is internally `verified` may show a public-facing state of `update_required` if they have an outstanding request for more information. This state persists until the requested information is provided.

<br />

#### Transfer Status

| **Status**                             | **Description**                                                                                                                                                                                                                                                            |
| :------------------------------------- | :------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **pending_customer_transfer_start**    | The customer has not yet initiated their transfer to the instruction.                                                                                                                                                                                                      |
| **pending_customer_transfer_complete** | The payment has been successfully received by the customer and the off-chain funds are available. Only used for off-ramp (fiat → crypto) transfers.                                                                                                                        |
| **pending_external**                   | Deposit/withdrawal has been submitted to an external network but is not yet confirmed. Waiting for another external crypto network or a bank transfer to complete.                                                                                                         |
| **pending_harbor**                     | Deposit/withdrawal is being processed internally by Harbor. This can also be used when Harbor must verify KYC information prior to deposit/withdrawal.                                                                                                                     |
| **on_hold**                            | Transfer is on hold for additional compliance checks (e.g. AML verification). This may occur before or after funds are received — for example, on-ramp CRB transfers (Debit Card/ACH Pull) trigger AML review before funds are pulled. Once cleared, the transfer resumes. |
| **request_for_information**            | The Harbor team will contact you and may ask you to provide more details about the transaction.                                                                                                                                                                            |
| **pending_customer**                   | The customer must take additional action before the deposit/withdrawal can complete, such as an email or 2FA confirmation of a withdrawal.                                                                                                                                 |
| **completed**                          | Deposit/withdrawal fully completed.                                                                                                                                                                                                                                        |
| **refunded**                           | The deposit/withdrawal is fully refunded.                                                                                                                                                                                                                                  |
| **expired**                            | Funds were never received, and the transfer is considered abandoned by the customer.                                                                                                                                                                                       |
| **cancelled**                          | Deposit/withdrawal was cancelled by the customer.                                                                                                                                                                                                                          |
| **reject**                             | The transfer has been rejected.                                                                                                                                                                                                                                            |
| **error**                              | Catch-all for any error not enumerated above.                                                                                                                                                                                                                              |

<br />

#### Transfer Sub-Status

The `sub_status` field provides a more granular state explanation within a primary `status`. This is particularly useful for tracking transactions that are undergoing multi-step clearing or bank settlement processes (such as ACH Pull or Debit Card transfers).

| **Sub-Status** | **Associated Status** | **Description** |
| :--- | :--- | :--- |
| **awaiting_settlement** | `pending_customer_transfer_start` | The debit instruction has been successfully initiated by the system (funds are being pulled from the customer's ACH bank account or Debit Card), but is currently awaiting bank clearing and settlement. |

<br />

> 📘 Why does `awaiting_settlement` occur?
> 
> When a customer initiates a transfer using **ACH Pull** or **Debit Card**, the system immediately triggers a pull/debit request to the customer's financial institution. 
> 
> - **ACH Pull** takes **2 U.S. business days (T+2)** to fully clear and settle.
> - **Debit Card** takes **1 U.S. business day (T+1)** to fully clear and settle.
>
> During this settlement window, the main transfer status remains `pending_customer_transfer_start` (because the funds have not yet cleared or arrived in Harbor's settlement account). However, the `sub_status` transitions to `awaiting_settlement` to inform you that **we have already debited/pulled the funds**, and we are simply waiting for the standard bank settlement period to complete.
