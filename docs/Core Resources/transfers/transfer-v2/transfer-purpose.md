---
title: Transfer Purpose
hidden: false
metadata:
  robots: index
privacy:
  view: public
---
### Overview

In **(V2) Transfer API**, the `transfer_purpose` field is **mandatory** under the `destination` object.

Unlike (V1), **(V2) only allows a fixed set of predefined values**.  
Free-text input is **not permitted**.

This design ensures:

* Stronger regulatory compliance
* Consistent reporting across jurisdictions
* Reduced transfer rejections and review delays
* Clear alignment with AML / CTF requirements

***

### Field Requirements

| Field              | Type   | Required | Description                               |
| ------------------ | ------ | -------- | ----------------------------------------- |
| `transfer_purpose` | string | Yes      | Must be one of the predefined enum values |

***

### Example Request (V2)

```bash
curl --location --request POST 'https://harbor-sandbox.owlpay.com/api/v2/transfer' \
--header 'Content-Type: application/json' \
--header 'Accept: application/json' \
--header 'X-API-KEY: {{API_KEY}}' \
--header 'X-Idempotency-Key: {{IDEMPOTENCY_KEY}}' \
--data-raw '{
  "...": "...",
  "destination": {
    "...": "...",
    "transfer_purpose": "SALARY"
  }
}'
```

***

### Allowed `transfer_purpose` Values (V2) Only)

The following enum **KEYS are strictly enforced** in (V2) Transfer.  
Requests containing values outside this list will be rejected.

#### Personal & Household

| Enum Key                  | Description                                             |
| ------------------------- | ------------------------------------------------------- |
| `TRANSFER_TO_OWN_ACCOUNT` | Transfers between accounts owned by the same individual |
| `FAMILY_MAINTENANCE`      | Financial support for family members                    |
| `EDUCATION`               | Tuition fees, school expenses, or education support     |
| `MEDICAL_TREATMENT`       | Hospital fees, medical services, or healthcare costs    |
| `HOTEL`                   | Hotel or lodging-related payments                       |
| `TRAVEL`                  | Travel-related expenses                                 |
| `REPAYMENT_OF_LOANS`      | Loan principal or interest repayment                    |
| `TAX_PAYMENT`             | Government or statutory tax payments                    |

***

#### Property & Insurance

| Enum Key                      | Description                                            |
| ----------------------------- | ------------------------------------------------------ |
| `PURCHASE_PROPERTY`           | Purchase of residential real estate                    |
| `PROPERTY_RENTAL`             | Rental payments for residential or commercial property |
| `INSURANCE_PREMIUM`           | Regular insurance premium payments                     |
| `PRODUCT_INDEMNITY_INSURANCE` | Insurance covering product liability                   |
| `INSURANCE_CLAIMS`            | Payouts related to insurance claims                    |

***

#### Investment & Income

| Enum Key                 | Description                              |
| ------------------------ | ---------------------------------------- |
| `MUTUAL_FUND_INVESTMENT` | Investment into mutual funds             |
| `INVESTMENT_SHARES`      | Purchase of stocks or equity instruments |
| `DONATIONS`              | Charitable or non-profit donations       |
| `SALARY`                 | Salary or wage payments                  |

***

#### Professional & Business Services

| Enum Key                  | Description                                         |
| ------------------------- | --------------------------------------------------- |
| `INFO_SERVICE`            | Payments for data, information, or digital services |
| `ADVERTISING`             | Marketing, advertising, or PR expenses              |
| `ROYALTY_FEES`            | IP-related royalty or licensing payments            |
| `BROKER_FEES`             | Brokerage and custodial service fees                |
| `ADVISOR_FEES`            | Professional consulting and advisory fees           |
| `REPRESENTATIVE_EXPENSES` | Costs related to representative or liaison offices  |

***

#### Trade & Logistics

| Enum Key                | Description                                     |
| ----------------------- | ----------------------------------------------- |
| `CONSTRUCTION`          | Construction or infrastructure-related payments |
| `TRANSPORTATION`        | Logistics and freight transportation costs      |
| `EXPORTED_GOODS`        | Payments related to exported goods              |
| `DELIVERY_FEES`         | Last-mile or delivery service fees              |
| `GENERAL_GOODS_OFFLINE` | Offline or traditional goods trading            |

<br />
