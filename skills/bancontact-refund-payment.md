---
generated: '2026-09-17'
method: generated
name: Refund a payment
description: Refund a SUCCEEDED Bancontact Pro payment idempotently through the Refund Service API, or fetch the debtor's IBAN for a manual SEPA refund.
api: openapi/bancontact-payment-refund-service-api-openapi.yml
operations: [create-refund, createRefund, getRefundById]
source: >-
  operationIds verified in openapi/bancontact-payment-refund-service-api-openapi.yml and
  openapi/bancontact-payment-v3-api-openapi.yml (create-refund = GET debtor refundIban); rules
  from https://docs.bancontactpro.com/guides/general/refunds052025.
---

# Refund a payment

Full or partial refunds are only possible against payments in status `SUCCEEDED`; the money is deducted from your next bulk payout, so the refund is refused if funds are insufficient.

## Auth
- `createRefund` and `getRefundById` require the ES256 detached JWS `Signature` **and** prior activation of the refund endpoint by Bancontact support (devsupport@bancontact.com / Account Manager). `create-refund` (the IBAN lookup) needs only the API key.

## Idempotency
- `createRefund` **requires** `Idempotency-Key` (≤ 64 chars, UUID recommended). Reuse the same key for every retry of a 4xx/5xx or timeout; use a new key only for a deliberate second refund. Reusing a key with different parameters returns 422 `REFUND_REQUEST_CONFLICT`. Coverage across the API is `partial` — this is the only idempotent write (`conventions/bancontact-conventions.yml`).

## Steps
1. **Confirm eligibility** — the payment must be `SUCCEEDED` (`merchant-get-payment` in the Payment V3 API).
2. **Create the refund** — `createRefund` (`POST /v3/payments/{payment-id}/refunds`) with `Idempotency-Key`, body `amount` (cents, ≤ remaining refundable), `currency: EUR`, `description`. Response `201`: `refundId`, `status: PENDING`, `creationDate`, plus a `SignatureBank` response header you should verify against Bancontact's JWKS.
3. **Track it** — `getRefundById` (`GET /v3/payments/{payment-id}/refunds/{refund-id}`) until `status` is `REFUNDED` or `FAILED`. Refunds appear in reconciliation the next day (`getRefunds`).
4. **Manual alternative** — if refunds are not activated, `create-refund` (`GET /v3/payments/{id}/debtor/refundIban`) returns the debtor IBAN for an out-of-band SEPA transfer; it moves no money and may be called repeatedly.

## Errors
- 422 `PAYMENT_FOR_REFUND_NOT_FOUND`, `INVALID_REFUND_AMOUNT`, `REFUND_NOT_ALLOWED` (not activated), `REFUND_NOT_POSSIBLE` (not SUCCEEDED / insufficient funds), `REFUND_REQUEST_CONFLICT`; 404 `REFUND_NOT_FOUND` / `PAYMENT_NOT_FOUND`; 422 `REFUND_NOT_AVAILABLE` on the IBAN lookup for P2P payments. Catalog: `errors/bancontact-problem-types.yml`.

## Notes
- No refund time window is published — do not assume one. A refund itself cannot be reversed.
- Consumer bank-statement text: `Bancontact Payconiq {refundId} {creditor name} {description} {reference}` (payout remittance guide).
