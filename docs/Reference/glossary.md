---
title: Glossary
excerpt: Plain-language definitions of the terms and acronyms used throughout the Harbor docs.
hidden: false
---
This page explains the terms and acronyms you'll come across in the Harbor docs. Terms are grouped by topic and listed alphabetically within each group.

### Harbor Concepts

The core building blocks of Harbor and the environments you work in.

| Term | Definition |
| :--- | :--- |
| **Application** | Your organization's workspace in Harbor. It holds the Customers you create and the transfers you make. See [**Harbor Basics**](/docs/harbor-basics). |
| **Customer** | An individual or business you make transfers for. Each Customer must complete onboarding before they can transact. |
| **Harbor Portal** | The web dashboard where you manage your application, API keys, and recipients without calling the API. |
| **Recipient** | The person or business that receives the funds of a transfer. Also called the **beneficiary**. See [**Recipients**](/docs/recipients). |
| **Sandbox** | Harbor's test environment. It uses testnet blockchains and test funds, so no real money is moved. |
| **Production** | Harbor's live environment, where transfers move real funds. You receive a Production API key once your integration is tested. |
| **Third-party recipient** | A recipient who is not the Customer sending the funds. See [**Third-Party Recipients**](/docs/third-party-recipients). |

### Compliance and Onboarding

Terms for how Harbor verifies Customers and keeps transfers compliant.

| Term | Definition |
| :--- | :--- |
| **AML** (Anti-Money Laundering) | Checks that detect and prevent money laundering. A transfer may be placed `on_hold` while AML checks run. See [**KYC/AML**](/docs/kyc-aml). |
| **Customer state** | A simplified, customer-facing version of the Customer `status`, such as `verified` or `update_required`. See [**Status Definitions**](/docs/status-definitions). |
| **Graded onboarding** | An onboarding scheme for individuals with three levels. Each level collects more information and unlocks higher limits. See [**Customer Levels**](/docs/customer-levels). |
| **KYB** (Know Your Business) | The verification process for business Customers, such as checking company registration and ownership. |
| **KYC** (Know Your Customer) | The verification process for individual Customers, such as checking identity documents. |
| **KYC-delegated mode** | An onboarding mode for licensed partners who perform KYC/KYB on their own users. See [**Licensed Partner Onboarding**](/docs/licensed-partner-onboarding). |
| **Onboarding** | The steps a Customer completes before they can transact: accepting the service agreement and passing KYC or KYB. |
| **RFI** (Request for Information) | A request from Harbor's compliance team for extra information or documents about a Customer or a transfer. See [**Onboarding RFI**](/docs/onboarding-rfi) and [**Transfer RFI**](/docs/transfer-rfi). |

### Transfers

Terms for creating, pricing, and completing transfers.

| Term | Definition |
| :--- | :--- |
| **B2B, B2C, C2B, C2C** | The sender and recipient types of a transfer: **B**usiness or **C**onsumer (individual). For example, B2C is a business paying an individual. |
| **Commission** | The fee you charge your Customer on a transfer, set as a fixed amount or a percentage when you create a quote. |
| **Corridor** | A route between a source and a destination, such as USDC on Ethereum to MXN in Mexico. Requirements and limits vary by corridor. |
| **Harbor fee** | The fee Harbor charges for processing a transfer. |
| **Off-ramp** | A transfer that converts a stablecoin into fiat currency, such as USDC to USD in a bank account. See [**Off-Ramp**](/docs/off-ramp-coverage). |
| **On-chain transfer** | A transfer from one blockchain address to another, without converting to fiat. Also called a **swap**. See [**On-Chain**](/docs/on-chain-coverage). |
| **On-ramp** | A transfer that converts fiat currency into a stablecoin, such as USD to USDC. See [**On-Ramp**](/docs/on-ramp-coverage). |
| **Pay first, bind later** | A debit card payout where the recipient adds their card details after the transfer is created. See [**Pay First, Bind Later**](/docs/pay-first-bind-later). |
| **Quote** | A price for a transfer, including the exchange rate and fees. You create a quote before each transfer, and it expires after a short time. See [**Quotes**](/docs/quotes). |
| **Settlement** | The point at which funds are fully cleared and available. See [**Settlement Strategy**](/docs/settlement-strategy). |
| **Swap** | Another name for an on-chain transfer, including moving stablecoins from one blockchain to another. See **On-chain transfer**. |
| **Transfer instructions** | The details returned when you create a transfer, telling the Customer where and how to send funds, and by when. |
| **Transfer requirements** | A JSON Schema listing the fields a specific transfer needs. They change by corridor and payment method, so you fetch them for each quote. See [**Transfer JSON Schema**](/docs/transfer-json-schema). |

### Funding and Bank Accounts

Ways Customers fund on-ramp transfers, and how Harbor matches and verifies bank deposits.

| Term | Definition |
| :--- | :--- |
| **Deposit account** | A dedicated virtual bank account issued to a Customer for on-ramp deposits. No narrative is needed. See [**Deposit Accounts**](/docs/deposit-accounts). |
| **Linked funding source** | A bank account or debit card the Customer links to fund on-ramp transfers. See [**Linked Funding Sources**](/docs/linked-funding-sources). |
| **Micro-deposit** | A small deposit (under US$1.00) that a bank sends to an account to verify it exists and belongs to the account holder. See [**Micro-Deposit Verification**](/docs/micro-deposit-verification). |
| **Narrative** | A unique 10-digit reference the sender includes in a wire, so Harbor can match the deposit to the right transfer. See [**Deposit Accounts vs. Narrative Transfers**](/docs/deposit-accounts-vs-narrative-transfers). |
| **Payment lock time** | The period during which funds from an ACH Pull or debit card are held while the payment settles. See [**Payment Lock Time**](/docs/payment-lock-time). |

