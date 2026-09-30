---
title: Onboarding Meta APIs
excerpt: >-
  Reference lookup endpoints for dynamic validation lists used during customer
  onboarding (individual & business).
content:
  excerpt: >-
    Reference lookup endpoints for dynamic validation lists used during customer
    onboarding (individual & business).
hidden: false
metadata:
  robots: index
privacy:
  view: public
---
When collecting customer onboarding data, several fields require specific, dynamically changing values (such as occupations, industries, states, or required document parts). 

Before presenting your UI or submitting the onboarding payload, you must use these lookup endpoints to populate selection fields and validate user input.

***

### 1. Individual Onboarding Lookups

Depending on whether your customer is using the **Standard Onboarding (v1)** or **Graded Onboarding (v2)** scheme, call the corresponding lookup APIs.

#### 1.1 Standard Onboarding (v1) Lookups

| Endpoint | Method | Scope | Purpose |
| :--- | :--- | :--- | :--- |
| `/api/v1/occupations` | `GET` | Global | Fetch valid occupation slugs. |
| `/api/v1/countries/{country}/subdivisions` | `GET` | Per-country | Fetch state subdivision codes (e.g., `VA`, `WA`, `ENG`). |

#### 1.2 Graded Onboarding (v2) Lookups

| Endpoint | Method | Scope | Purpose |
| :--- | :--- | :--- | :--- |
| `/api/v2/customers/individual/occupations` | `GET` | Global | Fetch valid occupation slugs. |
| `/api/v2/customers/individual/countries/{country}/subdivisions` | `GET` | Per-country | Fetch state subdivision codes (e.g., `VA`, `WA`, `ENG`). |
| `/api/v2/customers/individual/countries/{country}/identity_documents` | `GET` | Per-country | Fetch valid document types, required parts, and if back side is required. |

##### V2 Identity Document Lookup Response Example
Calling `GET /api/v2/customers/individual/countries/US/identity_documents` returns:
```json
{
  "data": [
    {
      "type": "PASSPORT",
      "required_parts": ["FRONT"],
      "back_required": false
    },
    {
      "type": "DRIVER_LICENCE",
      "required_parts": ["FRONT", "BACK"],
      "back_required": true
    }
  ]
}
```

***

### 2. Business Onboarding Lookups

These lookup endpoints are used when collecting data for corporate (KYB) onboarding.

| Endpoint | Method | Scope | Purpose |
| :--- | :--- | :--- | :--- |
| `/api/v1/countries/{country}/industries` | `GET` | Per-country | Fetch valid industry slugs for the country of incorporation. |
| `/api/v1/job-titles` | `GET` | Global | Fetch valid titles/positions for associated persons (e.g., `director`, `representative`). |
| `/api/v1/countries/{country}/subdivisions` | `GET` | Per-country | Fetch state subdivision codes (e.g., `VA`, `WA`). |
