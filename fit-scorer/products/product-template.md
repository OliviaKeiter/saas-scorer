# product-template.md

Copy to `products/<product-name>.md` (lowercase, hyphens). One file per
product, reused across runs and clients. Refresh before reuse if the research
date is more than 90 days old.

Research protocol (spec §10): live web research only — never training
knowledge; never bypass paywalls or logins; prefer sources under 6 months old;
trust order is official docs/changelogs → verified reviews and reputable
analysts → anonymous forums and social threads; vendor marketing is the
lowest-trust source about the vendor; conflicting sources get both claims
recorded with dates and the dimension marked Low confidence.

```markdown
# <Product name>

Research date: YYYY-MM-DD
Vendor: <company> · Category: <what it is>
GPC partner status: partner | not a partner | pending
GPC hands-on: none | house trial YYYY-MM-DD | client deployment (<client>)

## What it does

## Integrations and API

## Security and compliance
<!-- certifications, encryption, access controls, retention, residency,
incident history — with sources and dates -->

## Pricing
<!-- published, or estimated range from community sources with assumptions,
flagged "estimated, pending demo." Estimated pricing on a high scorer
auto-generates a book-the-demo task. -->

## Vendor signals
<!-- funding, team stability, release cadence, acquisition risk -->

## Sources
<!-- URL · date · type (official / analyst-review / community) -->

## Conflicts and open questions
<!-- both claims, with dates and source types; mark affected dimensions Low -->
```
