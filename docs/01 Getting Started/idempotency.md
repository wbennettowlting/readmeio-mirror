---
title: Idempotence
excerpt: Preventing duplicate API calls
content:
  excerpt: Preventing duplicate API calls
hidden: false
metadata:
  robots: index
privacy:
  view: public
---
Harbor's API supports **idempotency**, allowing you to safely retry requests without unintentionally duplicating the same operation. When you create or update an object, utilize an idempotency key. In the event of a connection error, you can confidently repeat the request without the possibility of creating a duplicate object or executing the update twice.

To perform an idempotent request, provide an additional **X-Idempotency-Key**: header to the request.

<br />

```shell Shell
curl --location --request POST 'https://harbor-sandbox.owlpay.com/api/v1/customers' \
--header 'Content-Type: application/json' \
--header 'Accept: application/json' \
--header 'X-API-KEY: {{API_KEY}}' \
--header 'X-Idempotency-Key: {{Idempotency-Key}}' \
--data-raw '{
  "type": "individual",
  "first_name": "John",
  "middle_name": "Michael",
  "last_name": "Doe",
  "email": "john.doe@example.com",
  "phone_country_code": "US",
  "phone_number": "555-555-1234",
  "birth_date": "1988-04-15",
  "description": "A freelance writer who loves exploring national parks."
}'
```
