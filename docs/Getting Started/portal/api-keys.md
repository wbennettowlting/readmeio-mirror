---
title: API Keys
excerpt: >-
  This guide explains how to securely authenticate and access the Owlting Harbor
  API using API keys.
x-content:
  excerpt: >-
    This guide explains how to securely authenticate and access the Owlting
    Harbor API using API keys.
hidden: false
metadata:
  robots: index
x-privacy:
  view: public
---
OwlPay Harbor API uses API keys for authentication. All requests must include the API key in the **X-API-Key** header. No additional credentials or passwords are required.

### Obtaining API Keys

You can generate and manage your API keys directly through the **Harbor Portal**.

* Log in to the [Harbor Portal](https://harbor-sandbox.owlpay.com/portal).
* Navigate to the **API Keys** tab in the main navigation bar.

   ![](https://files.readme.io/133d787faeda9542ed126b9fa6c32fa0736040147f8ab48fc743c37d76fa2ef3-Google_Chrome_2026-04-09_20.58.08.png)
* Click the **+ Create API Key** button.

   ![](https://files.readme.io/4e09b2422bc6a4724db9af4e16091e713038b50c58edf0fc205c31d608fe3002-Google_Chrome_2026-04-09_20.58.10.png)
* Once the key is generated, copy and save it in a secure location immediately.

<br />

> ⚠️ **Important:**
>
> For security reasons, the API key is only displayed **once** at the time of creation. It cannot be retrieved after you close the dialog. If you lose your key, you will need to create a new one.

If a request is made without an API key or with an invalid key, the system will return a **401 - Unauthorized error**. Additionally, **all API requests must be sent over HTTPS**, as HTTP requests will be automatically rejected.

<br />

> ⚠️ Warning! API Key Security and Best Practices
>
> Your API key grants full access to the API, so it should be handled with extreme care. Never expose your API key in public repositories, internal broadcasts, or unprotected code. Avoid hardcoding API keys in your application, and ensure they are securely stored.

<br />

### Example

```curl sh
curl --location --request POST 'https://harbor-sandbox.owlpay.com/api/v1/customers' \
--header 'Content-Type: application/json' \
--header 'Accept: application/json' \
--header 'X-API-KEY: {{API_KEY}}'
```
