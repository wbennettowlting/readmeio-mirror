---
title: Third-Party Recipients
excerpt: Handling Third-Party Payments via `destination.is_self_transfer`
x-content:
  excerpt: Handling Third-Party Payments via `destination.is_self_transfer`
hidden: false
metadata:
  robots: index
x-privacy:
  view: public
---
If you are initiating a transfer on behalf of a customer and the recipient is not the customer, you must clearly indicate this in your API request.

To represent such a third-party payment scenario, set the following field in your `POST /transfers` API request:

```curl curl
curl --location --request POST 'https://harbor-sandbox.owlpay.com/api/v1/transfer' \
--header 'Content-Type: application/json' \
--header 'Accept: application/json' \
--header 'X-API-KEY: {{API_KEY}}' \
--header 'Idempotency-Key: {{Idempotency-Key}}' \
--data-raw '{
    ...,
    "destination": {
        ...,
        "is_self_transfer": false
    }
}'
```

> ❗️ If you pass `false` in `is_self_transfer`
>
> If the payer and the payee are obviously different and the wrong field is passed, it may cause payment delays, and the transaction may also be cancelled.

Setting `"is_self_transfer": false` informs OwlPay Harbor that the transfer involves two different parties:

* The payer (your customer, identified via `on_behalf_of`)
* The payee (a third-party recipient specified in the `destination` object)

This field is essential for:

* Correct application of compliance logic
* Proper handling of transaction records and audit trails
* Preventing misuse of customer funds for self-transfers disguised as third-party payments

If the recipient is the same individual or business as the sender (i.e., the customer is transferring funds to themselves), then set `is_self_transfer` to `true`.

<br />

<Callout icon="🚧">
  Please note that setting **is_self_transfer** in an **on-ramp** scenario may affect the changes of the Travel Rule.
</Callout>
