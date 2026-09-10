# eSIM supplier portability

## Current decision

Checkout, payment, email, recovery and customer pages depend on Omniroam’s provider contract rather than an eSIMAccess class.

Each supplier adapter must translate its API into these operations:

1. list packages
2. create one order with an idempotency key
3. retrieve an order
4. retrieve usage

The adapter returns one canonical eSIM record with a provider order ID, status, activation details, QR location, ICCID, APN and optional supplier-hosted backup page. Omniroam derives the Apple and Android native installation links from the canonical activation string, so those customer actions do not depend on a supplier-branded page.

## What changes when a supplier changes

- Add one adapter implementing the EsimProvider contract.
- Register it in lib/providers/registry.ts.
- Add that provider’s product reference to each plan in the catalogue.
- Add its protected credentials and feature switches.
- Map its statuses, errors, price units and usage units into the canonical types.
- Run the provider contract, duplicate-order, recovery and reconciliation tests.

Checkout, Opn payment, transactional email, customer recovery, support and the customer-facing order page should not change.

## Honest limitation

A supplier change cannot usually be reduced to replacing an endpoint. Providers use different authentication, product identifiers, price units, order states, webhook signatures, QR formats, refund rules and usage responses. The adapter contains those differences. A well-documented REST supplier with the same capabilities should normally require days rather than a storefront rebuild, but the exact effort depends on its API and commercial rules.

## Remaining work before calling it fully portable

- Add a provider contract test suite that every real adapter must pass.
- Replace the legacy supplier_slug database name with a neutral product-reference table during a planned migration.
- Add capability flags for top-up, phone-number plans, cancellation, revocation and supplier refunds.
- Normalise supplier webhooks into one fulfilment-event contract.
- Add reconciliation jobs that compare Omniroam orders with the active supplier.
- Prove a second adapter in a sandbox before relying on portability in production.

## Current canary boundary

The first real-purchase canary uses eSIMAccess behind the same provider contract. Checkout selects a founder-only Omniroam plan. Customers never choose or see the supplier. The registry resolves the configured supplier on the server, the private product map translates the Omniroam plan ID, and the adapter returns the same canonical fulfilment record used by email, recovery, QR display and usage lookup.

The canary does not make portability proven. It proves that the customer journey is outside the eSIMAccess adapter. A second supplier implementation and contract-test run remain necessary evidence.

## Lifecycle operations boundary

Cancellation and revocation are not interchangeable:

- Cancellation is for an eligible unused profile. The operator must verify supplier status before submitting it. A supplier credit and a customer payment refund are separate records and separate decisions.
- Revocation permanently disables a profile and may not return supplier credit. It always requires a human decision and an audit note.
- Tier 1 and Tier 2 automation may retrieve status, usage and diagnostics. It may prepare a cancellation recommendation, but it must not cancel, revoke or refund.
- Tier 3 may use the supplier console during the MVP. A later internal tool must still call the provider adapter rather than embed eSIMAccess logic in customer or support code.

The next adapter revision will add explicit capability declarations and human-authorised lifecycle methods after the exact eSIMAccess request and response contracts have been verified from its current API documentation. Placeholder methods must not be presented as working controls.
