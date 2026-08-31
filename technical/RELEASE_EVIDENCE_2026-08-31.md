# Release evidence, 31 August 2026

This record contains non-sensitive evidence only. It excludes secrets, customer records, recovery credentials, QR codes and activation details.

## Result

The public demo and the protected founder canary code are ready to publish. The real canary has not run and no supplier purchase was made during this release.

The exact canary is Thailand, 100MB, seven days. The live eSIMAccess catalogue returned supplier code `TH_0.1_7` and a wholesale price of US$0.30 on 31 August 2026. The founder-facing test price is A$1.00.

## Release checks

| Check | Result |
| --- | --- |
| Automated regression suite | 28 of 28 passed |
| Lint | Passed |
| Next.js production build | Passed |
| Repository secret scan | Passed across 199 text files |
| Production dependency audit | Zero known vulnerabilities |
| Desktop visual review | Passed at 1440 by 1000 pixels |
| Mobile visual review | Passed at 390 by 844 pixels |
| Route review | Ten customer, legal, support, privacy and recovery routes returned HTTP 200 |
| Founder canary checkout | Opn card fields visible with no initial error |
| Recovery credential handling | Fragment removed before server redemption |

## Customer and security changes

- Production uses `https://omniroam.vercel.app` as the current public address.
- A future owned domain is a configuration change, not a code rewrite.
- The public Production deployment remains in demo mode with supplier writes disabled.
- The real supplier purchase is available only in a protected Vercel Preview with the founder test flag, Opn test mode and the supplier-write flag enabled together.
- A successful payment can create no more than one supplier order. A retry reuses the recorded payment and fulfilment.
- Installation email has HTML and plain-text versions. It remains readable when images are blocked.
- Private recovery credentials are placed in the URL fragment, shown as plain text in email and cleared before redemption.
- The header and footer use the simple Omniroam mark and wordmark.

## Canary status

The code path is complete, but the canary is not ready for founder top-up until the protected Preview environment has been configured and smoke-tested. Follow `docs/VERCEL_MANUAL_CHECKLIST.md`. Once that check passes, a founder may approve enough supplier balance for one US$0.30 order.

## Open launch blockers

1. Configure and protect the founder Preview environment.
2. Run one Opn test payment and one approved eSIMAccess purchase.
3. Confirm the newly issued profile in the founder inbox and on a compatible phone.
4. Replay the order and prove no duplicate charge, supplier order or email occurs.
5. Turn supplier writes off again after the test.
6. Complete webhooks, 3-D Secure recovery, fraud procedures, staff sign-in, alerts, legal operator details and independent penetration testing before public live sales.
