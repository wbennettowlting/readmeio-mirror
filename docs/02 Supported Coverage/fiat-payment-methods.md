---
title: Fiat payment methods
hidden: false
metadata:
  robots: index
privacy:
  view: public
---
### Supported Transfer Payment Methods

**Supported Blockchains (9):** ETH, Polygon, SOL, AVAX, Arbitrum, Optimism, Base, Arc, Stellar

**Supported Stablecoins:** USDC (all blockchains above), USDT (Ethereum only)

***

#### Deposit (On-Ramp) Payment Methods

| Region  | **Deposit Payment Method**               |
| :------ | :--------------------------------------- |
| **USA** | ACH Pull, Wire, Debit Card               |

***

<br />

#### Withdrawal (Off-Ramp) Payment Methods

<br />

| Region / Country            | **Withdrawal Payment Method**                                                                                                                                                                                                                                                                     |
| :-------------------------- | :------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| **USA (US)**                | ACH Push, RTP, Domestic Wire, International Wire, Debit Card                                                                                                                                                                                                                                      |
| **European Union (EU) & SEPA Countries**<br />*(Austria (AT), Belgium (BE), Bulgaria (BG), Croatia (HR), Cyprus (CY), Czech Republic (CZ), Denmark (DK), Estonia (EE), Finland (FI), France (FR), Germany (DE), Greece (GR), Hungary (HU), Ireland (IE), Italy (IT), Latvia (LV), Lithuania (LT), Luxembourg (LU), Malta (MT), Netherlands (NL), Poland (PL), Portugal (PT), Romania (RO), Slovakia (SK), Slovenia (SI), Spain (ES), Sweden (SE), Andorra (AD), Albania (AL), Switzerland (CH), Iceland (IS), Liechtenstein (LI), Monaco (MC), Moldova (MD), Montenegro (ME), North Macedonia (MK), Serbia (RS), San Marino (SM), Vatican City (VA))* | SEPA, SEPA instant, International Wire                                                                                                                                                                                                                                                            |
| **United Kingdom (GB)**     | Faster Payments, International Wire                                                                                                                                                                                                                                                               |
| **Mexico (MX)**             | CLABE (SPEI)                                                                                                                                                                                                                                                                                      |
| **Brazil (BR)**             | PIX                                                                                                                                                                                                                                                                                               |
| **Hong Kong (HK)**          | International Wire, Local Bank Transfer                                                                                                                                                                                                                                                           |
| **China (CN)**              | International Wire, Local Bank Transfer                                                                                                                                                                                                                                                           |
| **Singapore (SG)**          | International Wire, Local Bank Transfer                                                                                                                                                                                                                                                           |
| **Japan (JP)**              | International Wire, Local Bank Transfer                                                                                                                                                                                                                                                           |
| **UAE (AE)**                | International Wire, Local Bank Transfer                                                                                                                                                                                                                                                           |
| **Nigeria (NG)**            | International Wire, Local Bank Transfer                                                                                                                                                                                                                                                           |
| **Cayman Islands (KY)**     | International Wire                                                                                                                                                                                                                                                                                |
| **Colombia (CO)**           | Local Bank Transfer                                                                                                                                                                                                                                                                               |
| **India (IN)**              | Local Bank Transfer                                                                                                                                                                                                                                                                               |
| **Philippines (PH)**        | Local Bank Transfer                                                                                                                                                                                                                                                                               |
| **Canada (CA), South Korea (KR), Taiwan (TW), Norway (NO), Indonesia (ID), Kenya (KE), South Africa (ZA), Pakistan (PK), Egypt (EG), Ghana (GH), Georgia (GE), Kuwait (KW), Qatar (QA), Bahrain (BH), Thailand (TH), Vietnam (VN)** | Wire (USD only)                                                                                                                                                                                                                                                                                   |

***

<br />

#### On-Chain (Swap)

| Type         | Payment Method               |
| :----------- | :--------------------------- |
| **On-Chain** | Blockchain ↔ Blockchain swap |

***

<h4>Dynamic Settings Retrieval</h4>

To programmatically retrieve the latest withdrawal limits, supported countries, and currency pairs for your application, use the following API:

[Get Withdrawal Settings (v2)](/reference/getwithdrawalsettingsv2)

##### Why use this API?

