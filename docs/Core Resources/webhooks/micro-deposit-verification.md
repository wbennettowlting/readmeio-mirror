---
title: Micro-deposit Verification
deprecated: false
hidden: false
metadata:
  robots: index
---
An outside bank or platform can prove that a deposit account is real and belongs to the named holder by pushing a few cents into it and then asking you to report the amount it sent. Harbor reports that deposit to you as `bank_account.micro_deposit.received`, so you can read the amount and the sender's descriptor and complete the check on their side.

This is not a payment. It is a verification probe that happens to move money.

<br />

### When Harbor raises the event

All of the following must hold:

* The deposit arrives as an **inbound ACH push** from an external bank. An externally initiated ACH pull never qualifies, and wire transfers never qualify.
* The receiving account is one of the deposit accounts Harbor issues to you and your customers.
* The amount is above zero and **below US$1.00**. Exactly 1.00 does not qualify.
* The deposit has not been seen before. A repeated notification from the banking network does not fire the event twice.
* The receiving account number resolves to your application, and that application has at least one enabled subscription matching the event.

The classification does not look at the descriptor. `ACCTVERIFY` is simply what many senders write; the wording is theirs and carries no meaning for Harbor.

If the account number matches no account, no webhook is sent and Harbor raises an internal alert instead. Read a missing webhook as "not matched", never as "matched and handled quietly".

<br />

### A micro-deposit is not an on-ramp

> 📘 It never becomes a transfer: no `transfer.deposit.created` is emitted, no conversion takes place, no hold is placed, and your balance does not move. Compare `transfer.deposit.created`, which is fired when a genuine inbound deposit creates an ON_RAMP transfer.

Those few cents will not appear in your regular payment records or in balance movements. This event, and the email below, are the only notice you receive.

<br />

### An email goes out as well

Harbor also sends an email notification for the same deposit, so the webhook is not the only thing that happens.

* When the receiving account is held by a Customer, the email goes to **that Customer**, and your business owner is copied.
* When your application handles KYC itself, it goes to your application's registered email address instead.

The email shows the amount, the sender's descriptor and a masked account number (last four digits only), and its subject is `Micro Deposit of <amount> <currency> Received | OwlPay`. Your end customer may therefore hear about the deposit before your integration does — worth aligning your support script with.

<br />

### Payload

The full payload, field types and handling notes live with the other event payloads: [**Webhook payload example**](/docs/payload-examples).

Subscribe with `bank_account.micro_deposit.received`, or the wildcard `bank_account.*`. Subscribing to `deposit_account.*` does **not** deliver this event.

<br />

### Testing it in the sandbox

> 🚧 Planned for release in October 2026
>
> The sandbox simulation described below is not available yet.

Simulate an inbound ACH push below US$1.00 against one of your deposit accounts:

```curl curl
curl --location --request POST 'https://harbor-sandbox.owlpay.com/api/v1/deposit_accounts/depacc_1234567890/simulate_payment' \
--header 'Content-Type: application/json' \
--header 'Accept: application/json' \
--header 'X-API-KEY: {YOUR_API_KEY}' \
--data-raw '{
  "amount": 0.24,
  "method": "ACH_PUSH"
}'
```

The simulated deposit is classified by the same rules as a production deposit, so an `amount` of `1.00` or more arrives as an ordinary deposit instead, and `"method": "WIRE"` never produces this event. The response tells you which path was taken: `classified_as` is `micro_deposit` for this event, and `deposit` for an ordinary simulated deposit.

<br />