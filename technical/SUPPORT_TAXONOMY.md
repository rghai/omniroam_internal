# Customer support taxonomy

This taxonomy turns common supplier issues into customer language. Customers should not need to know which wholesaler provides their eSIM.

| Customer topic | Starting tier | First response | Supplier escalation |
| --- | ---: | --- | --- |
| Device compatibility | 1 | Check eSIM support, carrier lock and destination coverage before purchase | Device Capability |
| Device configuration | 1 | Check mobile data, data roaming, selected SIM, APN and software version | Device Configuration |
| eSIM activation | 2 | Check installation state, exact error and whether the profile was deleted | eSIM Activation |
| Connectivity | 2, high priority | Check location, network selection, APN, roaming and remaining data | Connectivity |
| Order recovery | 1 | Verify the customer privately and reissue the Omniroam recovery link | Escalate under the underlying fault only |
| Top up | 1 | Explain availability for the purchased plan and offer a new plan when top up is unsupported | Top Up |
| Payment or billing | 2, high priority | Match the order, payment state and fulfilment state before promising an outcome | Portal/API Issue if supplier state is inconsistent |
| Refund request | 3, human | Gather facts and preserve Australian Consumer Law rights. A person approves every refund | Refund Request |
| Other | 1 | Classify after reading the case | Others |

## Internal-only category

`supplier_portal_api` maps to eSIMAccess `Portal/API Issue`. It is never shown to customers. Engineering or Tier 3 staff use it when Omniroam's record and the supplier's record disagree, a webhook is missing or an API call fails repeatedly.

## Automation boundary

Tier 1 can provide compatibility checks, setup instructions, recovery guidance and top-up availability. Tier 2 can run structured activation and connectivity diagnostics. Tier 3 handles refunds, payment uncertainty, repeated provisioning failures, legal concerns and supplier disputes.

Automation may suggest the next check, collect safe diagnostic details and draft a reply. It must not ask for card data, activation codes or full ICCIDs. It must not approve a refund or create another supplier order.

## Launch work still required

- Connect a monitored support inbox and reply delivery.
- Add case assignment, notes and safe attachments.
- Alert on missed first-response and resolution targets.
- Publish device-specific help articles with reviewed screenshots.
- Agree service hours, named Tier 3 owners and supplier escalation contacts.
- Rehearse one case from every category before live sales.
