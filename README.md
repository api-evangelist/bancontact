# Bancontact (bancontact)

<!-- API-EVANGELIST-PROVENANCE:BEGIN -->
> ### About this repository
>
> **This is not our API.** This repository is an independent, third-party profile of a company's
> **publicly available** API surface, maintained by [API Evangelist](https://apievangelist.com).
> API Evangelist does not operate, host, resell, or support this company's APIs, and is not
> affiliated with or endorsed by the company unless stated on the profile.
>
> **Where the information came from.** Everything here is assembled from material a member of the
> public can reach with a browser and no credentials — the company's own website, developer portal
> and documentation, the specifications it publishes for public use (OpenAPI, AsyncAPI, JSON Schema,
> `apis.json`, `llms.txt` and similar), its public repositories, and its public status, pricing and
> changelog pages. **Nothing here is obtained by breaching a system, defeating an access control, or
> using credentials of any kind.**
>
> **The rating is an independent assessment.** The Kin Score and Agent Readiness rating are
> independently calculated scores of a company's *public* API artifacts, produced by API Evangelist
> against a published rubric. They are not certifications, endorsements, security assessments, or
> audits, and they score published artifacts — not the quality, safety, or security of the software.
>
> **Corrections, re-scores, and removal are free.** No partnership, contract, or purchase is
> required, and you do not need to justify the request.
>
> - **Something wrong?** Open an issue on this repository, or email
>   [info@apievangelist.com](mailto:info@apievangelist.com).
> - **Published something new?** Ask for a re-score and we will re-run the rating.
> - **Want the listing taken down?** Say so and we will honor it. The profile is reduced to your
>   company name, a factual description, and a link to your own site, and the company is recorded as
>   **unrated** — never scored zero for having asked.
>
> **Response times.** Acknowledgement within **one business day**; removal or restriction within
> **two business days**; corrections and re-scores within **five business days**.
>
> **On a security or compliance team?** Email
> [info@apievangelist.com](mailto:info@apievangelist.com) with *security* in the subject line and
> you will get a person, not a form. We will tell you exactly which public URLs this profile was
> built from so your team can see the same surface we did, and we will take the listing down on
> request while you work through it.
>
> Full detail: **[Where this data comes from](https://apievangelist.com/about/where-our-data-comes-from)**
<!-- API-EVANGELIST-PROVENANCE:END -->

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
