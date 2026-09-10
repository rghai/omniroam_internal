# Omniroam security review

Reviewed 10 September 2026. This is an internal threat-model, static-control, dependency and regression review. It is not an independent penetration-test certificate.

## Launch verdict

Controlled founder testing can continue. On 10 September the live Vercel MVP passed one Opn test payment and created one Thailand 100MB, seven-day eSIMAccess profile at US$0.30. The private page returned a real QR code, manual details, Apple and Android installation actions, supplier backup page and a live 100MB remaining reading. The recovery credential disappeared from the address bar before redemption. Supplier writes were then switched off and Production was redeployed. Public live sales remain blocked until verified webhook authenticity, reconciliation, 3-D Secure return, fraud controls, a verified sender and separate staff identity are complete.

## Controls implemented

- Same-origin checks and bounded JSON on public mutation routes.
- Durable, privacy-preserving rate-limit buckets.
- Server-side Turnstile verification plumbing on support, privacy and recovery forms.
- One-use recovery token exchange into a 24-hour HTTP-only, same-site session after a deliberate human click.
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

## 10 September Production canary evidence

- The live catalogue recheck confirmed the exact Thailand 100MB, seven-day product at US$0.30 before purchase.
- One Opn test charge completed and one new supplier order reached `GOT_RESOURCE`.
- The private delivery page showed the real QR code, native installation actions, backup supplier page, manual activation section and 100MB remaining.
- The recovery fragment was removed before the private page loaded. The former internal token-handling message did not appear.
- The checkout returned no installation-email failure. Resend accepted the send from the configured testing sender. Recipient-side inbox confirmation remains with the founder because this browser session has no Gmail access.
- The profile remains unused for the founder’s physical-device installation test. It was not cancelled or revoked.
- `ESIMACCESS_WRITE_ENABLED` was restored to `false` and Production was redeployed after the one supplier order.
- Activation credentials, QR content and the recovery credential were excluded from repository, Notion and public dashboard evidence.

## Production-safe dynamic checks

Run against `https://omniroam.vercel.app` again on 11 September 2026:

- Cross-origin checkout mutation rejected with HTTP 403.
- Oversized support payload rejected with HTTP 413.
- Invalid recovery token rejected with HTTP 400 and a non-sensitive error reference.
- Unauthenticated internal alert sweep failed closed because the feature is disabled.
- HTTPS, HSTS, frame denial, MIME sniffing protection, permissions policy, referrer policy and content security policy were present.
- Sensitive `/.env` and `/.git/config` paths returned HTTP 404.
- The eSIMAccess `CHECK_HEALTH` request returned HTTP 200 without business processing, and an unsupported event returned HTTP 400.
- The current Production deployment is READY. The current regression suite passes 50 of 50 checks, the production build passes and the dynamic security smoke passes.
- Mobile browser regression passed nine live routes at 390 by 844 with no horizontal overflow, broken image, framework overlay or flagged internal wording.

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
- Old recovery emails that contain a query-string token remain redeemable for compatibility. The browser clears that token immediately. New emails use fragment credentials and wait for a human click before redemption.

## Live eSIMAccess usage evidence

- Existing reserved profile found on 30 August 2026.
- Supplier state: `GOT_RESOURCE`.
- Dedicated `/esim/usage/query` request succeeded.
- Reading: 0 MB used, 100 MB remaining.
- No order, profile change, payment or supplier spend occurred.
