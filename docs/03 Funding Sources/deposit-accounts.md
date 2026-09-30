---
title: Customer's Deposit Account (on-ramp)
excerpt: >-
  Learn how to provision dedicated USD bank accounts for automatic
  fiat-to-stablecoin on-ramp transfers.
content:
  excerpt: >-
    Learn how to provision dedicated USD bank accounts for automatic
    fiat-to-stablecoin on-ramp transfers.
hidden: false
metadata:
  robots: index
privacy:
  view: public
---
> 📘 Looking for Comparison?
>
> To understand the differences in usage and workflow between **Customer Deposit Accounts** and **Narrative-Based Transfers (On-Ramp)**, please refer to our comparison guide: [Funding Methods: Deposit Accounts vs. Narrative-Based Transfers](doc:deposit-accounts-vs-narrative-transfers).

The **Customer's Deposit Account** feature allows you to provision dedicated, virtual fiat bank accounts for your verified customers. This acts as an asynchronous **fiat-to-crypto on-ramp** service.

When a customer transfers fiat currency (via USD ACH Push or USD Wire Transfer) into their dedicated bank account, Harbor automatically:
1. Detects the incoming deposit.
2. Converts the fiat amount to cryptocurrency (USDC) minus applicable fees.
3. Transfers the stablecoin directly to the specified `destination` (either an external blockchain address or a Harbor wallet).

<Callout icon="👉" theme="info">

**Direct API Reference:**
* [API Reference: Create a Deposit Account](doc:createadepositaccount)

</Callout>

---

### 💡 Use Cases

Here are some of the key use cases for Customer's Deposit Account:

* **Automated B2B Remittance & Pay-in**: Enable your business clients or merchants in the United States to settle payments in traditional fiat. Payer deposits funds into the dedicated bank account, and the platform automatically receives USDC on-chain.
* **Automated Payroll & Regular Disbursements**: Perfect for companies that need to make regular USDC payments to employees. By setting up a dedicated deposit account for each employee, the company simply transfers traditional fiat (wire/ACH) to the respective bank account. OwlPay then automatically converts the fiat into USDC and deposits it directly into the employee's designated wallet.
* **Recurring Wallet Funding**: Provide your users with their own persistent bank routing and account numbers. Users can configure recurring transfers from their traditional banking apps, automatically funding their web3 wallets with USDC.
* **Seamless Brokerage & Exchanges**: Onboard users into decentralized platforms. Users send fiat from their external bank accounts, and it instantly becomes liquid USDC in their trading account without having to go through a manual checkout flow.

---

### 🔄 How It Works (The Lifecycle)

The full integration lifecycle follows five simple phases:

1. **Provisioning (`provisioning`)**:
   When you create a deposit account, the API asynchronously provisions a dedicated bank account with the banking partner. The account starts in the `provisioning` status, and `deposit_instructions` is `null`.
2. **Active (`active`)**:
   Once provisioning completes, the account status flips to `active`, and `deposit_instructions` populated with the routing number, bank name, bank address, account number, and holder name. At this point, Harbor delivers `deposit_account.activated`, carrying those instructions.
   Provisioning can also fail. The account never reaches `active` and `deposit_account.failed` is delivered instead, so do not read "no activation yet" as "still provisioning" indefinitely — subscribe to both events.
3. **Incoming Deposits**:
   Your end-customer sends traditional fiat (US Dollar via WIRE or ACH Push) to the routing and account number provided in the instructions.
4. **Auto-Conversion & Settlement**:
   Upon receipt, Harbor converts the USD to USDC (minus applicable commissions/fees) and transfers it to the defined `destination`.
5. **Modification or Termination**:
   You can retarget the destination address as long as the account status is `active`. You can also permanently disable the deposit account. Disabling deletes the underlying bank account and is completely irreversible.

---

### 🛑 Key Requirements & Compliance (Travel Rule)

Because this feature transfers fiat to crypto on behalf of customers, the **Travel Rule** and compliance information must be supplied at creation time under the `destination` block.

This includes:
* **Beneficiary Info**: Legal name, date of birth, ID document number, and residential address of the beneficiary.
* **Transfer Purpose**: A standard enum detailing the purpose (e.g., `SALARY`, `FAMILY_MAINTENANCE`, `PRODUCT_INDEMNITY_INSURANCE`, etc.).
* **Wallet Type & Institution**: Flagging whether the wallet is custodial/non-custodial and specifying the receiving wallet provider (e.g., self-hosted, Coinbase, etc.).

---

### 💻 Step-by-Step API Walkthrough

