---
generated: '2026-09-17'
method: generated
name: Accept a QR / deeplink payment
description: Create a Bancontact Pro payment, present its QR code or deeplink to the consumer, and settle on the final status — cancelling if the consumer never confirms.
api: openapi/bancontact-payment-v3-api-openapi.yml
operations: [create, merchant-get-payment, cancel_payment, search]
source: >-
  operationIds verified in openapi/bancontact-payment-v3-api-openapi.yml; flow from
  https://docs.bancontactpro.com/guides/online/onlinesales and
  https://docs.bancontactpro.com/guides/instore/ondisplay052025v4.
---

# Accept a QR / deeplink payment

Create a dynamic payment for one product profile, show the consumer the QR code (in-store) or deeplink/checkout link (online), then act on the final status.

## Auth
- `Authorization: Bearer <per-product API key>` plus a `Signature` header carrying an ES256 detached JWS (see `authentication/bancontact-authentication.yml`). Test on `https://merchant.api.preprod.bancontact.net` first (`sandbox/bancontact-sandbox.yml`).

## Idempotency
- **None on payment creation.** `create` has no Idempotency-Key. Store the returned `paymentId` against your own `reference` before retrying; if a retry is in doubt, `search` by `reference` rather than creating again (`conventions/bancontact-conventions.yml`).

## Steps
1. **Create the payment** — `create` (`POST /v3/payments`) with `amount` (integer cents), `currency: EUR`, `description` and `reference` (SEPA extended character set, max 35), optional `bulkId`, `callbackUrl`, `returnUrl`. Response: `paymentId`, `status: PENDING`, `expireAt`, and `_links` — `qrcode.href`, `deeplink.href`, `checkout.href`.
2. **Present it** — in-store show `_links.qrcode`; online redirect to `_links.checkout` or open `_links.deeplink` (app-to-app, use a universal link as `returnUrl`). URLs are moving from `payconiq.com` to `pay.bancontact.net` (`lifecycle/bancontact-lifecycle.yml`); never hard-code them.
3. **Wait for the callback** — Bancontact POSTs a JWS-signed status to your `callbackUrl` (see the *Verify and acknowledge a payment callback* skill). Do not poll as your primary path.
4. **Confirm the status** — on callback (or if its signature cannot be verified) call `merchant-get-payment` (`GET /v3/payments/{id}`). Final statuses: `SUCCEEDED`, `AUTHORIZATION_FAILED`, `FAILED`, `CANCELLED`, `EXPIRED`, `VOIDED`.
5. **Cancel if abandoned** — while status is `PENDING` or `IDENTIFIED`, `cancel_payment` (`DELETE /v3/payments/{id}`) sets `CANCELLED`; otherwise the payment expires by itself after 20 minutes online / 2 minutes in-store. A confirmed payment cannot be cancelled (422 `PAYMENT_NOT_PENDING`) — refund it instead.

## Errors
- 401 `UNAUTHORIZED` / 403 `ACCESS_DENIED` — wrong key for the profile; 404 `PROFILE_NOT_FOUND`; 400 `FIELD_REQUIRED` / `BODY_MISSING`; 422 `UNABLE_TO_PAY_CREDITOR`; 429 — back off (no header published); 503 `TRY_AGAIN_LATER` — retry with delay. Catalog: `errors/bancontact-problem-types.yml`.
- A bank-side decline arrives only as status `AUTHORIZATION_FAILED` with no reason code (`errors/bancontact-decline-codes.yml`).

## Notes
- Static-QR points of sale use `create_static_qr_payment` (`POST /v3/payments/pos`) with `posId`; it invalidates any active payment for the same profile + POS.
- Reversibility: cancel before confirmation is `verified`; refund after success is `documented` with no stated window (`conventions/bancontact-conventions.yml#reversibility`).
