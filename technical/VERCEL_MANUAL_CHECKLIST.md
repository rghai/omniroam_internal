# Vercel manual configuration checklist

This is the single founder checklist for the Omniroam Vercel project as at 31 August 2026. Use a protected Vercel Preview deployment for the first real supplier canary. Keep the public Production deployment in demo mode.

Never paste secrets into GitHub, Notion, the founder dashboard or a screenshot. Add protected values only in Vercel Project Settings, then redeploy the affected environment once.

## Environment split

| Environment | Purpose | Checkout | Supplier writes |
| --- | --- | --- | --- |
| Production | Public working MVP at `https://omniroam.vercel.app` | Demo | Off |
| Preview | Founder-only end-to-end canary | Opn test charge | One approved US$0.30 eSIM |
| Development | Local engineering | Demo unless deliberately testing | Off |

Turn on Vercel Deployment Protection for the canary Preview. Do not expose a supplier-writable build as the public Production site.

## Production values

| Variable | Value or action |
| --- | --- |
| `NEXT_PUBLIC_SITE_URL` | `https://omniroam.vercel.app` |
| `NEXT_PUBLIC_CHECKOUT_MODE` | `demo` |
| `NEXT_PUBLIC_FOUNDER_TEST_FLOW` | `false` |
| `PAYMENT_PROVIDER` | `demo` |
| `OPN_TEST_CHARGES_ENABLED` | `false` |
| `ESIM_PROVIDER` | `demo` |
| `ESIMACCESS_WRITE_ENABLED` | `false` |
| `TRANSACTIONAL_EMAIL_ENABLED` | `false` unless a demo email is deliberately required |
| `NEXT_PUBLIC_ANALYTICS_ENABLED` | `false` while analytics work is paused |
| `NEXT_PUBLIC_GTM_ID` | Leave blank while analytics work is paused |
| `LOG_LEVEL` | `warn` |

Keep the current Australia and AUD defaults. These are acquisition-test defaults, not a permanent commitment to an Australia-only product.

## Protected Preview for one real canary

### Public build values

| Variable | Preview value |
| --- | --- |
| `NEXT_PUBLIC_SITE_URL` | Use the exact protected Preview URL after it exists, for example `https://<preview>.vercel.app` |
| `NEXT_PUBLIC_CHECKOUT_MODE` | `opn_test` |
| `NEXT_PUBLIC_FOUNDER_TEST_FLOW` | `true` |
| `NEXT_PUBLIC_DEFAULT_MARKET` | `AU` |
| `NEXT_PUBLIC_DEFAULT_CURRENCY` | `AUD` |
| `NEXT_PUBLIC_ANALYTICS_ENABLED` | `false` |
| `NEXT_PUBLIC_GTM_ID` | Leave blank |
| `LOG_LEVEL` | `info` |

### Opn test payment

| Variable | Preview value or action |
| --- | --- |
| `PAYMENT_PROVIDER` | `opn` |
| `NEXT_PUBLIC_OPN_PUBLIC_KEY` | Add the Opn test public key |
| `OPN_SECRET_KEY` | Add the Opn test secret as a protected server value |
| `OPN_TEST_CHARGES_ENABLED` | `true` |
| `OPN_TEST_REUSE_EXISTING_ESIM` | `false` for the new canary purchase |
| `OPN_WEBHOOK_SECRET` | Leave blank until signed webhooks are implemented |

Use only an official Opn test card. The standard success card is `4242 4242 4242 4242`. Use any future expiry and any CVV in test mode. This card never creates a real card charge.

### eSIMAccess canary

| Variable | Preview value or action |
| --- | --- |
| `ESIM_PROVIDER` | `esimaccess` |
| `ESIMACCESS_API_BASE_URL` | Add the documented API base URL |
| `ESIMACCESS_ACCESS_CODE` | Add as a protected server value |
| `ESIMACCESS_SECRET_KEY` | Add as a protected server value |
| `ESIMACCESS_WRITE_ENABLED` | `true` only for the protected canary Preview |
| `ESIMACCESS_EXPERIMENT_MAX_USD` | `0.50` |
| `ESIM_PROVIDER_PRODUCT_MAP_JSON` | `{"esimaccess":{"qa-th-0.1-7":"TH_0.1_7"}}` |
| `ESIM_PROVIDER_COST_MAP_JSON` | `{"esimaccess":{"qa-th-0.1-7":0.3}}` |
| `ESIMACCESS_WEBHOOK_SECRET` | Leave blank until signed webhooks are implemented |