#### Step 1: Create a Customer
Before creating a deposit account, you must have a verified customer in the system. If you haven't created one, refer to [Create a Customer](doc:customer).

#### Step 2: Create a Deposit Account
Create a dedicated deposit account for the customer. Currently `source_country` must be `US`. `destination.asset` is optional and defaults to `USDC`, which is the only asset supported today.

> 💡 **Commission Fee Support**
> 
> Deposit Accounts support setting a **Commission Fee** (which will be deducted automatically from the incoming deposit during auto-conversion and settlement). 
> For details on how commissions are calculated, what parameters are used, and the differences between Source and Destination Amount models, please refer to the [Commission Fee](doc:commission-fees) guide.

To configure a commission fee on creation, include the `commission` object in the payload as shown below:

```curl curl
curl --location --request POST 'https://harbor-sandbox.owlpay.com/api/v1/customers/cus_1234567890/deposit_accounts' \
--header 'Content-Type: application/json' \
--header 'Accept: application/json' \
--header 'X-API-KEY: {YOUR_API_KEY}' \
--header 'X-Idempotency-Key: {YOUR_IDEMPOTENCY_KEY}' \
--data-raw '{
  "source_country": "US",
  "commission": {
    "percentage": 0.5,
    "amount": 25.00
  },
  "destination": {
    "type": "external_address",
    "asset": "USDC",
    "chain": "ethereum",
    "address": "0x1234567890abcdef1234567890abcdef12345678",
    "is_self_transfer": true,
    "transfer_purpose": "SALARY",
    "beneficiary_receiving_wallet_type": "non_custodial",
    "beneficiary_institution_name": "MyPersonalWallet",
    "beneficiary_info": {
      "beneficiary_name": "John Doe",
      "beneficiary_dob": "1990-01-01",
      "beneficiary_id_doc_number": "12345678",
      "beneficiary_address": {
        "street": "123 4th Ave S",
        "city": "Minneapolis",
        "state_province": "MN",
        "postal_code": "55416",
        "country": "US"
      }
    }
  },
  "application_reference_id": "ref-0001"
}'
```

Response (HTTP 202 Accepted):
```json
{
  "data": {
    "id": "depacc_1234567890",
    "object": "customer_deposit_account",
    "customer_id": "cus_1234567890",
    "source_country": "US",
    "status": "provisioning",
    "destination": {
      "type": "external_address",
      "asset": "USDC",
      "chain": "ethereum",
      "address": "0x1234567890abcdef1234567890abcdef12345678",
      "memo": null,
      "beneficiary_info": {
        "beneficiary_name": "John Doe",
        "beneficiary_dob": "1990-01-01",
        "beneficiary_id_doc_number": "12345678",
        "beneficiary_address": {
          "street": "123 4th Ave S",
          "city": "Minneapolis",
          "state_province": "MN",
          "postal_code": "55416",
          "country": "US"
        }
      },
      "transfer_purpose": "SALARY",
      "is_self_transfer": true,
      "beneficiary_receiving_wallet_type": "non_custodial",
      "beneficiary_institution_name": "MyPersonalWallet"
    },
    "deposit_instructions": null,
    "commission": {
      "percentage": 0.5,
      "amount": 25
    },
    "application_reference_id": "ref-0001",
    "created_at": "2026-06-10T00:00:00+00:00",
    "updated_at": "2026-06-10T00:00:00+00:00"
  }
}
```

#### Step 3: Receive Deposit Instructions (via Webhook or Polling)
Since the deposit account is provisioned asynchronously, you will receive real-time notifications by subscribing to `deposit_account.activated` and `deposit_account.failed`, or to the `deposit_account.*` wildcard (refer to the [Webhook Subscriptions](doc:webhooks-overview) guide).

Alternatively, you can poll the GET endpoint to retrieve the status:

```curl curl
curl --location --request GET 'https://harbor-sandbox.owlpay.com/api/v1/deposit_accounts/depacc_1234567890' \
--header 'Accept: application/json' \
--header 'X-API-KEY: {YOUR_API_KEY}'
```

Once active, the `deposit_instructions` field is populated with banking details:
```json
{
  "data": {
    "id": "depacc_1234567890",
    "object": "customer_deposit_account",
    "customer_id": "cus_1234567890",
    "source_country": "US",
    "status": "active",
    "destination": {
      "type": "external_address",
      "asset": "USDC",
      "chain": "ethereum",
      "address": "0x1234567890abcdef1234567890abcdef12345678"
    },
    "deposit_instructions": {
      "account_number": "9801234567",
      "routing_number": "021214891",
      "bank_name": "Cross River Bank",
      "bank_address": "2115 Linwood Avenue, Fort Lee, NJ 07024",
      "account_holder_name": "OwlTing Inc. FBO John Doe"
    },
    "commission": {
      "percentage": 0.5,
      "amount": 25
    },
    "application_reference_id": "ref-0001",
    "created_at": "2026-06-10T00:00:00+00:00",
    "updated_at": "2026-06-10T00:00:00+00:00"
  }
}
```

