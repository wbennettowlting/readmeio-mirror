---
title: Transfer RFI
hidden: false
metadata:
  robots: index
privacy:
  view: public
---
Handling Transactions with an **request\_for\_information** Status

When you initiates a Transfer, if the transfer status is switched to **request\_for\_information**, we require additional information to proceed with the transfer.

Typically, when a transfer enters this state, the response will include a link, referred to as **rfi\_link** (Request for Information link). This link directs the customer to a page where they can provide the necessary details to complete the transfer.

```json JSON
{
  ...,
  "rfi_link": "https://harbor.owlpay.com/rfi?token=125dsGskblSflwlo1W40",
  "created_at": "2025-02-07T13:05:32+00:00",
  "updated_at": "2025-02-07T13:05:32+00:00"
}
```

<br />

Please follow the instructions provided in the **rfi\_link** to ensure a smooth processing of your transaction.
