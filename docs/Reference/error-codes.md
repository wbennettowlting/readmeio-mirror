---
title: Error Codes
metadata:
  robots: index
privacy:
  view: public
slug: error-codes
---
**Full Error Code Reference**

When validation fails, or when request parameters violate business rules, Harbor returns structured validation errors (`422 Unprocessable Entity`), state conflicts (`409 Conflict`), or custom numeric error codes. Use this reference to parse error responses and guide your users on corrective actions.

<Cards columns={3}>
  <Card title="1. Onboarding & Validation" href="#1-onboarding--validation-error-codes" icon="fa-solid fa-user-check">
    Validation, upgrade conflicts, and onboarding middleware errors.
  </Card>
  <Card title="2. Auth & General" href="#4-authentication--general-errors" icon="fa-solid fa-key">
    API keys, scopes, bad requests, validation errors, and server errors.
  </Card>
  <Card title="3. Customer & Application" href="#5-customer--application-errors" icon="fa-solid fa-user-gear">
    Unverified states, unsigned agreements, expired links, and disabled features.
  </Card>
  <Card title="4. Transfer & Quotas" href="#6-transfer-error-codes" icon="fa-solid fa-money-bill-transfer">
    Quotes, limits, transaction quotas, and routing errors.
  </Card>
  <Card title="5. Bank & Card" href="#7-bank-connection-error-codes" icon="fa-solid fa-building-columns">
    Widget generation, account syncing, and card bindings.
  </Card>
  <Card title="6. Wallet, Faucet & AML" href="#10-wallet-error-codes" icon="fa-solid fa-wallet">
    Wallet APIs, USDC testnet faucets, and Sumsub/Circle provider errors.
  </Card>
</Cards>

***

### Error Response Format

Every error response from the Application API follows the same JSON shape:

```json
{
  "message": "Human-readable error description",
  "error": "Same as message (deprecated, use message instead)",
  "code": 3017,
  "error_type": "transfer.quote_expired"
}
```

| Field | Type | Description |
| :--- | :--- | :--- |
| `message` | string | Human-readable error description. Safe to show to end users for some errors, but treat it as free text — it can change without notice. |
| `error` | string | **Deprecated.** Alias for `message`. Kept for backward compatibility only. |
| `code` | integer | Stable numeric error code. Use this for programmatic error handling — it will not change once published. |
| `error_type` | string | Dot-notation identifier derived from `code` (e.g. `transfer.quote_expired`), handy for logging/readability. |
| `details` | array/object | *(optional)* Additional machine-readable context, present when applicable (e.g. which fields are missing). |
| `errors` | object | *(validation only)* Field-level validation errors keyed by field name, returned with `code: 2005`. |

**Recommendation:** branch on `code` (or `error_type`), not on `message` — the wording of `message` is not a stable contract.

***

### 1. Onboarding & Validation Error Codes

The following numeric error codes are returned in the response body when submitting or updating onboarding data (`POST` / `PATCH` / `/upgrade` endpoints):