While the tables above provide a general overview, specific limits (Min/Max) and supported pairs can vary based on your Application's configuration and the type of entities involved (Individual vs. Business). This API returns the exact real-time settings applicable to your integration.

##### Response Explanation

The API returns a list of supported withdrawal paths. Each path includes:

| Field                  | Description                                                                   |
| :--------------------- | :---------------------------------------------------------------------------- |
| `source_currency`      | The currency being sent (e.g., `USDC`).                                       |
| `destination_country`  | The target country code (ISO 3166-1 alpha-2, e.g., `MX`).                     |
| `destination_currency` | The currency to be received in the target country (e.g., `MXN`, `USD`).       |
| `source_type`          | The sender type: `individual` or `business`.                                  |
| `destination_type`     | The recipient type: `individual` or `business`.                               |
| `source_amount`        | An object containing `min`, `max`, and `currency` for the source amount.      |
| `destination_amount`   | An object containing `min`, `max`, and `currency` for the destination amount. |

###### Example Response Entry

```json
{
  "source_currency": "USDC",
  "destination_country": "MX",
  "destination_currency": "MXN",
  "source_type": "individual",
  "destination_type": "individual",
  "source_amount": {
    "currency": "USDC",
    "min": 18,
    "max": 9998
  },
  "destination_amount": {
    "currency": "MXN",
    "min": 322.68,
    "max": 179233.15
  }
}
```

***

#### Detailed Breakdown

<br />

