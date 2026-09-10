# Product go-live readiness

Updated 11 September 2026. This list covers product and technical readiness only. Legal, marketing and general operations are tracked separately.

## Current product verdict

Founder testing is ready for one controlled Thailand 100MB eSIM using an Opn test payment and real eSIMAccess credit. Public sales are not ready. Only that exact plan is connected to supplier fulfilment, customer accounts are not yet durable and production payment handling is incomplete.

## Critical path

| Order | Deliverable | Status | Owner | Dependency |
| --- | --- | --- | --- | --- |
| 1 | Keep one-payment-to-one-eSIM idempotency, encrypted credentials and exact supplier product validation | Implemented and tested | Engineering | None |
| 2 | Complete physical installation and connectivity on the unused Thailand 100MB canary | Founder action | KG or AM | A supported, unlocked device and the private order email |
| 3 | Connect every displayed purchasable plan to a current supplier product and validate cost, country, allowance and validity before charge | Blocker | Engineering | Approved launch catalogue and margin floor |
| 4 | Implement durable customer access with email magic link as the primary method | Blocker | Engineering after identity-provider choice and credentials | Identity service decision; domain can follow later |
| 5 | Add Google, Apple, Facebook and LINE as optional secondary sign-in methods | Post-MVP unless required for launch | Engineering after founder registrations | Separate provider app credentials and redirect URLs for each provider |
| 6 | Complete Opn 3-D Secure return, uncertain-payment recovery and production webhook event handling | Blocker for live card payments | Engineering after founder supplies production Opn access | Opn production account, keys and webhook registration |
| 7 | Add automated payment, supplier and email reconciliation with an authenticated exception queue | Blocker | Engineering | Durable staff authentication |
| 8 | Configure separate staff authentication, roles and access logs | Blocker | Founder chooses provider; engineering implements | Staff identity-provider tenant and approved domains |
| 9 | Add fraud holds, velocity limits, chargeback evidence and a human-approved refund workflow | Blocker | Engineering plus founder decisions | Risk thresholds and operator ownership |
| 10 | Verify customer email delivery from an owned sender domain and monitored reply inbox | Blocker | Founder configures domain; engineering verifies | A registered domain and Resend DNS records |
| 11 | Run the full release gate, dynamic production security smoke, desktop/mobile visual review and independent penetration test | Final gate | Engineering; independent tester for certification | All earlier product blockers complete |

## What engineering can complete without founder input

- Build the catalogue adapter so unavailable products cannot enter checkout.
- Finish account data models, magic-link token handling and provider-neutral account linking behind disabled configuration.
- Add reconciliation jobs, exception states, structured logs and non-sensitive alerts.
- Complete regression tests, internal security review, accessibility checks and content scans.
- Expand the first-party installation and usage experience without exposing supplier branding or activation data.

## What needs a founder roadblock removed first

- Opn production work needs the live merchant account and production credentials.
- Customer and staff authentication needs a selected provider and its tenant credentials.
- Customer email needs an owned domain, verified Resend sender and monitored support address.
- A broad live catalogue needs approved prices, margin floor and target launch destinations.
- Independent penetration testing needs an approved tester and budget.

## Founder-only actions

- Complete the physical eSIM installation and connectivity test.
- Approve risk thresholds, refund authority, staff roles and launch catalogue economics.
- Register and control the production domain, identity-provider apps and payment account.
- Approve any exception that affects payment, fulfilment, customer data, security or launch claims.