| HTTP | Code | Meaning | What It Means / Developer Action |
| :--- | :--- | :--- | :--- |
| `422` | — | Field-level validation failed | One or more payload parameters are invalid. The response contains specific field-level validation errors. |
| `409` | `2301` | No verification record to elevate | An `/upgrade` request was made for a customer with no existing verification record. Complete initial onboarding first, or contact support if one should exist. |
| `409` | `2302` | Onboarding in progress | An onboarding is already in progress for this customer. Only one active onboarding is allowed. |
| `409` | `2303` | Level already approved / Conflict | Target level has already been approved, or `PATCH`/`POST` conflicts with current verified tier. |
| `409` | `2305` | Action blocked during processing | Tried to call `PATCH` or submit an upgrade while the onboarding status is still `processing`. Wait for processing to complete. |
| `422` | `2306` | Customer already verified | The customer is already fully KYC/KYB verified and cannot undergo onboarding again. |
| `422` | `2310` | Ineligible for elevation | The upgrade request does not meet eligibility prerequisites (e.g., attempting to upgrade to Level 3 before Level 2 is verified). |
| `409` | `2311` | Must update existing record | This customer already has a verification record. Send corrections with `PATCH` on the same path instead of `POST`. |
| `409` | `2312` | Declined — final | The customer was declined (or the verification provider marked it as banned) and cannot be verified again through this API. This state is final; contact support for manual review. |
| `409` | `2313` | Upgrade cooldown in progress | A Level 3 upgrade request was recently rejected, and the compliance cooldown period is active. |
| `409` | `2314` | KYC session change not allowed | The customer is already verified and the requested change cannot be re-verified. Only a Level 1 → 2 upgrade, or a switch between US and non-US residence, is accepted. |
| `409` | `2315` | KYC revoked | The customer's verification was revoked by the provider. No further information can be submitted through this API. |
| `422` | `2316` | Level 1 US-only violation | Submitting a non-US resident (`residence.country != "US"`) for Level 1 is forbidden. |
| `503` | `2317` | Verification session temporarily unavailable | The verification session could not be started right now. Retry after a short delay. |
| `422` | `2318` | Upgrade blocked by outstanding RFI | Cannot request an upgrade because the current onboarding is in `action_required` status. Correct existing data first. |
| `422` | `2319` | Duplicate upgrade in progress | Cannot request an upgrade because another upgrade request is currently being processed. |

***

### 2. Onboarding Upgrade Conflicts (409 Conflict)

When calling the `/upgrade` endpoint, if the upgrade request cannot proceed due to the customer's current onboarding state, the API returns a `409 Conflict` response with a specific `situation` or error state:

| Situation / Situation Code | Meaning | What It Means / Developer Action |
| :--- | :--- | :--- |
| `action_required` | Onboarding action required | The current onboarding level is in `action_required` state. The customer must resolve outstanding requirements using **PATCH** on the main onboarding endpoint. |
| `action_required_in_flight` | Upgrade action required | An upgrade is already pending but needs correction. Fix issues by sending a **PATCH** request. |
| `processing` | Onboarding in progress | The customer's initial onboarding is still being validated. Wait for initial onboarding to become `verified` first. |
| `submission_processing` | Upgrade request validating | An upgrade request is already in progress and being validated. Wait for the request to finish. |
| `submitted` | Upgrade under review | An upgrade request is under manual compliance review. Wait for the compliance team to approve or reject the pending review. |
| `already_active` | Level already active | No action needed. The customer has already been verified at this level (or higher). |
| `details` | Prerequisites not met | The upgrade request does not meet the eligibility prerequisites. Read the error details and ensure Level 2 is verified before attempting Level 3. |

***

### 3. Onboarding Entry Middleware Errors

These errors are thrown by the gateway middleware prior to route processing to protect data scheme integrity:

| HTTP | Code | Error Identifier | What It Means / Developer Action |
| :--- | :--- | :--- | :--- |
| `422` | `2307` | `individual_route_type_mismatch` | Attempted to submit a Graded Onboarding (V2) individual payload for a customer whose type is `corporate`. |
| `422` | `2308` | `kyc_delegated_not_supported` | Graded Onboarding is not supported for Delegated KYC customers. |
| `422` | `2309` | `v1_customer_use_v1` | Customer is already on the (V1) onboarding scheme (has (V1) records/history) and cannot migrate to (V2). |

***

### 4. Authentication & General Errors

