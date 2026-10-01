---
title: Payload Examples
hidden: false
metadata:
  robots: index
privacy:
  view: public
---
When a customer passes KYB/KYC verification, when there is a transfer status update, or when an account-verification micro-deposit lands on one of your deposit accounts, we will send a webhook notification to the server URL you specified. The payload will contain the following data structures:

> 📘 Please [Verify the signature of the Webhook Request Header](doc:verify-signatures)

<br />

### Bank Account Micro-Deposit Received

Sent as `bank_account.micro_deposit.received` when an account-verification micro-deposit lands on a deposit account Harbor issued for you. An outside bank or platform pushes a few cents in to prove the account exists and belongs to the named holder, then asks you to report the amount it sent. When and why it fires is documented in [Account-verification micro-deposits](doc:micro-deposit-verification).

> 🚧 This payload is **not** wrapped in a `data` object. Unlike the examples above, `object`, `event` and the deposit fields all sit at the top level of the request body.

> 📘 Harbor also emails a notification for the same deposit, so your customer may hear about it before your integration does. See [Account-verification micro-deposits](doc:micro-deposit-verification) for who receives it.

> 📘 A micro-deposit is never on-ramped. It does not create a transfer, is not converted, places no hold, and never moves your balance. This event is the only notice you receive.

* Example - Micro-Deposit Received:

```json
{
  "object": "bank_test_deposit",
  "event": "bank_account.micro_deposit.received",
  "account_number": "312345678901",
  "amount": "0.24",
  "currency": "USD",
  "description": "ACCTVERIFY",
  "received_at": "2026-07-30T03:08:10+00:00",
  "crb_ach_transaction_id": 1234,
  "customer_id": "cus_EXAMPLEcustomerid00000000000000000000000"
}
```

| **Field** | **Type** | **Description** |
| --------- | -------- | --------------- |
| `object` | string | Always `bank_test_deposit`. |
| `event` | string | Always `bank_account.micro_deposit.received`. Route on this field. |
| `account_number` | string | The deposit account that received the money, in full and unmasked. Keep the leading zeros. |
| `amount` | string | The amount credited, for example `"0.24"`. It is a string on purpose: parse it with a decimal type, never a float. This is the value the verifying party asks you to confirm. |
| `currency` | string | ISO 4217 code. `USD` for every micro-deposit today. |
| `description` | string \| null | The raw ACH descriptor supplied by the sending bank, for example `ACCTVERIFY`. Free text chosen by the sender, not an enumeration, and not part of how Harbor classifies the deposit. `null` when the sender supplied none. |
| `received_at` | string \| null | ISO 8601 timestamp of when Harbor recorded the deposit, carrying an explicit offset (`+00:00` today). This is not the sending bank's settlement time, so read the offset rather than assuming a fixed one. |
| `crb_ach_transaction_id` | integer \| null | Identifier of the ACH transaction behind the deposit. It is read from the same record as `received_at`, so the two are always present together or `null` together. Quote it to support when a deposit needs tracing. |
| `customer_id` | string \| null | The Harbor Customer holding the receiving deposit account, as `cus_` followed by 40 characters. Present in every delivery today; the field stays nullable for accounts held at application level. |

**Handling notes**

* Use `account_number` together with `crb_ach_transaction_id` as your idempotency key. Do not key on `received_at`.
* Subscribe with `bank_account.micro_deposit.received` or the wildcard `bank_account.*`. Subscribing to `deposit_account.*` does **not** deliver this event.
* Respond with any `2xx` within 10 seconds. A non-2xx response or a timeout is retried, and an HTTP `404` is read as a dead endpoint and dropped without retry.
* Ignore unknown fields. Harbor may add fields, and will not rename or remove the fields above without notice.

<br />
