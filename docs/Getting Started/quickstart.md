---
title: Quickstart
excerpt: This guide provides an introduction to getting started with the Harbor API, including how to authenticate requests, create customers, link blockchain addresses or bank accounts, and make API calls with the required headers and parameters.
x-content:
  excerpt: This guide provides an introduction to getting started with the Harbor API, including how to authenticate requests, create customers, link blockchain addresses or bank accounts, and make API calls with the required headers and parameters.
hidden: false
metadata:
  robots: index
x-privacy:
  view: public
---

### Introduction

This guide walks you through the same steps as [**Harbor Basics**](/docs/harbor-basics), from a developer's point of view: creating a Customer, onboarding them, and making your first transfer using the Harbor API.

In this example, you'll:

* Create an **individual** Customer.
* Take the Customer through onboarding.
* Make an **off-ramp** transfer, converting a stablecoin such as USDC into fiat currency and sending it to a bank account.



### Application

This guide assumes that you already have a **Sandbox API key** for your application and that you can call the Harbor API endpoints. If you don't have an API key yet, see [**Authentication**](/docs/authentication).

All requests use the Sandbox base URL, `https://harbor-sandbox.owlpay.com`, and must include your API key in the `X-API-KEY` header.

For testing and coding along, we recommend using [Postman](https://www.postman.com/downloads/).

### Customers


When creating a **Customer** object, create one for each individual or business you will make transfers for. Set the `type` field to either `individual` or `business`.


#### Creating a customer

In this example, we’ll create an individual Customer named John Michael Doe.


```shell
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

The endpoint returns the newly created Customer. These fields matter for the next steps:



```json
{
    "data": {
        "uuid": "{{customer_uuid}}",
        "object": "customer",
        "status": "deactivated",
        "state": "onboarding_needed",
        "type": "individual",
        "first_name": "John",
        "middle_name": "Michael",
        "last_name": "Doe",
        "email": "john.doe@example.com",
        "phone_country_code": "US",
        "phone_number": "555-555-1234",
        "birth_date": "1988-04-15",
        "is_bank_account_linkable": false,
        "is_card_linkable": false,
        "description": "A freelance writer who loves exploring national parks.",
        "intermediary_fi_reference": null,
        "created_at": "2025-05-22T07:26:12+00:00",
        "updated_at": "2025-05-22T07:26:12+00:00",
        "has_signed_agreement": false,
        "agreement_link": "https://harbor-sandbox.owlpay.com/agreement_link",
        "kyc_link": "https://harbor-sandbox.owlpay.com/gokyc"
    }
}
```

* `uuid`: the Customer's unique ID. It starts with `cus_`, the prefix for Customer objects. Every Harbor object ID starts with a prefix for its type, such as `transfer_` for transfers.
* `agreement_link`: the link the Customer uses to accept Harbor's service agreement (Onboarding, Step 1).
* `kyc_link`: the link the Customer uses to submit their verification information (Onboarding, Step 2).

Save the Customer's `uuid` from the response. You'll pass it in later requests for this Customer. 

Note that `state` is `onboarding_needed`: the Customer can't transact until onboarding is complete.

### Onboarding

Before you can create transfers for a Customer, the Customer must complete **Onboarding**. Onboarding has two main steps, and the Customer response above already includes the link for each.

**Step 1: Accept the agreement**

Send the Customer the `agreement_link` from the response so they can review and accept the service agreement. The `has_signed_agreement` field shows whether they have accepted it.

**Step 2: Provide information**

Next, send the Customer the `kyc_link`. The Customer uses it to submit the information Harbor needs for KYC (individuals) or KYB (businesses) verification.

<Accordion title="Can I submit customer information through the API?" icon="fa-solid fa-message-question">
  Yes. Instead of the hosted `kyc_link` form, you can submit the Customer's information through the Harbor API. See [**Onboard via API**](/docs/via-api).
</Accordion>

<br />

After the Customer submits their information, Harbor reviews it and updates the Customer's status. The diagram below shows the possible status changes.

<Image align="center" border={true} caption="Customer KYC status change diagram" src="https://files.readme.io/6d124f9016e0d0131981a451bccb9a67c7b1a8a816ca1a3965442d29be1a2fa2-Customer_Status.jpg" width="200px" />

For a detailed explanation of each status, see [**Status Definitions**](/docs/status-definitions).

### Transfers

Once the Customer is verified, you can create **transfers** for them. A transfer can be an on-ramp (fiat to stablecoin), an off-ramp (stablecoin to fiat), or an on-chain transfer (stablecoin to stablecoin).

Each transfer takes three API calls.

**Step 1: Create a quote**

Request a quote with the source, the destination, and the commission for the transfer. The response includes a `quote_id`. See [**Quotes**](/docs/quotes).

**Step 2: Obtain the transfer requirements**

Use the `quote_id` to fetch the transfer requirements. The response is a JSON Schema describing the exact payload the transfer needs. See [**Transfer JSON Schema**](/docs/transfer-json-schema).

**Step 3: Execute the transfer**

Create the transfer with the `quote_id` and a payload that matches the schema from Step 2.

For a complete walkthrough, including USD and non-USD local currency settlements such as HKD, SGD, MXN, and BRL, see [**Local Currency Transfers**](/docs/local-currency-transfers).
