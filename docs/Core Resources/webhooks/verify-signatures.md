---
title: Verify Signatures
hidden: false
metadata:
  robots: index
x-privacy:
  view: public
---
The `harbor-signature` header included in each signed webhook event contains a **timestamp** and **signature**. The timestamp is prefixed by `t=`, and the signature is prefixed by a scheme. The scheme begins with `v`.

### Example `harbor-signature` Header

```
harbor-signature:  
t=1689066169,
v1=e997b87453fb8923ae9c02faf45fe5fff60148d7a244896e0b359c96aa4825a0
```

> *Note*: For clarity, newlines are added in the example, but the `harbor-signature` header in an actual request is a single line.

Harbor generates signatures using **HMAC with SHA-256**. While we will provide official libraries for signature verification in the future, you can create a custom solution by following these steps:

***

### Steps to Verify the Webhook Signature

#### Step 1: Extract the Timestamp and Signature from the Header

Split the `harbor-signature` header using the `,` character to separate elements. Then split each element using the `=` character to extract the prefix and value pair.

* The value for the prefix `t` corresponds to the **timestamp**.
* The value for the prefix `v1` corresponds to the **signature**.

#### Step 2: Prepare the `signed_payload` String

To create the `signed_payload` string, concatenate the following:

1. The **timestamp** (as a string).
2. The character `.`.
3. The actual JSON payload (i.e., the request body).

#### Step 3: Compute the Expected Signature

Using the HMAC-SHA256 hash function:

* Use the **signing secret** as the key.
* Use the `signed_payload` string as the message.

#### Step 4: Compare the Signatures

Compare the computed signature with the one provided in the header (`v1`). If they match, the webhook signature is valid.

***

### Node.js Code Example

Below is an example of how to implement this process in Node.js:

```javascript
const crypto = require('crypto');

// Simulate the incoming harbor-signature header
const requestHeader = 't=1689066169,v1=e997b87453fb8923ae9c02faf45fe5fff60148d7a244896e0b359c96aa4825a0';

// Split the header into parts
const elements = requestHeader.split(',');
const signatureData = elements.reduce((acc, item) => {
  const [key, value] = item.split('=');
  acc[key] = value;
  return acc;
}, {});

// Extract the timestamp and signature
const timestamp = signatureData['t'];
const signature = signatureData['v1'];

// The JSON payload (request body)
const requestBody = '{
    "status": "completed",
    "source": {
        "asset": "USDC",
        "amount": "1000.00",
    },
    "destination": {
        "asset": "USD",
        "account_number": "123456789012",
        "routing_number": "021000021",
        "bank_name": "Test Bank USA",
        "bank_address": "123 Test Street, New York, NY 10001, USA",
        "account_holder_name": "John Doe",
        "amount": "980.00",
    },
    "application_transfer_uuid": "{{YOUR_APPLICATION_TRANSFER_UUID}}",
    "transfer_instructions": {
        "chain": "ethereum",
        "address": "0x6990E7E90ab50C12111f99b84183D3fE298Bb3e4"
    },
    "commission": {
        "rate": "0.01",
        "price": 1
    },
    "receipt": {
      "initial_amount": "1000.00",
      "commission_fee": "20.00",
      "harbor_fee": "5.00",
      "final_amount": "975.00",
      "tracking_number": '92711234310210', // This field will be filled if the transfer status is `bank_in_process`
    },
    "created_at": "2025-02-07T13:05:32+00:00",
    "updated_at": "2025-02-07T13:05:32+00:00"
}';

// Harbor signing secret
const signingSecret = 'whs_xxxxxxx';

// Create the signed payload string
const signedPayload = `${timestamp}.${requestBody}`;

// Generate the expected HMAC signature
const expectedSignature = crypto
  .createHmac('sha256', signingSecret)
  .update(signedPayload)
  .digest('hex');

// Verify if the signature matches
if (expectedSignature === signature) {
  console.log('Signature verification passed.');
} else {
  console.log('Signature verification failed.');
}
```

<br />

When executing a ***Transfer*** for an e-commerce platform or similar use case, we strongly recommend verifying the harbor-signature to ensure the request originates from Harbor. This step helps maintain security before delivering goods to the consumer.
