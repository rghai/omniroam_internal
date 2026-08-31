# Omniroam release QA and QC gate

Run this gate against every customer-facing release. A release is not complete because it builds. It is complete when the evidence below is recorded and any exceptions have the right approval.

## Owners

- AM: Akhil Modi, founder.
- KG: Karan Ghai, founder.
- Engineering owner: the person or agent preparing the release.

Founder email addresses are kept out of the public checklist. Use the approved private contact record for notifications and approval evidence.

## Automated gate

Run `npm run qa:release`. GitHub Actions runs the same command on pull requests and every push to `main`.

- ESLint passes.
- All Node regression tests pass.
- The Next.js production build passes.
- Customer copy contains no em dashes.
- AUD remains the binding base price.
- Analytics and marketing code respect the configured consent defaults.
- Checkout uses the private supplier registry and never passes the customer email to a supplier.
- Email delivery and supplier operations preserve idempotency.
- A completed-order retry cannot create another charge, supplier order or email.
- A reused QA eSIM must create a delivery row for the current order without claiming the supplier order identifier, and checkout must fail if that row is missing.
- Brand masters, the small-size mark and all six product character assets exist.
- Fixed watercolour scenes and the retired koala badge are not restored.

## Browser and visual gate

Check at least one desktop viewport of 1,280 pixels or wider and one phone viewport between 360 and 430 pixels.

- Home page has meaningful content and no framework error overlay.
- Browser console has no errors.
- Every image completes with a non-zero natural width and height after scrolling through the page.
- No horizontal overflow.
- Header and favicon mark remain recognisable at 32, 43 and 96 pixels.
- Characters do not cover headings, body copy, prices, controls or legal conditions.
- Characters retain complete faces, ears, props and feet unless an approved crop is deliberate.
- Colour contrast, focus rings, keyboard navigation and reduced-motion treatment remain usable.
- Checkout, order recovery, support, privacy request, legal pages and consent controls receive a visual spot check when touched by the release.
- The owned installation guide works at desktop and phone sizes, every device tab is reachable and no third-party image is required.

## Product and operations gate

- One successful payment can create no more than one order and one supplier fulfilment attempt.
- Supplier writes remain off unless the release has explicit approval to enable them.
- Refund execution remains human-only.
- Test and production environments cannot silently share live keys or writable supplier modes.
- Support and DSAR requests create durable references and do not expose free text in analytics.
- Staff access remains separate from customer access and fails closed when staff identity is not configured.
- Legal operator details, monitored inboxes and launch placeholders are checked before any production launch claim.
- No credential, QR code, activation string, customer record or private founder contact is committed to GitHub or copied into release evidence.
- Recovery links are treated as bearer credentials. They use no-referrer, noindex and no-store behaviour, and no unrelated third-party script loads on the token-bearing route.
- New recovery links place the bearer credential in the URL fragment. The browser removes it before redemption and exchanges it for a 15-minute HTTP-only, same-site cookie.
- Installation emails show the private recovery address as plain text. The bearer credential must not be an HTML link that an email click-tracker can wrap.
- The email remains readable with images blocked and includes an equivalent plain-text part.
- The private delivery page retrieves live usage from eSIMAccess `/esim/usage/query` using the supplier transaction number, not the customer-visible order reference.
- A temporary usage-endpoint failure falls back to the latest profile reading without exposing supplier names, identifiers or internal errors.
- Every eSIMAccess request carries the access code, timestamp, unique request ID and HMAC-SHA256 signature required by the supplier.

## Feedback regression register

Every founder comment that changes the product creates or updates a row in the register.

