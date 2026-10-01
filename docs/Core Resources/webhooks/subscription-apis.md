---
title: Subscription APIs
excerpt: >-
  A webhook subscription is a mechanism that allows one system (the destination)
  to receive automated, real-time HTTP notifications from another system (the
  source) about specific events. The destination system "subscribes" to
  particular events, and the source system sends an HTTP request (the webhook)
  to a predefined callback URL whenever those events occur.
content:
  excerpt: >-
    A webhook subscription is a mechanism that allows one system (the
    destination) to receive automated, real-time HTTP notifications from another
    system (the source) about specific events. The destination system
    "subscribes" to particular events, and the source system sends an HTTP
    request (the webhook) to a predefined callback URL whenever those events
    occur.
hidden: false
metadata:
  robots: index
privacy:
  view: public
---
This guide provides a comprehensive overview and API reference for managing webhook subscriptions. Developers can use the provided API endpoints to:

* [List all notification subscriptions:](doc:subscription-apis#list-all-notification-subscriptions) Retrieve a list of all active webhook subscriptions, including their unique identifiers, endpoints, enabled status, and the types of notifications they receive.
* [Create a webhook subscription](doc:subscription-apis#create-a-webhook-subscription): Establish new webhook subscriptions by specifying an endpoint URL and the desired notification types (e.g., all events denoted by *).
* [Get a notification subscription](doc:subscription-apis#get-a-notification-subscription): Fetch detailed information about a specific webhook subscription using its unique identifier.
* [Update a notification subscription](doc:subscription-apis#update-a-notification-subscription): Modify existing webhook subscriptions, such as changing their name or enabling/disabling them.
* [Delete a notification subscription](doc:subscription-apis#delete-a-notification-subscription): Remove a webhook subscription using its unique identifier.

<Callout icon="👉" theme="info">

**Direct API Reference:**
* [API Reference: Webhook Subscriptions](doc:createawebhooksubscription)

</Callout>

<br />

***

### List all notification subscriptions

Retrieve a list of all active webhook subscriptions.

* HTTP Method: GET
* URL: /api/v1/notifications/subscriptions
* Request Example:

```curl
curl --location 'https://harbor.owlpay.com/api/v1/notifications/subscriptions' \
--header 'X-API-KEY: ••••••'
```

* Response Example:

```json
{
    "data": [
        {
            "uuid": "sub_ade75cbc-46f3-4c87-902a-c6596a088968",
            "name": "Transactions Webhook",
            "endpoint": "https://example.com/webhook",
            "enabled": true,
            "notification_types": [
                "*"
            ],
            "restricted": false,
            "created_at": "2025-08-28T08:34:33.000000Z",
            "updated_at": "2025-08-28T08:34:33.000000Z"
        },
        {
            "uuid": "sub_ffc519bb-4819-48de-8483-441b5472bfc6",
            "name": "Transactions Webhook",
            "endpoint": "https://example.com/webhook",
            "enabled": true,
            "notification_types": [
                "*"
            ],
            "restricted": false,
            "created_at": "2025-08-28T08:36:17.000000Z",
            "updated_at": "2025-08-28T08:36:17.000000Z"
        },
        {
            "uuid": "sub_951ca83b-1330-4b7b-921a-a1c02f1345b5",
            "name": "Transactions Webhook",
            "endpoint": "https://example.com/webhook",
            "enabled": true,
            "notification_types": [
                "*"
            ],
            "restricted": false,
            "created_at": "2025-08-28T08:37:21.000000Z",
            "updated_at": "2025-08-28T08:37:21.000000Z"
        }
    ],
    "links": {
        "first": "http://harbor.owlpay.com/api/v1/notifications/subscriptions?page=1",
        "last": null,
        "prev": null,
        "next": null
    },
    "meta": {
        "current_page": 1,
        "current_page_url": "http://harbor.owlpay.com/api/v1/notifications/subscriptions?page=1",
        "from": 1,
        "path": "http://harbor.owlpay.com/api/v1/notifications/subscriptions",
        "per_page": 15,
        "to": 4
    }
}
```

<br />

### Create a webhook subscription

Establish a new webhook subscription by specifying an endpoint URL and the desired notification types.

* HTTP Method: POST
* URL: /api/v1/notifications/subscriptions
* Request Example:

```curl
curl --location 'https://harbor.owlpay.com/api/v1/notifications/subscriptions' \
--header 'Accept: application/json' \
--header 'Content-Type: application/json' \
--header 'X-API-KEY: ••••••' \
--data '{
    "endpoint" : "https://yourserver.com/webhook",
    "notification_types" :["*"]
}'
```

* Response Example:

```json
{
    "data": {
        "uuid": "sub_951ca83b-1330-4b7b-921a-a1c02f1345b5",
        "name": "Transactions Webhook",
        "endpoint": "https://yourserver.com/webhook",
        "enabled": true,
        "notification_types": ["*"],
        "restricted": false,
        "created_at": "2025-08-28T08:37:21.000000Z",
        "updated_at": "2025-08-28T08:37:21.000000Z"
    }
}
```

<br />

### Get a notification subscription

Fetch detailed information about a specific webhook subscription using its unique identifier.

* HTTP Method: GET
* URL: /api/v1/notifications/subscriptions/:webhookSubscription_uuid
* Request Example:

```curl
curl --location 'https://harbor.owlpay.com/api/v1/notifications/subscriptions/sub_951ca83b-1330-4b7b-921a-a1c02f1345b5' \
--header 'X-API-KEY: ••••••'
```

* Response Example:

```json
{
    "data": {
        "uuid": "sub_951ca83b-1330-4b7b-921a-a1c02f1345b5",
        "name": "Transactions Webhook",
        "endpoint": "https://yourserver.com/webhook",
        "enabled": true,
        "notification_types": ["*"],
        "restricted": false,
        "created_at": "2025-08-28T08:37:21.000000Z",
        "updated_at": "2025-08-28T08:37:21.000000Z"
    }
}
```

<br />

### Delete a notification subscription

Remove a webhook subscription using its unique identifier.

* HTTP Method: DELETE
* URL: /api/v1/notifications/subscriptions/:webhookSubscription_uuid
* Request Example:

```curl
curl --location --request DELETE 'https://harbor.owlpay.com/api/v1/notifications/subscriptions/sub_951ca83b-1330-4b7b-921a-a1c02f1345b5' \
--header 'Accept: application/json' \
--header 'Content-Type: application/json' \
--header 'X-API-KEY: ••••••' \
--data ''
```

* Response Example:

```json
{
    "message": "The webhook subscription was deleted successfully.",
    "data": null
}
```

<br />

<br />