| HTTP | Code | Error Type | What It Means / Developer Action |
| :--- | :--- | :--- | :--- |
| `401` | `1001` | `auth.api_key_required` | The `X-API-KEY` header is missing from the request. |
| `401` | `1002` | `auth.invalid_api_key` | The provided API key is invalid or has been revoked. Generate a new one from the Harbor Portal. |
| `401` | `1003` | `auth.token_expired` | The authentication token has expired. Re-authenticate and retry. |
| `403` | `1006` | `auth.api_key_scope_insufficient` | The API key does not have the required scope/permission for this endpoint. Check the key's scopes in the Harbor Portal. |
| `404` | `2001` | `general.resource_not_found` | The requested resource does not exist. |
| `403` | `2002` | `general.forbidden` | Access to the resource is denied. |
| `400` | `2003` | `general.bad_request` | The request is malformed. |
| `202` / `400` | `2004` | `general.idempotency_conflict` | A request with the same `X-Idempotency-Key` has already been processed (or is currently processing). Reuse the original response instead of retrying with a new payload under the same key. |
| `422` | `2005` | `general.validation_error` | Request body validation failed. Check the `errors` field for per-field details. |
| `502` | `2006` | `general.internal_server_error` | An unexpected internal error occurred. Retry later; contact support if it persists. |

***

### 5. Customer & Application Errors

| HTTP | Code | Error Type | What It Means / Developer Action |
| :--- | :--- | :--- | :--- |
| `404` | `2101` | `customer.not_found` | The specified customer does not exist. |
| `403` | `2102` | `customer.not_verified` | The customer status is not verified — the action requires a verified customer. |
| `403` | `2103` | `customer.agreement_not_signed` | The customer has not signed the required agreement yet. |
| `400` | `2104` | `customer.link_expired` | The customer-facing link (e.g. onboarding/KYC link) has expired. Generate a new one. |
| `400` | `2105` | `customer.hash_error` | Customer hash verification failed — the link or signed payload was tampered with or malformed. |
| `400` | `2106` | `customer.invalid_integrated_status` | The customer's integrated KYC status is invalid for this operation. |
| `403` | `2107` | `customer.creation_not_allowed` | Your application is not allowed to create customers. Contact support. |
| `403` | `2201` | `application.kyc_not_verified` | Your application's own KYC/verification is not complete. Complete application-level verification before this operation is allowed. |
| `400` | `2202` | `application.feature_not_enabled` | This feature (e.g. customer deposit accounts) is not enabled for your application. Contact your account manager to have it enabled. |

***

### 6. Transfer Error Codes

