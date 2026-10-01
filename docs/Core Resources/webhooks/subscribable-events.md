---
title: Subscribable Events
hidden: false
metadata:
  robots: index
x-privacy:
  view: public
---
<br />

| **Event**                        | **Description**                                                                                                |
| -------------------------------- | -------------------------------------------------------------------------------------------------------------- |
| `*`                              | Subscribe to all available event types. Harbor will send every event notification supported by the system.     |
| `bank_account.*`                 | Subscribe to all bank account-related events.                                                                  |
| `bank_account.micro_deposit.received` | Fired when an account-verification micro-deposit (under US$1.00) is received on a deposit account issued to you. It does **not** create an ON_RAMP transfer and is not converted. See [Account-verification micro-deposits](doc:micro-deposit-verification). |
| `customer.*`                     | Subscribe to all customer-related events.                                                                      |
| `customer.agreement.accepted`    | Fired when a customer has successfully signed and accepted the required terms or service agreement.            |
| `customer.kyc.verifying`         | The customer has submitted KYC information and verification is in progress.                                    |
| `customer.kyc.verified`          | The customer’s KYC verification has been successfully completed and approved.                                  |
| `customer.kyc.revoked`           | The customer’s previously verified KYC status has been revoked (e.g., expired documents or compliance review). |
| `customer.kyc.rejected`          | The KYC verification was rejected due to invalid or incomplete information.                                    |
| `customer.kyc.declined`          | The KYC application was declined manually or by compliance review; the user cannot proceed.                    |
| `customer.kyc_level.changed`     | Fired when the customer's KYC level changes, in either direction. Only a real change is announced, and the payload carries the new level. |
| `customer.rfi.raised`            | Fired when compliance raises a request for information (RFI) against the customer. The customer must supply the requested details before they can continue. |
| `customer.rfi.resolved`          | Fired when a customer-level RFI has been answered and no longer blocks the customer. |
| `customer_onboarding.*`          | Subscribe to all customer onboarding-related events (direct API integration).                                  |
| `customer_onboarding.submitted`  | Fired when the onboarding payload has been successfully submitted to the compliance provider for verification. |
| `customer_onboarding.action_required` | Fired when the onboarding requires attention or corrections (requires PATCH to resolve).                      |
| `customer_onboarding.verified`   | Fired when the onboarding verification is approved (customer is KYC/KYB verified).                             |
| `customer_onboarding.declined`   | Fired when the onboarding verdict is a declinature that cannot be corrected. Contrast `customer_onboarding.action_required`, which is the correctable case. |
| `customer_onboarding.rejected`   | **Retired.** An uncorrectable verdict is now announced as `customer_onboarding.declined`, and a correctable one as `customer_onboarding.action_required`. A subscription that still carries this type receives nothing. |
| `customer_onboarding.failed`     | Fired when the onboarding verification fails due to a systemic compliance provider error.                       |
| `deposit_account.*`              | Subscribe to all virtual deposit account-related events.                                                       |
| `deposit_account.activated`      | Fired when the customer's virtual deposit account has been successfully provisioned and activated; deposit instructions (e.g., account number) are now available. |
| `deposit_account.failed`         | Fired when the provisioning of the customer's virtual deposit account has failed.                             |
| `deposit_account.disabled`       | Fired when a customer's virtual deposit account is closed/disabled.                                            |
| `transfer.*`                     | Subscribe to all transfer-related events.                                                                      |
| `transfer.deposit.created`       | Fired when an inbound deposit is received by a customer's deposit account, and a corresponding ON_RAMP transfer is successfully created in Harbor. Subsequent status updates will follow the standard `transfer.status.*` events. |
| `transfer.status.completed`      | The transfer has been successfully completed and funds are settled.                                            |
| `transfer.status.expired`        | The transfer request has expired because it was not completed within the allowed time frame.                   |
| `transfer.status.on_hold`        | The transfer is temporarily placed on hold (e.g., under compliance review or awaiting manual approval).        |
| `transfer.status.pending_harbor` | The transfer is pending settlement or confirmation by Harbor.                                                  |
| `transfer.status.rejected`       | The transfer has been rejected (for example, due to invalid recipient details or compliance failure).          |
| `transfer.status.refunded`       | Fired when a transfer moves to `refunded`: the funds did not reach the recipient and have been returned.       |
| `transfer.sub_status.*`          | Subscribe to all transfer sub-status change events.                                                           |
| `transfer.sub_status.awaiting_settlement` | Fired when a transfer's sub-status changes to awaiting settlement.                                     |
| `transfer.sub_status.cpn_rfi`    | Fired when a transfer requires additional information or documentation (RFI) from the customer.               |
| `transfer.sub_status.cpn_rfi_submitted` | Fired when required RFI information or documentation for a transfer has been submitted.               |
| `wallet.*`                       | Subscribe to all wallet-related events.                                                                        |
| `wallet.balance.updated`         | When there are any changes in the wallet balance.                                                              |

<br />
