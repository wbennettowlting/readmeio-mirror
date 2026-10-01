---
title: Customers
excerpt: >-
  This guide provides an overview of how to register and manage customers using
  the Harbor Customers API, including KYC verification, required headers, and
  API request examples.
content:
  excerpt: >-
    This guide provides an overview of how to register and manage customers
    using the Harbor Customers API, including KYC verification, required
    headers, and API request examples.
hidden: false
metadata:
  robots: index
privacy:
  view: public
---
The Harbor Customers API allows you to get KYC link for Customer, enabling you to manage the verification process independently. This API provides flexibility in handling customer interactions.

<Callout icon="👉" theme="info">

**Direct API Reference:**
* [API Reference: Create a Customer](doc:createacustomer)

</Callout>

<br />

### Register Your First Customer

You are now ready to start using Harbor’s core APIs.
The first step is to create a customer object, which represents a user in your system.

When making this request, ensure you include the required headers as shown in the example below. Most importantly, you must pass the **X-API-KEY** to authenticate the request.

> ⚠️ Key Requirements
>
> The customer must agree to Harbor’s Terms of Service before proceeding with KYC/AML verification.
> An Idempotency Key is required (**X-Idempotency-Key**) to prevent duplicate submissions.

#### API Request: Creating a Customer

```curl sh
curl --location --request POST 'https://harbor-sandbox.owlpay.com/api/v1/customers' \
--header 'Content-Type: application/json' \
--header 'Accept: application/json' \
--header 'X-API-KEY: {{API_KEY}}' \
--header 'Idempotency-Key: {{Idempotency-Key}}' \
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

<br />

#### API Response Example: Individual Customer

The endpoint will return a response as shown below for individual customers. Note that **kyc_link** is returned for verification.

```json JSON
{
    "data": {
        "uuid": "{{customer_uuid}}",
        "status": "deactivated",
        "type": "individual",
        "first_name": "John",
        "middle_name": "Michael",
        "last_name": "Doe",
        "email": "john.doe@example.com",
        "phone_country_code": "US",
        "phone_number": "555-555-1234",
        "birth_date": "1988-04-15",
        "has_signed_agreement": false,
        "agreement_link": "https://harbor-sandbox.owlpay.com/agreement_link",
        "kyc_link": "https://harbor-sandbox.owlpay.com/kyc_link",
        "description": "A freelance writer who loves exploring national parks.",
        "created_at": "2025-02-11T06:35:50+00:00",
        "updated_at": "2025-02-11T06:35:50+00:00"
    }
}
```

<br />

#### API Response Example: Business Customer

The endpoint will return a response as shown below for business customers. Note that **verification_link** is returned instead of `kyc_link` for corporate onboarding.

```json JSON
{
    "data": {
        "uuid": "{{customer_uuid}}",
        "status": "deactivated",
        "type": "business",
        "company_name": "Acme Corporation",
        "email": "contact@acme.com",
        "phone_country_code": "US",
        "phone_number": "555-555-9999",
        "has_signed_agreement": false,
        "agreement_link": "https://harbor-sandbox.owlpay.com/agreement_link",
        "verification_link": "https://harbor-sandbox.owlpay.com/verification_link",
        "description": "A global provider of widgets and tech solutions.",
        "created_at": "2025-02-11T06:35:50+00:00",
        "updated_at": "2025-02-11T06:35:50+00:00"
    }
}
```

<br />

### Agreement Link

After creating a **Customer**, **Application** must provide the **Customer's Agreement Link** and **KYC Link** / **Verification Link** so that the **Customer** can agree to our terms and get verified.

<br />

### KYC

Once a **Customer** submits their **KYC data**, the **Application** can refer to the following status flow diagram to understand the current review status. For a detailed explanation of each status, please refer to [Status](doc:status-definitions) .

<Image align="center" border={true} caption="Customer KYC status change diagram" src="https://files.readme.io/6d124f9016e0d0131981a451bccb9a67c7b1a8a816ca1a3965442d29be1a2fa2-Customer_Status.jpg" width="200px" />
