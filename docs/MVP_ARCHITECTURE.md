# MVP architecture decision

Date: 2026-08-29

## Target production shape

Build one TypeScript modular monolith on Vercel with managed PostgreSQL as the system of record. Keep storefront, API routes, operations views, scheduled reconciliation, and webhooks in one deployable application, with explicit catalog, pricing, checkout, payments, fulfillment, events, notifications, and operations boundaries.

The cross-functional support, growth, engineering, and revenue workflow is specified in [OPERATIONS_MODEL.md](./OPERATIONS_MODEL.md).

The working preview is deliberately fake-by-default. It includes a real Opn test-mode seam but never orders live eSIM inventory after a test payment.

## Non-negotiable invariant

A paid and independently verified order may create exactly one provider order. Refreshes, retries, duplicate webhooks, deploys, and network timeouts may never create a second eSIM.

Production implementation therefore needs:

1. PostgreSQL unique constraints for public idempotency keys and provider transaction IDs.
2. Separate order, payment-attempt, and fulfillment state machines.
3. A durable webhook inbox and transactional event outbox.
4. Payment verification from the direct Opn response, retrieved charge state, and reconciliation. Browser redirects and webhooks alone are not proof.
5. A deterministic eSIMAccess `transactionId` reused for every retry.

## Provider facts that shape the design

- eSIMAccess has no sandbox. Live writes remain separately gated.
- Current documentation describes asynchronous allocation of roughly 30 seconds, an 8 request/second limit, idempotent duplicate `transactionId`, and usage that may lag 2–3 hours.
- The supplier webhook model requires event de-duplication and source verification/re-query. Polling remains the fallback.
- Opn tokenizes card data in the browser with the public key. The secret key stays server-side.
- Opn webhooks must be independently verified, and non-3DS successful charges may not emit `charge.complete`: https://docs.opn.ooo/api-webhooks
- AUD multi-currency must be enabled on the Thailand merchant account: https://docs.opn.ooo/multi-currency

## Current safe implementation

- Configurable brand, market, destination, price, supplier, payment, analytics, and support settings.
- Normalized launch catalog populated from verified wholesale rows, not supplier CSV fields rendered directly.
- Price-floor test coverage.
- Guest checkout UI with explicit compatibility and data-only confirmations.
- Opn browser tokenization and test charge adapter. Raw card data never reaches Omniroam.
- Demo eSIM fulfillment after a successful Opn test payment.
- Disabled webhook endpoints that fail closed until durable inbox and verification are configured.
- Vendor-neutral event envelope.
- eSIMAccess interface and live adapter boundary with writes disabled.

## Production acceptance criteria

- Same idempotency key and request returns the original result; a changed request returns conflict.
- A verified paid order creates at most one eSIMAccess transaction.
- Amount, currency, test/live mode, order metadata, and charge status are independently checked.
- Supplier provisioning retries re-query before writing again.
- Customer-visible usage includes a provider-updated timestamp and is never labelled live when delayed.
- No secret appears in the client bundle, repository, logs, analytics, or error responses.
- Founders can inspect failed payments, provisioning exceptions, catalog changes, price-floor failures, and agent activity.
