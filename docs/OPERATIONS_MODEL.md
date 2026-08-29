# Omniroam operations model

Omniroam should run at Level 3 autonomy: routine work is performed or prepared automatically inside explicit limits, while founders approve commercial, legal, sensitive, and irreversible actions. Every automated action needs evidence, an audit record, and a recovery path.

## Shared operating layer

Use one small authenticated founder console backed by four durable records:

- **Case:** a customer or operational issue, its evidence, owner, priority, and resolution.
- **Action:** a proposed or completed tool action with risk class, inputs, approver, result, and rollback path.
- **Incident:** a service disruption with impact, timeline, mitigation, and follow-up.
- **Reconciliation exception:** a mismatch between payment, order, supplier, refund, and accounting records.

The event layer feeds these records without coupling business logic to a specific analytics, support, or messaging vendor.

## 1. Customer Support

### Tier 1 — automated self-service

- answer approved product, compatibility, installation, coverage, and policy questions;
- locate an order using a safe recovery flow;
- show payment, provisioning, installation, and delayed-usage status;
- resend an existing secure installation link;
- gather device, destination, timing, and troubleshooting evidence;
- hand off with a complete summary when confidence or permissions are insufficient.

### Tier 2 — automated investigation

- correlate payment, internal order, supplier order, eSIM, usage, and notification events;
- classify known payment, provisioning, installation, activation, network, and data issues;
- run approved read-only checks and safe idempotent retries;
- prepare a replacement, refund, or supplier-escalation recommendation;
- open and enrich a Tier 3 case instead of improvising outside a runbook.

### Tier 3 — human

Humans handle refunds or credits, supplier write actions outside approved runbooks, legal or safety complaints, suspected fraud, account takeover, high-value exceptions, and unresolved Tier 2 cases. The automation can prepare evidence and a recommended response but cannot approve its own sensitive action.

Support must never reveal QR payloads, activation codes, payment tokens, supplier credentials, or full identity data in logs or analytics.

## 2. Marketing and Growth

Founders should have a simple control plane for brand tokens, approved page sections, destination content, campaign pages, lifecycle copy, featured plans, and SEO metadata. Prices, costs, networks, and coverage remain catalog data—not pasted marketing prose.

Publishing workflow:

1. Edit a draft using approved components and tokens.
2. Generate desktop/mobile preview.
3. Check links, disclosures, accessibility, price references, expiry dates, and margins.
4. Apply the approval class.
5. Publish an immutable version with one-click rollback.
6. Record who changed what and which experiment or campaign it belongs to.

Low-risk copy, metadata, and broken-link fixes can be automated. Landing-page variants, lifecycle campaigns, competitor comparisons, global design changes, pricing, legal/payment content, and bulk messages require escalating levels of founder review.

PostHog is the initial analytics destination, behind the vendor-neutral event interface. The weekly founder dashboard should stay focused on qualified sessions, product selection, checkout completion, orders, revenue, contribution margin, provisioning success, support rate, refund rate, and repeat orders.

## 3. IT / Engineering Operations

The goal is low-touch operation, not no controls. Before enabling live automatic fulfilment, the system needs:

- durable database idempotency for checkout and supplier orders;
- verified webhook inbox and transactional outbox;
- bounded retries with backoff, circuit breakers, and a dead-letter queue;
- scheduled payment, order, supplier, and cost reconciliation;
- structured logs, traces, health checks, service-level indicators, and alerts;
- immutable audit history and secret redaction;
- tested runbooks for payment ambiguity, delayed provisioning, duplicate callbacks, supplier outage, stale usage, catalog anomalies, margin breaches, email failure, refund failure, and credential rotation.

Only actionable exceptions should wake a founder. Routine recovery, rechecking, and reconciliation should run automatically. A founder can pause checkout, quarantine a SKU, switch a provider route, or disable fulfilment without a code deployment.

## 4. Financial / Revenue Operations

Automate payment-to-order, order-to-supplier, refund-to-payment, and wholesale-cost reconciliation. Produce daily control totals and an exception queue for mismatches, missing records, duplicate attempts, abnormal fees, stale FX assumptions, or contribution-margin breaches.

Refund workflow:

1. Automation collects payment, fulfilment, activation/usage, policy, and prior-contact evidence.
2. It calculates the refundable amount and supplier-recovery likelihood.
3. A human approves, changes, or rejects the proposal.
4. The payment adapter executes the approved idempotent refund.
5. Webhook and polling independently confirm the outcome.
6. Ledger, order, customer message, and audit record update from the confirmed result.

No AI or support agent may change margin floors, approve its own refund, issue refund batches, alter tax treatment, or silently write off reconciliation differences.

## Delivery phases

1. **MVP foundation:** consistent events, correlation IDs, provider abstractions, safe demo mode, audit fields, margin guards, support-ready order status, and configuration placeholders.
2. **Live-order gate:** Postgres idempotency/inbox/outbox, verified webhooks, reconciliation jobs, alerts, runbooks, and a founder exception queue.
3. **Tier 1–2 automation:** knowledge retrieval, guided diagnostics, safe action catalogue, confidence thresholds, case summaries, and Tier 3 routing.
4. **Founder control plane:** content/design publishing, campaign experiments, pricing approvals, operational toggles, refund approvals, and finance reporting.
5. **Scale:** secondary suppliers, automated routing, lifecycle orchestration, deeper fraud controls, accounting integration, and stricter separation of duties.
