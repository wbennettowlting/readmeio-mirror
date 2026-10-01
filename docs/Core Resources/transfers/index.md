---
title: Transfers
metadata:
  robots: index
x-privacy:
  view: public
---
### Transfer API (V2)

<Callout icon="📘" theme="info">
  **The Transfer (V2) API** supports **USDC/local currencies**  & **USD/USDC** & **USDC/USDC** and Quote API & JSON Schema.
</Callout>

* [**Get Quote**](/docs/quotes)  
  Use **Get Quote** to request a real-time quote for a transaction.  
  A quote defines the core parameters of the transfer, including source currency, destination country and currency, payment method, supported amount limits, estimated fees, and exchange rates.

  Once a quote is successfully created, the API returns a `id`.  
  This `id` acts as a temporary reference that must be used to retrieve the requirement fields and to initiate the transaction while the quote remains valid.
* [**Retrieve requirements ​​via Quote ID (JSON Schema)**](/docs/transfer-json-schema)  
  Use **Retrieve requirements via Quote ID** to fetch the data requirements associated with a specific `quote_id`.  
  The response is returned as a **JSON Schema (Draft 2020-12)**, dynamically generated based on the quote parameters such as destination country, currency, payment method, and use case.
* **Initiate a transaction**  
  Use **Initiate a transaction** to create and submit a transaction based on a previously generated `quote.id`.  
  You must provide all required fields as defined in the corresponding JSON Schema requirements, such as recipient details, payout instruments, and compliance-related information.

Once the transaction is initiated, the system processes it according to the route and conditions locked by the quote and returns a transaction id along with the initial transaction status.

After creation, you can track the transaction lifecycle using status query APIs or webhooks.

* [**On-ramp (Fiat to Stablecoin)**](/docs/on-ramp)
* [**Off-ramp (Stablecoin to Fiat)**](/docs/off-ramp)
* [**On-chain (Stablecoin to Stablecoin)**](/docs/on-chain)
* [**Purpose of Transfer**](/docs/transfer-purpose)  
  In _Transfer (V1) API_, we did not restrict the value that could be entered in the **transfer_purpose** field, but **in (V2) Transfer, you can only enter a fixed value**. Please refer to our documentation.

### Status

When a **Transfer** is sent, the state changes may be as follows (For a detailed explanation of each status, please refer to [**Status**](/docs/status-definitions) :

<Image align="center" border={true} caption="Status of Transfer" src="https://files.readme.io/31f56c439a3cdea09db70e29d6f3f1c013101f51c786ec8c635ca3fc9ffeeedf-Status_of_Transfer.jpg" />
