# Vercel manual configuration checklist

This is the single founder checklist for the Omniroam Vercel project. Do it after the current implementation sprint, not piecemeal. Never paste secrets into GitHub, Notion or the founders’ public page.

## Production settings needed for the controlled MVP

### Public product settings

| Variable | Production value or action |
| --- | --- |
| NEXT_PUBLIC_SITE_URL | https://omniroam.vercel.app until goomniroam.com is connected |
| NEXT_PUBLIC_CHECKOUT_MODE | opn_test during controlled testing |
| NEXT_PUBLIC_DEFAULT_MARKET | AU |
| NEXT_PUBLIC_DEFAULT_CURRENCY | AUD |
| NEXT_PUBLIC_CONSENT_MODE | opt-out during testing |
| NEXT_PUBLIC_CONSENT_PREFERENCES_DEFAULT | granted |
| NEXT_PUBLIC_CONSENT_ANALYTICS_DEFAULT | denied |
| NEXT_PUBLIC_CONSENT_MARKETING_DEFAULT | denied |
| NEXT_PUBLIC_ANALYTICS_ENABLED | false while GA work is paused |
| NEXT_PUBLIC_GTM_ID | Leave blank while GA work is paused |

### Payments

| Variable | Action |
| --- | --- |
| PAYMENT_PROVIDER | opn |
| NEXT_PUBLIC_OPN_PUBLIC_KEY | Add the Opn test public key |
| OPN_SECRET_KEY | Add the Opn test secret as a protected value |
| OPN_TEST_CHARGES_ENABLED | true for the controlled test only |
| OPN_WEBHOOK_SECRET | Leave blank until the webhook endpoint and signature check are ready |

### eSIM fulfilment

| Variable | Action |
| --- | --- |
| ESIM_PROVIDER | esimaccess |
| ESIM_PROVIDER_PRODUCT_MAP_JSON | Add the approved plan-to-supplier product map as a protected value |
| ESIM_PROVIDER_COST_MAP_JSON | Add current wholesale costs as a protected value |
| ESIMACCESS_API_BASE_URL | Add the documented API base URL |
| ESIMACCESS_ACCESS_CODE | Add as a protected value |
| ESIMACCESS_SECRET_KEY | Add as a protected value if the selected API operation requires it |
| ESIMACCESS_WRITE_ENABLED | false until the live-order gate is approved |
| ESIMACCESS_EXPERIMENT_MAX_USD | 0.50 during experiments |
| ESIMACCESS_WEBHOOK_SECRET | Leave blank until webhook verification is implemented |

### Database

| Variable | Action |
| --- | --- |
| DATABASE_URL | Keep the Neon pooled production connection |
| DATABASE_URL_UNPOOLED | Keep the Neon direct migration connection |

### Transactional email

| Variable | Action |
| --- | --- |
| RESEND_API_KEY | Add as a protected value |
| RESEND_EMAIL_DOMAIN | goomniroam.com after Resend reports the domain verified |
| TRANSACTIONAL_EMAIL_FROM | Omniroam orders address after domain verification |
| TRANSACTIONAL_EMAIL_REPLY_TO | Use a real monitored support inbox |
| TRANSACTIONAL_EMAIL_ENABLED | Keep false until the domain and reply inbox are verified, then change to true |
| SUPPORT_EMAIL | Use the same monitored support address |

Resend currently reports goomniroam.com as not_started. Add its SPF and DKIM records in DNS, then add a DMARC monitoring record. Disable open and click tracking for transactional mail.

## Keep off until production work resumes

- Google Analytics and Google Tag Manager
- eSIMAccess supplier writes
- automatic refunds
- advertising tags
- internal staff dashboard access
- scheduled alert sweeps
- Opn live charges
- public checkout on the custom domain

## Production-only configuration to complete later

- Connect goomniroam.com to the Vercel project and set the canonical site URL.
- Add Opn 3-D Secure return handling and verified webhook settings.
- Add verified eSIMAccess webhook settings and reconciliation.
- Add Resend delivery webhooks and their signing secret.
- Configure a separate staff identity provider and staff domain allowlist.
- Set monitored support, privacy, DSAR and finance alert recipients.
- Add the scheduler secret only when the alert sweep is enabled.
- Rotate every development key that has appeared in chat before live sales.

## Cloudflare later

Cloudflare is not required for the current MVP. When production hardening resumes:

- move or confirm DNS management
- connect the custom domain
- add Turnstile to high-abuse public forms
- store the Turnstile secret only on the server
- do not add Turnstile to ordinary checkout unless measured abuse justifies the extra friction

## After changing Vercel variables

Redeploy once so the new values reach the production build. Then run one controlled order covering payment, fulfilment, installation email, recovery and duplicate-request protection.
