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


## Contact Us for Sandbox and Production Access

To get started with OwlPay Harbor, please contact us to request **Sandbox and Production access**.

We’ll first provide you with **Sandbox credentials and an `API_KEY`** so you can test and complete your integration in your environment. Once your integration is ready and the required testing is complete, we’ll provide your **Production `API_KEY`**, allowing you to go live.

> ❗️ **Keep Your API Key Secure**
>
> Your `API_KEY` provides access to your Harbor integration and must be stored securely.
>
> If you lose or suspect that your **Production `API_KEY` has been compromised**, contact our customer service team immediately. Never share your `API_KEY` or expose it in insecure environments, such as client-side code, public repositories, or logs.