| Country | Dest Currency | Payment Method | Blockchains | Fiat Currency | Stablecoin | B2B | B2C | C2B | C2C |
| :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- |
| US | USD | ACH Push | ETH, Polygon, SOL, AVAX, Arbitrum, OP, Base, Arc, Stellar | USD | USDC, USDT* | ✅ | ✅ | ✅ | ✅ |
| US | USD | Domestic Wire | ETH, Polygon, SOL, AVAX, Arbitrum, OP, Base, Arc, Stellar | USD | USDC, USDT* | ✅ | ✅ | ✅ | ✅ |
| US | USD | FedWire | ETH, Polygon, SOL, AVAX, Arbitrum, OP, Base, Arc, Stellar | USD | USDC, USDT* | ✅ | ✅ | ✅ | ✅ |
| US | USD | International Wire | ETH, Polygon, SOL, AVAX, Arbitrum, OP, Base, Arc, Stellar | USD | USDC, USDT* | ✅ | ✅ | ✅ | ✅ |
| US | USD | RTP | ETH, Polygon, SOL, AVAX, Arbitrum, OP, Base, Arc, Stellar | USD | USDC, USDT* | ✅ | ✅ | ✅ | ✅ |
| BR | BRL | PIX | ETH, Polygon, SOL, AVAX, Arbitrum, OP, Base, Arc, Stellar | BRL | USDC, USDT* | ✅ | ✅ | ✅ | ✅ |
| MX | MXN | CLABE (SPEI) | ETH, Polygon, SOL, AVAX, Arbitrum, OP, Base, Arc, Stellar | MXN | USDC, USDT* | ✅ | ✅ | ✅ | ✅ |
| EU (Austria (AT), Belgium (BE), Bulgaria (BG), Croatia (HR), Cyprus (CY), Czech Republic (CZ), Denmark (DK), Estonia (EE), Finland (FI), France (FR), Germany (DE), Greece (GR), Hungary (HU), Ireland (IE), Italy (IT), Latvia (LV), Lithuania (LT), Luxembourg (LU), Malta (MT), Netherlands (NL), Poland (PL), Portugal (PT), Romania (RO), Slovakia (SK), Slovenia (SI), Spain (ES), Sweden (SE), Andorra (AD), Albania (AL), Switzerland (CH), Iceland (IS), Liechtenstein (LI), Monaco (MC), Moldova (MD), Montenegro (ME), North Macedonia (MK), Serbia (RS), San Marino (SM), Vatican City (VA)) | EUR | SEPA | ETH, Polygon, SOL, AVAX, Arbitrum, OP, Base, Arc, Stellar | EUR | USDC, USDT* | ✅ | ✅ | ✅ | ✅ |
| EU (Austria (AT), Belgium (BE), Bulgaria (BG), Croatia (HR), Cyprus (CY), Czech Republic (CZ), Denmark (DK), Estonia (EE), Finland (FI), France (FR), Germany (DE), Greece (GR), Hungary (HU), Ireland (IE), Italy (IT), Latvia (LV), Lithuania (LT), Luxembourg (LU), Malta (MT), Netherlands (NL), Poland (PL), Portugal (PT), Romania (RO), Slovakia (SK), Slovenia (SI), Spain (ES), Sweden (SE), Andorra (AD), Albania (AL), Switzerland (CH), Iceland (IS), Liechtenstein (LI), Monaco (MC), Moldova (MD), Montenegro (ME), North Macedonia (MK), Serbia (RS), San Marino (SM), Vatican City (VA)) | USD | International Wire | ETH, Polygon, SOL, AVAX, Arbitrum, OP, Base, Arc, Stellar | USD | USDC, USDT* | ✅ | ✅ | ✅ | ✅ |
| GB | GBP | Faster Payments | ETH, Polygon, SOL, AVAX, Arbitrum, OP, Base, Arc, Stellar | GBP | USDC, USDT* | ✅ | ✅ | ✅ | ✅ |
| GB | USD | International Wire | ETH, Polygon, SOL, AVAX, Arbitrum, OP, Base, Arc, Stellar | USD | USDC, USDT* | ✅ | ✅ | ✅ | ✅ |
| HK | HKD | Local Bank Transfer | ETH, Polygon, SOL, AVAX, Arbitrum, OP, Base, Arc, Stellar | HKD | USDC, USDT* | ✅ | ✅ | ✅ | ✅ |
| HK | USD | International Wire | ETH, Polygon, SOL, AVAX, Arbitrum, OP, Base, Arc, Stellar | USD | USDC, USDT* | ✅ | ✅ | ✅ | ✅ |
| CN | CNY | Local Bank Transfer | ETH, Polygon, SOL, AVAX, Arbitrum, OP, Base, Arc, Stellar | CNY | USDC, USDT* | ✅ | ✅ | ✅ | ✅ |
| CN | USD | International Wire | ETH, Polygon, SOL, AVAX, Arbitrum, OP, Base, Arc, Stellar | USD | USDC, USDT* | ✅ | ✅ | ✅ | ✅ |
| SG | SGD | Local Bank Transfer | ETH, Polygon, SOL, AVAX, Arbitrum, OP, Base, Arc, Stellar | SGD | USDC, USDT* | ✅ | ✅ | ✅ | ✅ |
| SG | USD | International Wire | ETH, Polygon, SOL, AVAX, Arbitrum, OP, Base, Arc, Stellar | USD | USDC, USDT* | ✅ | ✅ | ✅ | ✅ |
| JP | JPY | Local Bank Transfer | ETH, Polygon, SOL, AVAX, Arbitrum, OP, Base, Arc, Stellar | JPY | USDC, USDT* | ✅ | ✅ | ✅ | ✅ |
| JP | USD | International Wire | ETH, Polygon, SOL, AVAX, Arbitrum, OP, Base, Arc, Stellar | USD | USDC, USDT* | ✅ | ✅ | ✅ | ✅ |
| AE | AED | Local Bank Transfer | ETH, Polygon, SOL, AVAX, Arbitrum, OP, Base, Arc, Stellar | AED | USDC, USDT* | ✅ | ✅ | ✅ | ✅ |
| AE | USD | International Wire | ETH, Polygon, SOL, AVAX, Arbitrum, OP, Base, Arc, Stellar | USD | USDC, USDT* | ✅ | ✅ | ✅ | ✅ |
| NG | NGN | Local Bank Transfer | ETH, Polygon, SOL, AVAX, Arbitrum, OP, Base, Arc, Stellar | NGN | USDC, USDT* | ✅ | ✅ | ✅ | ✅ |
| NG | USD | International Wire | ETH, Polygon, SOL, AVAX, Arbitrum, OP, Base, Arc, Stellar | USD | USDC, USDT* | ✅ | ✅ | ✅ | ✅ |
| KY | USD | International Wire | ETH, Polygon, SOL, AVAX, Arbitrum, OP, Base, Arc, Stellar | USD | USDC, USDT* | ✅ | ✅ | ✅ | ✅ |
| ID | USD | International Wire | ETH, Polygon, SOL, AVAX, Arbitrum, OP, Base, Arc, Stellar | USD | USDC, USDT* | ✅ | ✅ | ✅ | ✅ |
| KE | USD | International Wire | ETH, Polygon, SOL, AVAX, Arbitrum, OP, Base, Arc, Stellar | USD | USDC, USDT* | ✅ | ✅ | ✅ | ✅ |
| PK | USD | International Wire | ETH, Polygon, SOL, AVAX, Arbitrum, OP, Base, Arc, Stellar | USD | USDC, USDT* | ✅ | ✅ | ✅ | ✅ |
| EG | USD | International Wire | ETH, Polygon, SOL, AVAX, Arbitrum, OP, Base, Arc, Stellar | USD | USDC, USDT* | ✅ | ✅ | ✅ | ✅ |
| CA | USD | Wire | ETH, Polygon, SOL, AVAX, Arbitrum, OP, Base, Arc, Stellar | USD | USDC, USDT* | ✅ | ✅ | ✅ | ✅ |
| KR | USD | Wire | ETH, Polygon, SOL, AVAX, Arbitrum, OP, Base, Arc, Stellar | USD | USDC, USDT* | ✅ | ✅ | ✅ | ✅ |
| TW | USD | Wire | ETH, Polygon, SOL, AVAX, Arbitrum, OP, Base, Arc, Stellar | USD | USDC, USDT* | ✅ | ✅ | ✅ | ✅ |
| NO | EUR | SEPA | ETH, Polygon, SOL, AVAX, Arbitrum, OP, Base, Arc, Stellar | EUR | USDC, USDT* | ✅ | ✅ | ✅ | ✅ |
| NO | USD | Wire | ETH, Polygon, SOL, AVAX, Arbitrum, OP, Base, Arc, Stellar | USD | USDC, USDT* | ✅ | ✅ | ✅ | ✅ |
| ZA | USD | Wire | ETH, Polygon, SOL, AVAX, Arbitrum, OP, Base, Arc, Stellar | USD | USDC, USDT* | ✅ | ✅ | ✅ | ✅ |
| GH | USD | Wire | ETH, Polygon, SOL, AVAX, Arbitrum, OP, Base, Arc, Stellar | USD | USDC, USDT* | ✅ | ✅ | ✅ | ✅ |
| CO | COP | Local Bank Transfer | ETH, Polygon, SOL, AVAX, Arbitrum, OP, Base, Arc, Stellar | COP | USDC, USDT* | ✅ | ✅ | ✅ | ✅ |
| IN | INR | Local Bank Transfer | ETH, Polygon, SOL, AVAX, Arbitrum, OP, Base, Arc, Stellar | INR | USDC, USDT* | ✅ | ✅ | ✅ | ✅ |
| PH | PHP | Local Bank Transfer | ETH, Polygon, SOL, AVAX, Arbitrum, OP, Base, Arc, Stellar | PHP | USDC, USDT* | ✅ | ✅ | ✅ | ✅ |
| GE | USD | Wire | ETH, Polygon, SOL, AVAX, Arbitrum, OP, Base, Arc, Stellar | USD | USDC, USDT* | ✅ | ✅ | ✅ | ✅ |
| KW | USD | Wire | ETH, Polygon, SOL, AVAX, Arbitrum, OP, Base, Arc, Stellar | USD | USDC, USDT* | ✅ | ✅ | ✅ | ✅ |
| QA | USD | Wire | ETH, Polygon, SOL, AVAX, Arbitrum, OP, Base, Arc, Stellar | USD | USDC, USDT* | ✅ | ✅ | ✅ | ✅ |
| BH | USD | Wire | ETH, Polygon, SOL, AVAX, Arbitrum, OP, Base, Arc, Stellar | USD | USDC, USDT* | ✅ | ✅ | ✅ | ✅ |
| TH | USD | Wire | ETH, Polygon, SOL, AVAX, Arbitrum, OP, Base, Arc, Stellar | USD | USDC, USDT* | ✅ | ✅ | ✅ | ✅ |
| VN | USD | Wire | ETH, Polygon, SOL, AVAX, Arbitrum, OP, Base, Arc, Stellar | USD | USDC, USDT* | ✅ | ✅ | ✅ | ✅ |

<sup>*</sup> USDT is only supported on Ethereum.
