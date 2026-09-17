---
generated: '2026-09-17'
method: generated
name: Verify and acknowledge a payment callback
description: Receive Bancontact's JWS-signed payment status callback, verify it against the published JWKS, and acknowledge it when VOID mode requires.
api: openapi/bancontact-payment-v3-api-openapi.yml
operations: [callback, merchant-acknowledge, merchant-get-payment]
source: >-
  operationIds verified in openapi/bancontact-payment-v3-api-openapi.yml; rules from
  https://docs.bancontactpro.com/guides/general/callback052025 and the FAQ.
---

# Verify and acknowledge a payment callback

Bancontact calls your HTTPS `callbackUrl` with the `callback` operation (`POST`, body = `MerchantCallback`) on every status change. Order is not guaranteed and retries repeat identical content for up to 24 hours until you answer `200`.

## Auth (inbound)
- Header `signature`: detached JWS (RFC 7797), `alg ES256`, JOSE `kid` resolvable in `https://jwks.bancontact.net/` (PROD) or `https://jwks.preprod.bancontact.net/` (PREPROD). `crit` claims: `https://payconiq.com/sub` (your profile id), `/iss`, `/iat`, `/jti`, `/path`.
- `user-agent: Bancontact Payments/v3`, `content-type: application/json`.

## Steps
1. **Verify the signature** — rebuild `base64url(JOSE header) + '.' + base64url(raw body)` and verify with the JWK whose `kid` matches. Cache JWKS for at most 12 h; on a `kid` miss re-fetch once and retry (keys rotate without notice, new key published 24 h before removal).
2. **Check the claims** — `sub` equals your PaymentProfileId; `path` equals your callback URL; reject otherwise.
3. **Deduplicate** — key on `paymentId` + `status`; retries are byte-identical.
4. **Fallback when verification fails** — call `merchant-get-payment` (`GET /v3/payments/{id}`) and trust that status, per the FAQ.
5. **Answer `200` fast** — within 15 s (5 s if sync-callback mode is configured). In sync mode a 4xx/5xx or timeout *rejects* the payment; after 3 failures Bancontact marks it `FAILED`.
6. **Acknowledge when required** — if status is `PENDING_MERCHANT_ACKNOWLEDGEMENT` (VOID active), call `merchant-acknowledge` (`POST /v3/payments/{id}/acknowledge`); otherwise the payment becomes `VOIDED`.

## Errors
- Your endpoint returning 429/500/503/504/509 triggers Bancontact retries (`asyncapi/bancontact-callbacks-webhooks.yml`).
- `merchant-acknowledge`: 400 `BODY_MISSING`, 404 `PAYMENT_NOT_FOUND` (`errors/bancontact-problem-types.yml`).

## Notes
- Only EUR; amounts are integer cents; `debtor.iban` is masked in the callback.
