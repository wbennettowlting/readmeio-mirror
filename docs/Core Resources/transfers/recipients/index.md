---
title: Recipients
excerpt: Pre-fill and manage reusable payee destinations for frictionless transfers.
x-content:
  excerpt: Pre-fill and manage reusable payee destinations for frictionless transfers.
hidden: false
metadata:
  robots: index
x-privacy:
  view: anyone_with_link
---
The **Application Recipients API** (also known as the **Recipient Book**) allows you to create, manage, and reuse pre-filled payee destination profiles across your application.

By saving beneficiary and payout information in advance, you can initiate cross-border and local payouts using the **Transfer API** by simply referencing a `recipient_name`—eliminating the need to repeatedly construct complex `destination` payloads for every transfer.

***

### Key Benefits

* **Frictionless Payouts**: Initiate transfers with a single `recipient_name` handle instead of specifying full bank or wallet details every time.
* **Error Reduction**: Prevent typos in complex payout fields (e.g. IBANs, routing numbers, account numbers, beneficiary addresses, or tax IDs).
* **Centralized Profile Management**: Easily manage, update, or remove recipient profiles via API endpoints or through the Harbor Portal.

***

### How Recipients Work with the Transfer API

When creating a recipient profile, you define a user-friendly `name` and a `payload` containing pre-filled destination fields mapped in flat dot-notation (e.g. `destination.beneficiary_info.beneficiary_name`).

When initiating a payout via `POST /api/v2/transfers` (Create Transfer (V2):

1. **Pre-filling**: Include the `"recipient_name": "<name>"` parameter in your request body.
2. **Payload Expansion**: Harbor looks up the saved recipient for your application, expands the dot-notation keys into a structured `destination` object, and injects them into the transfer payload.
3. **Field Overriding (**`array_replace_recursive`**)**: Any destination fields explicitly included in your `POST /api/v2/transfers` request body will automatically override the pre-filled values stored in the recipient record.

<Callout icon="📘" theme="info">
  ### Precedence & Overriding

  Explicit fields supplied directly in the `destination` object of your Transfer API request will always take precedence over the pre-filled values in the saved recipient record.
</Callout>

***

### Step-by-Step Workflow

#### Step 1: Create an Application Recipient

Store a payee's information in your application's recipient book using dot-notation field paths under `payload`.

**Endpoint:** `POST /api/v1/applications/recipients`

```json Request Body
{
  "name": "John Smith Wire",
  "payload": {
    "destination.beneficiary_info.beneficiary_name": "John Smith",
    "destination.payout_instrument.account_number": "1234567890",
    "destination.payout_instrument.routing_number": "021000021",
    "destination.transfer_purpose": "SALARY",
    "destination.is_self_transfer": false
  }
}
```

```json Response (201 Created)
{
  "data": {
    "object": "recipient",
    "uuid": "rcpt_0123456789abcdefghij0123456789abcdefghij",
    "name": "John Smith Wire",
    "payload": {
      "destination.beneficiary_info.beneficiary_name": "John Smith",
      "destination.payout_instrument.account_number": "1234567890",
      "destination.payout_instrument.routing_number": "021000021",
      "destination.transfer_purpose": "SALARY",
      "destination.is_self_transfer": false
    },
    "created_at": "2026-06-10T00:00:00+00:00",
    "updated_at": "2026-06-10T00:00:00+00:00"
  }
}
```

***

#### Step 2: Create a Transfer referencing `recipient_name`

When you create a transfer, pass `recipient_id` , the UUID (prefixed `rcpt_` )you got back when you created the recipient. Harbor uses it to fill in the transfer's destination fields from that saved recipient. If you need to change something just for this transfer, you can still pass that field directly in destination and it'll override the saved value.

**Endpoint:** `POST /api/v2/transfers`

```json Request Body
{
  "on_behalf_of": "cus_baWXWeq34SRWe6rbqHIh7iCrGkFXxU7eP9XiEoDA",
  "quote_id": "quote_1234567890abcdef",
  "application_transfer_uuid": "ORD-20260804-001",
  "recipient_id": "rcpt_0123456789abcdefghij0123456789abcdefghij",
  "destination": {
    "transfer_purpose": "CONSULTING_FEE"
  }
}
```

In this example, the beneficiary name, account number, routing number, and self-transfer flag are populated from the recipient created above&#x20;

`"name": "John Smith Wire", "uuid": "rcpt_0123456789abcdefghij0123456789abcdefghij"`

while `transfer_purpose` is overridden to `CONSULTING_FEE` for this transfer.

***

### Managing Recipients

You can list, inspect, update, or delete application-level recipients using the following endpoints:

| Action               | HTTP Method | Endpoint                                         | Description                                                                  |
| :------------------- | :---------- | :----------------------------------------------- | :--------------------------------------------------------------------------- |
| **List Recipients**  | `GET`       | `/api/v1/applications/recipients`                | Retrieve all saved recipients for your application.                          |
| **Create Recipient** | `POST`      | `/api/v1/applications/recipients`                | Save a new recipient entry with a user-friendly name and pre-filled payload. |
| **Get Recipient**    | `GET`       | `/api/v1/applications/recipients/{recipient_id}` | Retrieve details of a specific recipient by id.                              |
| **Update Recipient** | `PUT`       | `/api/v1/applications/recipients/{recipient_id}` | Update an existing recipient's  or pre-filled payload.                       |
| **Delete Recipient** | `DELETE`    | `/api/v1/applications/recipients/{recipient_id}` | Remove a recipient entry from your address book.                             |

<Callout icon="ℹ️" theme="info">
  ### Portal Management

  Recipients can also be managed visually by logging into the **Harbor Portal** under your application settings.
</Callout>