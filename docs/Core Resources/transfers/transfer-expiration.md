---
title: Transfer Expiration
hidden: false
metadata:
  robots: index
x-privacy:
  view: public
---
In Harbor, if the **transfer** has **no funds are received within 72 hours**, the **transfer** will be marked as `expired` and an event will be triggered to notify your server.

Harbor automatically **marks a same-currency transfer as expired** if **no funds are received within 72 hours** and triggers a webhook event to your server.
For **cross-currency transfers**, expiration times **differ by currency pair**.
