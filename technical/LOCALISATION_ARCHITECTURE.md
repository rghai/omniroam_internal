# Market and language architecture

Updated 11 September 2026.

## Decision

Use path-based experiences on one origin:

- `/global/en` for global English
- `/au/en` for Australian English
- future routes such as `/global/es` and `/au/zh` after editorial approval

Paths keep one deployment, one cookie boundary and one analytics origin. They are easier to operate than country subdomains and still allow distinct canonical URLs, `hreflang` metadata and market-specific content.

## Selection rules

1. An explicit visitor choice wins.
2. The site remembers that choice for six months and explains the preference cookie beside the selector.
3. If there is no remembered choice, the Vercel country header may suggest Australia for an Australian IP. All other traffic starts on Global.
4. IP is a hint only. It never locks content, price or language.
5. The selector remains available so a traveller can change market at any time.

## Language rollout

The planned order is English, Spanish, French, Mandarin Chinese, Japanese, Korean and Thai. Only English is published now.

Each new locale needs:

- a fluent human editorial review rather than a direct machine translation;
- market-appropriate terminology, units, dates, examples and support expectations;
- translated metadata, canonical and `hreflang` checks;
- legal review for the markets where that locale is promoted;
- checkout, email, installation, support, privacy and error-state coverage;
- screenshots at desktop and phone sizes;
- a fallback to reviewed English when a string is missing.

Orders record market and locale so transactional email and later customer support can use the same approved experience. Supplier identity and supplier routing remain server-side and are never selected by the customer.