| HTTP | Code | Error Type | What It Means / Developer Action |
| :--- | :--- | :--- | :--- |
| `400` | `3001` | `transfer.final_amount_negative` | The calculated final amount is negative. Check fees/rates against the requested amount. |
| `400` | `3002` | `transfer.unsupported_transfer_type` | The requested transfer type is not supported. |
| `400` | `3003` | `transfer.on_behalf_of_not_found` | The `on_behalf_of` customer was not found. |
| `400` | `3004` | `transfer.failed_to_create_order` | Failed to create the order with the payment provider. Retry, or contact support if it persists. |
| `400` | `3005` | `transfer.failed_to_query_order` | Failed to query the order status from the payment provider. |
| `400` | `3006` | `transfer.on_behalf_of_not_active` | The `on_behalf_of` customer is not in an active state. |
| `400` | `3007` | `transfer.unsupported_transfer_pair` | The requested source/destination currency pair is not supported. |
| `400` | `3008` | `transfer.exchange_rate_not_found` | No exchange rate is available for the requested pair right now. |
| `400` | `3009` | `transfer.min_transfer_amount_in_USD` | The transfer amount is below the minimum limit. |
| `400` | `3010` | `transfer.unsupported_source_asset` | The source asset is not supported for this route. |
| `400` | `3011` | `transfer.failed_to_update_status` | Failed to update the transfer status. |
| `400` | `3012` | `transfer.monthly_transaction_count_limit_exceeded` | The monthly transaction count limit has been reached. |
| `400` | `3013` | `transfer.max_transfer_amount_in_USD` | The transfer amount exceeds the maximum limit. |
| `400` | `3014` | `transfer.invalid_rate` | The exchange rate used is invalid. |
| `400` | `3015` | `transfer.invalid_commission` | The commission configuration is invalid. |
| `400` | `3016` | `transfer.invalid_partner_fee` | The partner fee configuration is invalid. |
| `400` | `3017` | `transfer.quote_expired` | The transfer quote has expired. Request a new quote and retry. |
| `400` | `3018` | `transfer.failed_to_get_quotes` | Failed to retrieve quotes from the payment provider. |
| `400` | `3019` | `transfer.failed_to_query_bank_module_quote_requirements` | Failed to query quote requirements. |
| `400` | `3020` | `transfer.update_transfer_status_failed` | Failed to update the transfer status. |
| `400` | `3021` | `transfer.quote_requirements_not_found` | Quote requirements could not be found for this route. |
| `400` | `3022` | `transfer.failed_to_query_bank_module_quote_exchange_rate` | Failed to query the exchange rate for the quote. |
| `400` | `3023` | `transfer.failed_to_query_bank_module_quote_limits` | Failed to query quote limits. |
| `400` | `3024` | `transfer.failed_to_query_bank_module_supported_sender_countries_and_currencies` | Failed to query supported sender countries/currencies. |
| `400` | `3025` | `transfer.failed_to_query_bank_module_supported_destination_countries_and_currencies` | Failed to query supported destination countries/currencies. |
| `400` | `3026` | `transfer.no_fee_set_found` | No active fee set is configured for this application. Contact support. |
| `400` | `3027` | `transfer.failed_to_query_bank_module_supported_withdrawal_chains` | Failed to query supported withdrawal chains. |
| `400` | `3028` | `transfer.failed_to_query_bank_module_supported_withdrawal_source_countries` | Failed to query supported withdrawal source countries. |
| `400` | `3029` | `transfer.payment_method_per_transaction_limit_exceeded` | The per-transaction limit was exceeded for this payment method. |
| `400` | `3030` | `transfer.payment_method_weekly_limit_exceeded` | The weekly limit was exceeded for this payment method. |
| `400` | `3031` | `transfer.payment_method_monthly_limit_exceeded` | The monthly limit was exceeded for this payment method. |
| `400` | `3032` | `transfer.payment_method_daily_limit_exceeded` | The daily limit was exceeded for this payment method. |
| `400` | `3033` | `transfer.creation_failed` | Transfer creation failed. |
| `400` | `3034` | `transfer.receipt_calculation_failed` | Fee/receipt calculation failed. |
| `400` | `3035` | `transfer.quote_retrieval_failed` | No quotes are available for the requested transfer pair. |
| `400` | `3036` | `transfer.quote_creation_failed` | Failed to persist the transfer quote. |
| `400` | `3037` | `transfer.missing_amount_input` | Either source or destination amount is required, and neither was supplied. |
| `400` | `3038` | `transfer.insufficient_wallet_balance` | The wallet does not have enough available balance to complete this transfer. |
| `400` | `3039` | `transfer.rtp_routing_number_required` | A routing number is required for RTP transfers. |
| `400` | `3040` | `transfer.rtp_routing_number_not_supported` | The supplied routing number does not support RTP. |
| `400` | `3041` | `transfer.simulate_not_allowed` | Transfer simulation is not allowed for this request/application. |
| `400` | `3042` | `transfer.x402_quote_not_owlpay` | x402 requires a transfer quote created in OwlPay gas-fee mode. |
| `400` | `3043` | `transfer.x402_type_not_off_ramp` | x402 only supports off-ramp transfers. |
| `400` | `3044` | `transfer.x402_transfer_state_invalid` | The transfer is not in a state that can be enabled for x402 (must be pending the customer's transfer start). |
| `400` | `3045` | `transfer.x402_not_enabled` | x402 is not enabled for this customer. |
| `400` | `3049` | `transfer.x402_transfer_expired` | The transfer's crypto pay-in window has already expired; it can no longer be enabled for x402. Create a fresh transfer instead. |
| `422` | `3050` | `transfer.not_completed` | The requested resource (e.g. a payment confirmation letter) is only available once the transfer is `COMPLETED`. |
| `403` | `3051` | `transfer.creation_not_allowed` | Your application is not allowed to create new transfers right now (existing quotes/transfers are unaffected). Contact support. |
| `400` | `3052` | `transfer.incomplete_cardholder_profile` | The customer's cardholder profile is missing fields required for a card payment. Check `details` for which fields are missing. |
| `400` | `3053` | `transfer.tier_quota_per_transaction_exceeded` | The amount exceeds the per-transaction limit for the customer's verification level. |
| `400` | `3054` | `transfer.tier_quota_daily_exceeded` | The amount would exceed the daily limit for the customer's verification level. |
| `400` | `3055` | `transfer.tier_quota_monthly_exceeded` | The amount would exceed the monthly limit for the customer's verification level. |
| `400` | `3056` | `transfer.tier_quota_lifetime_exceeded` | The amount would exceed the lifetime limit for the customer's verification level. |
| `400` | `3057` | `transfer.tier_quota_valuation_unavailable` | The source asset could not be valued in USD to check the verification-level quota. Retry later. |
| `409` | `3058` | `transfer.tier_quota_check_in_progress` | Another transfer is currently being created for this customer, holding the quota lock. Retry shortly. |
| `400` | `3059` | `transfer.usdt_not_enabled` | USDT is not enabled for this application. Contact your account manager. |
| `400` | `3060` | `transfer.tier_quota_level_missing` | The customer has no approved verification level yet, so no quota can be measured. Complete verification first. |
| `400` | `3061` | `transfer.tier_quote_method_not_allowed` | No quote is available because the customer's verification level restricts which payment methods can be quoted. |

***

### 7. Bank Connection Error Codes

| HTTP | Code | Error Type | What It Means / Developer Action |
| :--- | :--- | :--- | :--- |
| `400` | `5001` | `bank_connection.failed_to_generate_widget_url` | Failed to generate the bank connection widget URL. |
| `400` | `5002` | `bank_connection.failed_to_sync` | Failed to sync bank connection data. |
| `400` | `5003` | `bank_connection.failed_to_list_accounts` | Failed to list linked bank accounts. |
| `400` | `5004` | `bank_connection.failed_to_get_balance` | Failed to retrieve the account balance. |
| `400` | `5005` | `bank_connection.failed_to_generate_update_widget_url` | Failed to generate the widget URL used to refresh/update an existing bank connection. |

***

### 8. Card Error Codes

| HTTP | Code | Error Type | What It Means / Developer Action |
| :--- | :--- | :--- | :--- |
| `400` | `6001` | `card.failed_to_create_binding_link` | Failed to create the card binding link. |
| `400` | `6002` | `card.failed_to_list` | Failed to list cards. |
| `400` | `6003` | `card.failed_to_review` | Failed to submit the card for compliance review. |
| `400` | `6004` | `card.incomplete_cardholder_profile` | The cardholder profile is missing fields required for card binding. Check `details` for which fields are missing. |

***

### 9. Faucet Error Codes

The faucet endpoints are **sandbox/testnet-only** developer tools for funding test wallets — they are not part of the production API surface.

| HTTP | Code | Error Type | What It Means / Developer Action |
| :--- | :--- | :--- | :--- |
| `422` | `7001` | `faucet.daily_limit_exceeded` | The daily USDC faucet limit has been reached. Retry after the limit resets. |
| `422` | `7002` | `faucet.circle_recipient_failed` | Failed to create the Circle recipient for the faucet payout. |
| `422` | `7003` | `faucet.circle_payout_failed` | Failed to create the Circle payout. |
| `403` | `7004` | `faucet.production_not_allowed` | The faucet is not available in production. Use the sandbox environment. |
| `422` | `7005` | `faucet.usdt_chain_not_supported` | The USDT faucet only supports the Ethereum chain. |
| `422` | `7006` | `faucet.bank_module_faucet_failed` | The underlying faucet call failed. Check `details.http_status` for the upstream status. |

***

### 10. Wallet Error Codes

| HTTP | Code | Error Type | What It Means / Developer Action |
| :--- | :--- | :--- | :--- |
| `404` | `8001` | `wallet.not_found` | The specified wallet does not exist. |
| `403` | `8002` | `wallet.api_not_enabled` | The Wallet API is not enabled for this application. Contact your account manager. |
| `400` | `8003` | `wallet.failed_to_create` | Failed to create the wallet. |
| `400` | `8004` | `wallet.failed_to_delete` | Failed to delete the wallet. |
| `400` | `8005` | `wallet.failed_to_get_records` | Failed to retrieve wallet records. |
| `400` | `8006` | `wallet.failed_to_get_balances` | Failed to retrieve wallet balances. |

***

### 11. Identity Verification (AML/KYC Provider) Errors

These codes come from Harbor's calls to its identity-verification provider during onboarding, transfer creation, and card binding. All of them return **HTTP 400**. For these specifically, the `message` field is either a curated, merchant-safe message from the provider or a generic fallback ("We were unable to complete this request with our verification provider. Please try again later or contact support if the issue persists.") — the underlying diagnostic detail is intentionally never exposed. **Branch on `code`, not on `message`, for these errors.**

| Code | Error Type | What It Means |
| :--- | :--- | :--- |
| `9901` | `aml.chain_bind_failed` | Failed to bind a blockchain address to the customer's verification profile. |
| `9902` | `aml.target_chain_bind_failed` | Failed to bind the destination chain address. |
| `9903` | `aml.on_ramp_order_creation_failed` | Failed to create the on-ramp order with the verification provider. |
| `9904` | `aml.off_ramp_order_creation_failed` | Failed to create the off-ramp order with the verification provider. |
| `9905` | `aml.swap_order_creation_failed` | Failed to create the swap order with the verification provider. |
| `9906` | `aml.swap_order_finish_failed` | Failed to finalize the swap order with the verification provider. |
| `9907` | `aml.transfer_metadata_missing` | Required transfer metadata was missing for the verification call. |
| `9908` | `aml.cpn_rfi_sync_failed` | Failed to sync the RFI (request-for-information) status with the verification provider. |
| `9909` | `aml.order_cancel_failed` | Failed to cancel the order with the verification provider. |
| `9910` | `aml.order_crypto_info_failed` | Failed to retrieve crypto order info from the verification provider. |
| `9911` | `aml.order_fiat_info_failed` | Failed to retrieve fiat order info from the verification provider. |
| `9912` | `aml.liveness_requirement_check_failed` | Failed to check whether a liveness check is required. |
| `9913` | `aml.liveness_token_request_failed` | Failed to request a liveness-check token. |
| `9914` | `aml.liveness_token_missing` | The provider reported success but returned no liveness token. |
| `9915` | `aml.user_info_fetch_failed` | Failed to fetch the customer's verification profile. |
| `9916` | `aml.user_parent_name_update_failed` | Failed to update the customer's parent/guardian name on file. |
| `9917` | `aml.meta_fetch_failed` | Failed to fetch verification metadata. |
| `9918` | `aml.v2_request_failed` | The (V2) verification API request failed. |
| `9919` | `aml.v2_response_missing_field` | The (V2) verification API response was missing a required field. |
| `9920` | `aml.submit_check_response_invalid` | The submission-check response from the provider was invalid. |
| `9921` | `aml.sumsub_token_request_failed` | Failed to request a Sumsub verification session token. |
| `9922` | `aml.sumsub_token_missing` | The provider reported success but returned no Sumsub session token. |
| `9923` | `aml.card_bind_failed` | Failed to notify the verification provider of a card-binding event. |
