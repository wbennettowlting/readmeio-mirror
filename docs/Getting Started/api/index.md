---
title: API
excerpt: How to authenticate requests and safely retry them with the Harbor API.
hidden: false
---
Every request to the Harbor API needs an API key, and requests that create or update data should use an idempotency key so they can be retried safely.

<Cards columns={2}>
  <Card title="Authentication" href="/docs/authentication" icon="fa-solid fa-key">
    Authenticate requests with your API key in the `X-API-KEY` header.
  </Card>
  <Card title="Idempotency" href="/docs/idempotency" icon="fa-solid fa-rotate">
    Use the `X-Idempotency-Key` header to retry requests without creating duplicates.
  </Card>
</Cards>
