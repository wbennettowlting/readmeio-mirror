---
title: JSON Schema (Draft 2020-12)
hidden: false
metadata:
  robots: index
privacy:
  view: public
---
This endpoint returns a **route-specific** JSON Schema (Draft 2020-12) that describes the **exact request payload** required to create a v2 transfer for a given `quote_id`.

Use it to:

* **Dynamically render** required fields in your UI
* **Validate** transfer payloads before submission
* Ensure you meet **corridor + payment method + compliance** requirements

***

### Endpoint

Retrieve the JSON Schema requirements for creating a transfer:

```bash
curl --location 'https://harbor-sandbox.owlpay.com/api/v2/transfers/quotes/{{quote_id}}/requirements' \
--header 'Content-Type: application/json' \
--header 'Accept: application/json' \
--header 'x-api-key: {{API_KEY}}'
```

### Response

The response is a **JSON Schema** document compliant with **Draft 2020-12**.

Example (truncated):

```json
{
  "$schema": "https://json-schema.org/draft/2020-12/schema",
  "type": "object",
  "additionalProperties": false,
  "required": [
    "quote_id",
    "destination",
    "on_behalf_of",
    "application_transfer_uuid"
  ],
  "properties": {
    "on_behalf_of": { "type": "string" },
    "quote_id": { "type": "string" },
    "application_transfer_uuid": { "type": "string" },
    "destination": {
      "type": "object",
      "additionalProperties": false,
      "required": ["beneficiary_info", "payout_instrument"],
      "properties": {
        "beneficiary_info": { "...": "..." },
        "payout_instrument": { "...": "..." },
        "transfer_purpose": { "type": "string" },
        "is_self_transfer": { "type": "boolean" }
      }
    }
  },
  "title": "NG_BANK_TRANSFER",
  "$comment": "{\"bank_title\":\"NG_BANK_TRANSFER\",\"mapping_version\":1}"
}
```

***

### How to Read the Schema (Key Draft 2020-12 Concepts)

#### `$schema`

Indicates the schema dialect (Draft 2020-12):

* `"$schema": "https://json-schema.org/draft/2020-12/schema"`

#### `type`

Defines the JSON type allowed at this node.

Example:

* `"type": "object"` means this node must be a JSON object.

#### `required`

A list of **mandatory properties** that must exist in the object.

Example:

* The root requires: `quote_id`, `destination`, `on_behalf_of`, `application_transfer_uuid`

#### `properties`

Defines the allowed keys and their validation rules.

Example:

* `"application_transfer_uuid": { "type": "string" }`

#### `additionalProperties: false`

Strict mode:

* Any key **not declared** in `properties` will be rejected.
* This prevents accidental or unsupported fields from being sent.

#### Validation Keywords Commonly Used

Depending on the route, you may see:

* `minLength`, `maxLength`
* `pattern` (regex)
* `enum`
* `format` (tooling-dependent)
* `oneOf`, `anyOf`, `allOf`
* `if` / `then` / `else` (conditional requirements)

#### `title`

A human-readable identifier for the route requirement set.

Example:

* `"title": "NG_BANK_TRANSFER"`

#### `$comment`

Non-validating metadata.
Harbor may include internal mapping or version hints here.

Example:

* `{"bank_title":"NG_BANK_TRANSFER","mapping_version":1}`

***

### Practical Usage Pattern

#### 1) Get a Quote

Call the Quote API to obtain a `quote_id`.

#### 2) Get Requirements Schema

Use the endpoint in this document to retrieve the Draft 2020-12 JSON Schema for that `quote_id`.

#### 3) Validate Before Creating a Transfer

Validate your transfer payload locally before calling `POST /api/v2/transfers`.

This reduces:

* avoidable 4xx validation errors
* compliance-related missing fields
* mismatched corridor fields

***

### Example: Validating a Payload (JavaScript / Node.js with AJV)

> AJV supports Draft 2020-12 when configured with the appropriate package.

```js
import Ajv from "ajv/dist/2020.js";

const ajv = new Ajv({ allErrors: true, strict: false });

// schema = JSON you got from the requirements endpoint
// payload = the transfer request you will submit
const validate = ajv.compile(schema);

if (!validate(payload)) {
  console.log(validate.errors);
  throw new Error("Transfer payload validation failed.");
}
```

***

### Example: Validating a Payload (Python)

Python `jsonschema` supports Draft 2020-12 validators.

```py
from jsonschema import Draft202012Validator

validator = Draft202012Validator(schema)
errors = sorted(validator.iter_errors(payload), key=lambda e: e.path)

if errors:
    for e in errors:
        print(f"{list(e.path)}: {e.message}")
    raise Exception("Transfer payload validation failed.")
```

***

### Notes & Recommendations

* The schema is **quote-specific** and may change by:
  * destination country/currency
  * payment method type
  * use case (B2B/B2C/C2C/C2B)