The supplier map was checked read-only against the live eSIMAccess package list on 31 August 2026. `TH_0.1_7` returned Thailand 100MB, seven days and US$0.30. Check it again immediately before purchase. The code fetches the package, checks the exact supplier price against the US$0.50 cap and uses the checkout idempotency key as the supplier transaction ID. A retry reuses the recorded payment and supplier order.

### Database and protection

| Variable | Preview value or action |
| --- | --- |
| `DATABASE_URL` | Add the Neon pooled connection |
| `DATABASE_URL_UNPOOLED` | Add the Neon direct migration connection |
| `SECURITY_HASH_SECRET` | New random value of at least 32 bytes |
| `ESIM_DATA_ENCRYPTION_KEY` | Base64 text representing exactly 32 random bytes |
| `NEXT_PUBLIC_TURNSTILE_ENABLED` | `false` for the protected founder canary unless the widget is fully configured |
| `TURNSTILE_ENFORCED` | `false` for this canary. Turnstile is not required on checkout |

Do not weaken same-origin checks, payload limits, idempotency or encrypted eSIM storage to make the canary pass.

### Transactional email for founder testing

| Variable | Preview value or action |
| --- | --- |
| `RESEND_API_KEY` | Add as a protected server value |
| `RESEND_EMAIL_DOMAIN` | Leave blank while using Resend's testing sender |
| `TRANSACTIONAL_EMAIL_FROM` | `Omniroam <onboarding@resend.dev>` |
| `TRANSACTIONAL_EMAIL_REPLY_TO` | Leave blank or use the monitored temporary support inbox |
| `TRANSACTIONAL_EMAIL_ENABLED` | `true` only when the order email is the Resend account owner's email |
| `EMAIL_SUPPORT_URL` | `https://omniroam.vercel.app/support` |
| `EMAIL_COMPANY_DETAILS` | `Omniroam testing service · Legal operator details pending before launch` |
| `SUPPORT_EMAIL` | Use the monitored temporary support inbox |

Resend's `onboarding@resend.dev` sender can send only to the email address attached to the Resend account. It is suitable for the founder canary, not customer delivery. The email includes a plain-text version and remains understandable when images are blocked. Its private recovery address is plain text so click tracking cannot wrap the bearer credential.

## Domain change later

No code change is required when a replacement domain is registered. Update these settings, verify the new sender domain in Resend, then redeploy:

- `NEXT_PUBLIC_SITE_URL`
- `NEXT_PUBLIC_SUPPORT_EMAIL`
- `NEXT_PUBLIC_PRIVACY_EMAIL`
- `TRANSACTIONAL_EMAIL_FROM`
- `TRANSACTIONAL_EMAIL_REPLY_TO`
- `EMAIL_SUPPORT_URL`
- `EMAIL_COMPANY_DETAILS`
- `TURNSTILE_ALLOWED_HOSTNAMES`

Add the new domain to Vercel, configure its DNS, add Resend SPF and DKIM records, then add a DMARC monitoring policy. Keep the Vercel URL working until the new domain is verified.

## Before the founder tops up eSIMAccess

1. Apply the Preview values above and redeploy.
2. Confirm the Preview is protected and shows the founder-only Thailand 100MB canary.
3. Run the release gate and a dry checkout with supplier writes still off.
4. Confirm the live supplier package slug and price are still Thailand, 100MB, seven days and US$0.30.
5. Turn on the supplier-write flag in Preview only and redeploy.
6. Top up only enough for the single approved canary.
7. Use the Opn test card and capture non-sensitive evidence of payment, supplier order, delivery email, QR display, installation guide and usage lookup.
8. Replay the same checkout request and confirm there is no second charge, supplier order or email.
9. Turn the supplier-write flag off again immediately after the test.

## Still blocks public live sales

- Opn and eSIMAccess webhook authentication and replay handling
- payment, supplier and email reconciliation
- 3-D Secure return and uncertain-payment recovery
- fraud holds, chargeback operations and human refund approval
- verified customer email sender and monitored reply inbox
- separate staff authentication and access logs
- final legal operator details and Australian legal review
- independent penetration testing before material scale
