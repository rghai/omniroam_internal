# Omniroam security review

Reviewed 31 August 2026. This is an internal threat-model, static-control, dependency and regression review. It is not an independent penetration-test certificate.

## Launch verdict

Controlled testing can continue. The code now contains a founder-only canary that exactly matches the Thailand 100MB, seven-day, US$0.30 supplier package. It is restricted to Opn test mode, a protected founder build and an explicit supplier-write switch. No canary order has been placed. Public live sales remain blocked until the production environment, verified webhooks, reconciliation, 3-D Secure return, fraud controls and separate staff identity are complete.

## Controls implemented

- Same-origin checks and bounded JSON on public mutation routes.
- Durable, privacy-preserving rate-limit buckets.
- Server-side Turnstile verification plumbing on support, privacy and recovery forms.
- One-use recovery token exchange into a 15-minute HTTP-only, same-site session.
- New recovery credentials travel in the URL fragment rather than the request path. The browser removes the fragment before redeeming it.
- Installation email presents the private recovery address as plain text, so an email provider cannot wrap the bearer credential in a click-tracking redirect.
- No-referrer, noindex and no third-party script loading on private delivery routes.
- AES-256-GCM protection for sensitive eSIM activation values on new writes.
- Database and supplier idempotency, with one payment limited to one supplier order.
- HMAC-SHA256 signing for eSIMAccess requests.
- Dedicated eSIMAccess live-usage lookup with a profile-query fallback.
- Secret scanning and zero known production dependency vulnerabilities.
- Human-only refunds and separately gated supplier writes.
- Completed-order retries reuse the recorded payment and eSIM fulfilment. They cannot create a second charge, supplier order or installation email.
- QA reuse never stores the reserved supplier order number as if it belonged to a new customer order. The delivery row is verified after every write.
- The new supplier canary is limited to one catalogue entry and a maximum supplier price of US$0.50. The supplier package is re-read before purchase.
- A read-only live catalogue request on 31 August returned `TH_0.1_7` as Thailand 100MB, seven days and US$0.30. It created no supplier order.

## 30 August reserved-profile evidence

- Opn test charge: successful, AUD presentment, test mode confirmed.
- Supplier action: read-only retrieval of the existing reserved eSIMAccess profile.
- New supplier purchase: none.
- Delivery: protected record stored, private one-use recovery exchange passed, QR and quick-install controls rendered.
- Usage: 0 MB used and 100 MB remaining returned by the dedicated supplier endpoint.
- Email: first-party installation email sent through Resend and confirmed opened in the real founder inbox.
- Replay: the same checkout request returned the completed order without a second charge, eSIM order or email.
- Sensitive evidence: no live QR code, activation string, token or customer record was copied to GitHub, Notion or the public dashboard.

## Production-safe dynamic checks

Run against `https://omniroam.vercel.app` on 30 August 2026:

- Cross-origin checkout mutation rejected with HTTP 403.
- Oversized support payload rejected with HTTP 413.
- Invalid recovery token rejected with HTTP 400 and a non-sensitive error reference.
- Unauthenticated internal alert sweep failed closed because the feature is disabled.
- HTTPS, HSTS, frame denial, MIME sniffing protection, permissions policy, referrer policy and content security policy were present.
- The prior production deployment was READY. The current local regression suite passes 28 of 28 checks and the production build passes.

## P0 before live sales

1. Configure the production `SECURITY_HASH_SECRET` and `ESIM_DATA_ENCRYPTION_KEY`, then confirm the current migration in production.
2. Verify Opn and eSIMAccess webhook authenticity and replay handling.
3. Add payment, supplier and email reconciliation plus an operator mismatch queue.
4. Complete 3-D Secure returns and uncertain-payment recovery.
5. Add fraud rules and a chargeback and refund procedure.
6. Configure separate staff authentication and access logs.
7. Replace the current Resend testing sender with a verified sender on the next owned domain and a monitored reply inbox.
8. Arrange an independent penetration test before material scale. The work recorded here is an internal review, not independent certification.

## Temporary limitations

- Content security policy still permits inline styles and scripts.
- Four moderate findings remain in development-only tooling. The production audit is clean.
- Turnstile enforcement is off until production secrets and hostnames are configured.
- A public founder dashboard can hold non-secret progress only. Private approvals and customer data belong in access-controlled systems.
- Resend's testing sender can deliver only to the Resend account email. It is not a customer-delivery setup.
- Old recovery emails that contain a query-string token remain redeemable for compatibility. The browser clears that token immediately. New emails use fragment credentials.

## Live eSIMAccess usage evidence

- Existing reserved profile found on 30 August 2026.
- Supplier state: `GOT_RESOURCE`.
- Dedicated `/esim/usage/query` request succeeded.
- Reading: 0 MB used, 100 MB remaining.
- No order, profile change, payment or supplier spend occurred.