> 📘 **Account-verification micro-deposits**
> An outside bank or platform may push a few cents into this account to prove that it exists and belongs to the named holder. Those deposits are **not** on-ramped: no transfer is created, no conversion takes place, no hold is placed, and your balance does not move. You are notified through the [`bank_account.micro_deposit.received`](https://harbor-developers.owlpay.com/docs/micro-deposit-verification#/) webhook instead. Harbor also emails a notification about the deposit — to the Customer holding the account, with your business owner copied.

#### Step 4: Retarget or Disable the Account
If you need to change where the funds are routed (only allowed for `active` status), or permanently close the bank account:

```curl curl
curl --location --request PATCH 'https://harbor-sandbox.owlpay.com/api/v1/deposit_accounts/depacc_1234567890' \
--header 'Content-Type: application/json' \
--header 'Accept: application/json' \
--header 'X-API-KEY: {YOUR_API_KEY}' \
--data-raw '{
  "destination": {
    "type": "external_address",
    "asset": "USDC",
    "chain": "arbitrum",
    "address": "0x567890abcdef1234567890abcdef1234567890ab",
    "is_self_transfer": true,
    "transfer_purpose": "SALARY",
    "beneficiary_receiving_wallet_type": "non_custodial",
    "beneficiary_institution_name": "MyArbitrumWallet",
    "beneficiary_info": {
      "beneficiary_name": "John Doe",
      "beneficiary_dob": "1990-01-01",
      "beneficiary_id_doc_number": "12345678",
      "beneficiary_address": {
        "street": "123 4th Ave S",
        "city": "Minneapolis",
        "state_province": "MN",
        "postal_code": "55416",
        "country": "US"
      }
    }
  }
}'
```

To permanently close the bank account (irreversible):
```curl curl
curl --location --request PATCH 'https://harbor-sandbox.owlpay.com/api/v1/deposit_accounts/depacc_1234567890' \
--header 'Content-Type: application/json' \
--header 'Accept: application/json' \
--header 'X-API-KEY: {YOUR_API_KEY}' \
--data-raw '{
  "status": "disabled"
}'
```

> 🚧 Disabling is Permanent
>
> Once disabled, the account **cannot be re-enabled or retargeted**. This physically deletes the virtual banking link at our provider. To resume, you must create a new deposit account.

---

#### Step 5: Simulate Deposit (Sandbox Only)
To test the automatic settlement flow in the Sandbox environment, simulate a payment to your deposit account using the simulation API:

```curl curl
curl --location --request POST 'https://harbor-sandbox.owlpay.com/api/v1/deposit_accounts/depacc_1234567890/simulate_payment' \
--header 'Content-Type: application/json' \
--header 'Accept: application/json' \
--header 'X-API-KEY: {YOUR_API_KEY}' \
--data-raw '{
  "amount": 1000,
  "method": "WIRE",
  "sender_name": "John Doe"
}'
```

Response (HTTP 202):
```json
{
  "data": {
    "simulated": true,
    "rail_payment_id": "sim_9f2c1d7a",
    "method": "WIRE"
  }
}
```

This triggers the downstream flow: receiving the fiat, initiating a transfer, converting to USDC, and delivering to the defined destination address. You can trace this order status via transfer status hooks and standard transaction logs.

> 🚧 Planned for release in October 2026
>
> An `ACH_PUSH` of less than US$1.00 does not simulate a deposit. It is classified as an account-verification micro-deposit, exactly as production would classify it, and delivers [`bank_account.micro_deposit.received`](https://harbor-developers.owlpay.com/docs/micro-deposit-verification#/) instead of creating a transfer. The response reports which path was taken in `classified_as`.

---

### 📋 Listing Deposit Accounts

You can list all deposit accounts associated with a specific customer:

```curl curl
curl --location --request GET 'https://harbor-sandbox.owlpay.com/api/v1/customers/cus_1234567890/deposit_accounts?per_page=15' \
--header 'Accept: application/json' \
--header 'X-API-KEY: {YOUR_API_KEY}'
```

This returns a paginated list of accounts, allowing you to manage and audit provisioned accounts easily.
