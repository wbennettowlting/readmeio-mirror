---
title: Subscription
metadata:
  robots: index
content:
  excerpt: >-
    This document describes the API specification for subscribing to event
    notifications (webhooks).
privacy:
  view: public
slug: webhooks-overview
---
You create a Subscription with a target HTTPS endpoint and a list of Event Types. When an event occurs, Harbor POSTs a signed JSON payload to your endpoint, you must verify authenticity using Harbor’s HMAC signature headers.

<br />

### Quick Start:

1. [Subscription APIs](doc:subscription-apis)
2. [List of subscribing events](https://harbor-developers.owlpay.com/docs/subscribable-events) 
3. How to verify the request from Harbor - [Verifying requests from Harbor](doc:verify-signatures)
4. Webhook Payload Example - [Webhook payload example](doc:payload-examples)
