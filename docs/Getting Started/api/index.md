---
title: API
excerpt: How to authenticate requests and safely retry them with the Harbor API.
hidden: false
---
Every request to the Harbor API needs an API key, and requests that create or update data should use an idempotency key so they can be retried safely.

<Cards columns={2}>
  <Card title="API Keys" href="/docs/api-keys" icon="fa-solid fa-key">
    Authenticate requests with your API key in the `X-API-KEY` header.
  </Card>
  <Card title="Idempotency" href="/docs/idempotency" icon="fa-solid fa-rotate">
    Use the `X-Idempotency-Key` header to retry requests without creating duplicates.
  </Card>
</Cards>


## Get API access

Contact us to request access. We'll send you a Sandbox `API_KEY` to build and test your integration. Once testing is complete, we'll issue your Production `API_KEY` so you can go live.

<br />

> ❗️ **Keep your API key secure**
>
> Never expose your `API_KEY` in client-side code, public repositories, or logs. If your Production key is lost or compromised, contact customer service immediately.
