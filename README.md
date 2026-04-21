# Bancontact (bancontact)
Bancontact is Belgium's most popular electronic payment system, operating through the Bancontact Payconiq Company (now transitioning to Bancontact Pro brand). The platform provides debit card payments, QR code payments, and mobile payments via the Payconiq by Bancontact app. The REST API enables merchants to accept payments online, in-app, and via QR codes with settlement in Belgian bank accounts.

**URL:** [Visit APIs.json URL](https://raw.githubusercontent.com/api-evangelist/bancontact/refs/heads/main/apis.yml)

**Run:** [Capabilities Using Naftiko](https://github.com/naftiko/fleet?utm_source=api-evangelist&utm_medium=readme&utm_campaign=company-api-evangelist&utm_content=repo)

## Tags:

 - Banking, Belgium, Debit Cards, E-Commerce, Fintech, Payments

## Timestamps

- **Created:** 2025-01-01
- **Modified:** 2026-04-21

## APIs

### Bancontact Payconiq Acceptance API
REST API for accepting Bancontact payments online and via QR code. Enables merchants to create payment transactions, generate QR codes, handle callbacks, and process refunds. Transitioning to Bancontact Pro branding in 2026.

**Human URL:** [https://docs.bancontactpro.com/](https://docs.bancontactpro.com/)

#### Tags:

 - Checkout, Payments, QR Code, Transactions, Refunds

#### Properties

- [Documentation](https://docs.bancontactpro.com/)
- [Legacy Payconiq Documentation](https://docs.payconiq.be/)

## Common Properties

- [Website](https://www.bancontact.com/)
- [Bancontact Pro Developer Portal](https://docs.bancontactpro.com/)

## Features

| Name | Description |
|------|-------------|
| Online Payments | Accept Bancontact debit card payments in e-commerce checkouts. |
| QR Code Payments | Generate QR codes for in-store and contactless payment acceptance. |
| Mobile App Payments | Payconiq by Bancontact app integration for mobile checkout. |
| Webhooks | Real-time payment status notifications via webhook callbacks. |
| Refunds | Programmatic refund processing for completed transactions. |
| Multi-currency | EUR-denominated payments with Belgian bank account settlement. |
| Deep Links | Mobile deep links to open the Payconiq app directly from merchant checkout. |

## Use Cases

| Name | Description |
|------|-------------|
| E-Commerce Checkout | Accept Bancontact as a local Belgian payment method at checkout. |
| QR Code POS | In-store and restaurant QR code payment acceptance. |
| Mobile In-App Payments | Integrate Bancontact into iOS and Android apps. |
| Invoice Payments | Payment links and QR codes for invoicing and B2C collections. |
| Subscription Billing | Recurring payment collection from Belgian consumers. |

## Integrations

| Name | Description |
|------|-------------|
| Adyen | Accept Bancontact via Adyen payment gateway. |
| Computop | Accept Bancontact via Computop payment platform. |
| PPRO | Access Bancontact via PPRO's local payment method network. |
| Stripe | Accept Bancontact via Stripe payment infrastructure. |
| Mollie | Accept Bancontact via Mollie payment service. |
| MultiSafepay | Accept Bancontact via MultiSafepay payment gateway. |

## Artifacts

Machine-readable API specifications organized by format.

### JSON-LD

- [Bancontact JSON-LD Context](json-ld/bancontact-context.jsonld)

## Capabilities

- [Bancontact Payment Capability](capabilities/bancontact-payment-capability.yaml) — Online checkout, QR code payment, and refund processing workflows

## Vocabulary

- [Bancontact Vocabulary](vocabulary/bancontact-vocabulary.yaml) — Taxonomy covering 5 resources, 6 actions, 3 workflows, and 3 personas for Belgian payment services

## Rules

- [Bancontact Spectral Rules](rules/bancontact-spectral-rules.yml) — 12 rules across 5 categories enforcing Bancontact API conventions

## Maintainers

**FN:** Kin Lane

**Email:** kin@apievangelist.com