### Payment Methods

The banking networks Harbor uses to move fiat money in and out, by country.

| Term | Definition |
| :--- | :--- |
| **ACH** (Automated Clearing House) | The US network for bank-to-bank transfers. |
| **ACH Pull** | An ACH payment where Harbor pulls funds from the Customer's linked US bank account. It takes about 2 business days to settle. |
| **ACH Push** | An ACH payment where the sender's bank pushes funds to the recipient's account. |
| **CLABE** | The 18-digit standardized bank account number used in Mexico. |
| **Faster Payments** | The UK's real-time bank transfer system for GBP. |
| **FedWire** | The US Federal Reserve's wire transfer system for domestic USD wires. |
| **International wire** | A cross-border bank transfer, usually sent over the SWIFT network. |
| **Local bank transfer** | A payout over the destination country's own banking network, in the local currency. |
| **PIX** | Brazil's instant payment system for BRL. |
| **RTP** (Real-Time Payments) | A US network for instant bank transfers, available 24/7. |
| **SEPA** (Single Euro Payments Area) | The payment network for EUR bank transfers between participating European countries. |
| **SPEI** | Mexico's real-time interbank payment system for MXN. Payouts use the recipient's CLABE. |
| **SWIFT/BIC code** | A code that identifies a bank in international wires. |

### Cards

Terms for debit card on-ramps and payouts.

| Term | Definition |
| :--- | :--- |
| **AFT** (Account Funding Transaction) | A card transaction that pulls funds from a debit card. Harbor uses it for card on-ramps. |
| **OCT** (Original Credit Transaction) | A card transaction that pushes funds to a debit card. Harbor uses it for card payouts (off-ramps). |
| **VDC** (Visa Direct to Card) | The card network service Harbor uses for debit card on-ramps and payouts. See [**Debit Card Coverage**](/docs/debit-card-coverage). |

### Blockchain and Stablecoins

Terms for the crypto side of a transfer: the assets Harbor supports and the networks they run on.

| Term | Definition |
| :--- | :--- |
| **Anchor** | On Stellar, a business that connects the Stellar network to traditional banking, handling deposits and withdrawals. See [**Stellar Anchor**](/docs/stellar-anchor). |
| **Blockchain address** | The public identifier of a wallet on a blockchain, such as `0xae13…7b55` on Ethereum. Funds are sent to and from addresses. |
| **Confirmations** | The number of blocks added after a transaction before it's considered final. It varies by blockchain. See [**Stablecoins and Blockchains**](/docs/stablecoins-and-blockchains). |
| **EVM** (Ethereum Virtual Machine) | The technology that runs Ethereum. Blockchains built on it are called **EVM-compatible** and share the same `0x` address format, so one wallet address works on all of them. Harbor's EVM chains are Ethereum, Polygon, Arbitrum, Avalanche, Optimism, Base, and Arc. |
| **Faucet** | A tool that gives you free test stablecoins on a testnet. See [**Testnet Faucet**](/docs/testnet-faucet). |
| **Mainnet** | The live version of a blockchain, where tokens have real value. Used in Production. |
| **SEP-10 / SEP-24** | Stellar Ecosystem Proposals. SEP-10 handles authentication with an anchor, and SEP-24 handles deposits and withdrawals. |
| **Solana** | A non-EVM blockchain. Solana addresses use a different format (32–44 base58 characters) and don't work on EVM chains or Stellar. |
| **Stablecoin** | A cryptocurrency designed to hold a steady value, usually pegged 1:1 to a fiat currency such as the US dollar. |
| **Stellar** | A non-EVM blockchain built for payments. Stellar addresses start with `G` and are 56 characters long. Accounts need a **trust line** to hold an asset, and some transfers need a **memo** (`address_tag`). |
| **Testnet** | A test version of a blockchain, where tokens have no real value. Used in Sandbox. |
| **Trust line** | On Stellar, a setting an account must add before it can hold a specific asset, such as USDC. |
| **USDC** | A US dollar stablecoin issued by Circle. Supported on all Harbor blockchains. |
| **USDT** | A US dollar stablecoin issued by Tether. Supported on Ethereum only. |

#### EVM vs. Solana vs. Stellar

Addresses are not interchangeable between these network types. Sending funds to an address on the wrong network can result in lost funds.

| Network type | Blockchains | Address format | Example |
| :--- | :--- | :--- | :--- |
| **EVM** | Ethereum, Polygon, Arbitrum, Avalanche, Optimism, Base, Arc | `0x` + 40 hex characters | `0xae13f53f0F85FF0A2245F3c7b6bfaB18e4117b55` |
| **Solana** | Solana | 32–44 base58 characters | `7EcDhSYGxXyscszYEp35KHN8vvw3svAuLKTzXwCFLtV` |
| **Stellar** | Stellar | `G` + 55 characters | `GBXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX` |

### API and Development

Terms you'll use when building your integration with the Harbor API.

| Term | Definition |
| :--- | :--- |
| **API key** | The secret key that authenticates your requests, sent in the `X-API-KEY` header. See [**API Keys**](/docs/api-keys). |
| **Idempotency key** | A unique value sent in the `X-Idempotency-Key` header so a retried request isn't processed twice. See [**Idempotency**](/docs/idempotency). |
| **JSON Schema** | A standard format for describing the structure of JSON data. Harbor uses it to describe transfer requirements. |
| **Webhook** | A notification Harbor sends to your server when something changes, such as a transfer status update. See [**Webhooks**](/docs/webhooks). |