| Date | Feedback | Resulting regression check | Status |
| --- | --- | --- | --- |
| 29 Aug 2026 | Characters loaded only halfway on mobile | Inspect complete character bounds at 360 to 430 pixels | Fixed and checked |
| 29 Aug 2026 | Characters felt pasted into unrelated boxes | Confirm each character has a journey role and no fixed scene dependency | Fixed and checked |
| 29 Aug 2026 | Art looked like generic geometric mascot construction | Compare new assets with the approved bright sticker specification | Fixed and checked |
| 29 Aug 2026 | Watercolour art fought the vibrant product palette | Product test rejects the retired fixed scene references | Fixed and automated |
| 29 Aug 2026 | Koala pin resembled a US State Park badge | Check the abstract O mark at favicon sizes and reject shield or pin silhouettes | Fixed and checked |
| 29 Aug 2026 | Copy must avoid em dashes and generic AI phrasing | Automated em-dash test plus editorial review of changed customer copy | Active |
| 29 Aug 2026 | Installation help must live on Omniroam rather than depend on a supplier or competitor page | `/install` route test, device-tab review and owned-illustration check | Active |
| 29 Aug 2026 | Hero should use the brand navy rather than flat black | Shared `--navy` token keeps hero, education and installation panels consistent | Active |
| 29 Aug 2026 | Header and footer looked unfinished and the wordmark needed more character | Keep the approved abstract mark plus a plain Omniroam wordmark. Do not restore character crowding without a new approved design | Active |
| 29 Aug 2026 | Recovery bearer token was visible in the URL | New links use a fragment, remove it before redemption and exchange it for a short HTTP-only cookie. Old query links remain compatible and are cleared immediately | Fixed and automated |
| 30 Aug 2026 | Live usage must use eSIMAccess data | Test the dedicated usage endpoint, signed request headers, byte conversion and safe profile-query fallback | Active |
| 30 Aug 2026 | Checkout displayed a generic failure after a successful test payment | Require a stored delivery row, repair an earlier missing QA row, retry email safely and return a customer error reference | Fixed and automated |
| 30 Aug 2026 | Support displayed “request protection is not configured” | Keep the testing fallback isolated from live mode and include support submission in production smoke checks | Fixed and automated |
| 30 Aug 2026 | Founders received failed release and deployment notices | Require both the GitHub release gate and the Vercel production deployment to finish successfully before calling a release complete | Active |
| 31 Aug 2026 | The owned domain expired | Keep the Vercel domain as the configured MVP default and verify that domain replacement requires environment changes rather than code changes | Active |
| 31 Aug 2026 | Installation email images were blocked and its link pointed to localhost | Require image-independent layout, a plain-text part, production fallback to the Vercel origin and a non-clickable fragment recovery address | Fixed and automated |
| 31 Aug 2026 | Character crowding made the logo worse | Require the simple mark plus Omniroam lockup in the header and footer until founders approve a stronger master | Fixed and automated |
| 31 Aug 2026 | Australia-first had not been justified with raw numbers | Publish official outbound-volume figures and describe Australia as a measured acquisition hypothesis rather than an objective winner | Fixed and documented |
| 31 Aug 2026 | A real supplier test must use the cheapest exact package | Founder-only catalogue entry must remain Thailand, 100MB, seven days, US$0.30 and supplier writes require four independent gates | Active |

### 30 August live supplier-read evidence

- Existing reserved profile found without creating or modifying an order.
- Supplier state returned `GOT_RESOURCE`.
- Dedicated `/esim/usage/query` call succeeded with 0 MB used and 100 MB remaining.
- No supplier write, payment or account top-up occurred.
- A separate bounded end-to-end test later passed an Opn sandbox payment, protected delivery, one-use recovery, live usage and first-party email using the same reserved eSIM. No new supplier order was created.
- The real QR and activation details were inspected privately and excluded from screenshots, email evidence and public records.
- Production customer-session verification remains open until the Vercel environment is configured for controlled Opn test mode.

### 31 August canary catalogue evidence

- A signed read-only live package-list request returned `TH_0.1_7` as Thailand 100MB for seven days at US$0.30.
- The next-cheapest suitable result was Thailand 500MB for seven days at US$0.40.
- The canary remains the cheaper US$0.30 plan.
- No supplier order, payment, profile change or account top-up occurred.
- The live package and price must be checked again immediately before the approved purchase.

Add new feedback below these entries. Link it to a test, visual check, product invariant or explicit accepted limitation.

## Exceptions and approval

The engineering owner may log and accept a small exception only when all of these are true:

- it is visual or editorial rather than financial, legal, privacy, security or fulfilment-related;
- it is reversible in one release;
- it does not hide a limitation or reduce accessibility;
- the reason and follow-up date are recorded.

AM or KG approval is required before release when an exception affects payment, pricing logic, supplier writes, refunds, customer data, consent, legal wording, authentication, security controls, production launch claims or a standing founder decision.

If a new proposal conflicts with an earlier QA check, record the conflict, why the old rule no longer fits, the risk introduced and the evidence supporting the change. Simple presentation changes may use the small-exception rule. All other conflicts wait for founder approval.

## Release evidence template

- Release or commit:
- Date and operator:
- Automated gate result:
- Desktop viewport and result:
- Mobile viewport and result:
- Routes checked:
- Payment and fulfilment mode:
- Privacy and operations checks:
- Feedback rows added or changed:
- Exceptions:
- Founder approval, if required:
- Production URL checked after deployment:
