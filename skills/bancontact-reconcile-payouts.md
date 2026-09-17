---
generated: '2026-09-17'
method: generated
name: Reconcile payouts
description: Match the SEPA payouts credited to your IBAN with the Bancontact Pro payments and refunds they settle, using the Merchant Reconciliation API.
api: openapi/bancontact-merchant-reconciliation-api-openapi.yml
operations: [getPayoutList, getPayments, getRefunds]
source: >-
  operationIds verified in openapi/bancontact-merchant-reconciliation-api-openapi.yml; rules from
  https://docs.bancontactpro.com/guides/general/reconciliation052025 and
  https://docs.bancontactpro.com/guides/general/payoutremittance052025.
---

# Reconcile payouts

Reconciliation data is available **D+1 from 09:00 CET** in PREPROD and PROD. Bulked payouts land the next working day (Fri–Sun activity may arrive Monday/Tuesday depending on your bank).

## Auth
- ES256 detached JWS `Signature` header (`JWS-Request-Signature`), see `authentication/bancontact-authentication.yml`.

## Steps
1. **List payouts for a day** — `getPayoutList` (`GET /v3/reconciliation/payouts?date=yyyy-MM-dd&page=0&size=10000`). Each `PayoutListItem` gives `payoutId`, `iban`, `bulkId`, `payoutStatus`, `payoutDate`, `totalPayments`, `totalRefunds`, `totalPaymentAmount`, `totalRefundAmount`, `payoutAmount`.
2. **Match the bank line** — a bulked payout's remittance reads `YYYYMMDD-{bulkPayoutId}-{bulkId|NONE}-PQ-BulkRecon-{merchantId}`; an individual payout carries `Payconiq {paymentId} {merchant name} {reference} {description}`. Successful payments also expose `endToEndId`, equal to the End-to-end reference on your CAMT statement.
3. **Pull the payments behind it** — `getPayments` (`GET /v3/reconciliation/payments?payout-id=…`) or by `start-date`/`end-date` (both required, ≤ 30 days apart). Each `TransactionDetails` row has `paymentId`, `payoutId`, `paymentProfileId`, `paymentChannel` (ONLINE | INSTORE | INVOICE), `amount`, `reference`, `transactionDate`.
4. **Pull the refunds** — `getRefunds` (`GET /v3/reconciliation/refunds`) with the same filters; refunds are deducted from the bulk payout.
5. **Check the sum** — `payoutAmount == totalPaymentAmount - totalRefundAmount` per payout; page with zero-based `page` while `number < totalPages - 1`.

## Errors
- 400 `BAD_REQUEST` — bad date format or > 30-day range; 404 `PAYOUT_NOT_FOUND`; 401/403 key or signature problems; 500 `TECHNICAL_ERROR` — retry with backoff (`errors/bancontact-problem-types.yml`).

## Notes
- Default page `size` is 10000; there are no rate-limit headers, so back off on 429 (`rate-limits/bancontact-rate-limits.yml`).
- Use `bulkId` on payment creation to split payouts per till or shop.
