# Configuration guide

The customer application reads customer-facing choices from dedicated brand, business, and catalogue configuration. A rename, market change, currency change, destination priority, or launch-price update should not require rewriting the storefront.

Production secrets belong in the hosting provider's encrypted environment settings. This public repository contains documentation only and must never contain live or test credentials.

## Safety modes

- `demo`: no payment and no supplier order.
- `opn_test`: Opn test tokenization and a test charge; demo eSIM fulfillment only.
- No live checkout mode is implemented. Production enablement requires PostgreSQL idempotency, durable webhook inbox/outbox, charge reconciliation, and order recovery.
- eSIMAccess writes remain disabled by default. A bounded supplier experiment may be enabled only with an explicit spend cap and a selected package at or below that amount.

## Secret placeholders

- `ESIMACCESS_ACCESS_CODE`
- `ESIMACCESS_SECRET_KEY`
- `NEXT_PUBLIC_OPN_PUBLIC_KEY` (a test public key may be browser-visible)
- `OPN_SECRET_KEY`
- `OPN_WEBHOOK_SECRET`
- `DATABASE_URL`

Never prefix a secret with `NEXT_PUBLIC_`.

## Founder-editable configuration

The intended control plane exposes safe, previewable settings for:

- brand name, promise, colours, typography, and mascot assets;
- launch country, currency, and market-specific copy;
- featured destinations and merchandising order;
- retail prices subject to contribution floors;
- provider selection and independent write switches;
- campaign pages, lifecycle copy, and SEO metadata;
- operational pause, quarantine, refund-approval, and escalation controls.

Every published change should keep an immutable version, show who approved it, and support one-click rollback.

## Google analytics

- Google Tag Manager container: `GTM-WJ62HWC6`
- Google Analytics 4 property: `G-G0PSHS6130`
- Account owner: `karanghai@gmail.com`

Google Analytics is configured inside Google Tag Manager. Do not add the direct Google Analytics script to the customer application. GTM is consent-gated through the analytics cookie category. The no-script iframe is deliberately omitted because it cannot honour a visitor's banner choice.
