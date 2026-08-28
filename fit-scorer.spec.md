# fit-scorer.spec.md

GPC Client-Product Fit Scorer. A Claude skill that scores SaaS and AI products against GPC clients using their existing AI strategy assessments, and against GPC's own stack. Answers "is this a good fit, and if not, what instead." Scored findings feed per-client newsletters as the retainer delivery vehicle.

Status: spec. No code or skill file written yet. Newsletter integration reflects Grant's direction from 8/27 Slack; exact format and cadence pending his confirmation.
Owner: Olivia Keiter. Packaging, naming, pricing, and newsletter format owned by Grant Hushek.
Date: 2026-08-27, revised same day after newsjack context.

---

## 1. What it does

- Scores a product against a specific client's dossier using client-specific weights and pass/fail gates.
- Runs bake-offs: multiple products ranked against one client need, showing the full spread.
- Scores products against GPC's own needs to decide what earns hands-on trial time.
- Produces two output layers per run: a fully honest internal report and curated client-facing content.
- Feeds scored, threshold-clearing recommendations into each client's tailored newsletter.
- Maintains an append-only recommendation log per client.
- Is allowed to conclude "nothing fits, buy nothing," with named triggers that would change the outcome.

## 2. What it does not do

- It does not scout. Humans and the newsjack digest surface candidates; the skill scores them.
- It does not send anything. Client-facing content ships only after human review, and only if the score clears the client's threshold.
- It does not recommend based on partnership status. Partners and non-partners score on identical rubrics.
- It does not score from a stale or incomplete dossier. It refuses and flags instead.
- It does not duplicate the newsjack engine. That system drafts GPC marketing content at market scale. This one produces per-client advisory judgments. Same raw signals, opposite directions.

## 3. Relationship to the newsjack engine

GPC already runs a market-scale content engine (the Grantbot Newsjack Digest): scan signals, rank pitch-ready angles, draft posts for GPC's own marketing. It answers "what should GPC say publicly."

The fit scorer answers "what should this specific client buy or skip." The two connect at exactly one point: the digest is a candidate source. Products and launches surfaced in digest runs can be queued for scoring against client dossiers or the house dossier. Nothing else is shared. The scorer never drafts marketing content; the digest never scores fit.

## 4. Two lanes

**Client lane.** Score a product against a client dossier. Threshold-clearing output feeds that client's newsletter and check-in.

**House lane.** Score a product against GPC's own stack and service needs (the Workato pattern). Output decides whether GPC gets hands-on. Firsthand use earned in this lane becomes Verified confidence in future client-lane runs.

The house lane feeds the client lane. Research scoring is the filter; demo time is spent only on what clears the bar.

## 5. Repo layout

All files live in the GPC GitHub repo (private). All lowercase, hyphen separated.

```
fit-scorer/
  manifest.md                          # entry point: client -> paths, weights, cadence
  scorecard-template.md                # dimensions, rubric anchors, formula
  house-dossier.md                     # GPC's own stack and needs (house lane)
  products/
    <product-name>.md                  # research file per product, reused across runs
  clients/
    <client-name>/
      dossier.md                       # stack, pain points, maturity, budget, constraints, past decisions
      weights.md                       # client-specific weights and gates
      recommendations.md               # append-only log
      runs/
        YYYY-MM-DD-<product-name>.md           # pointed run scorecard
        YYYY-MM-DD-bakeoff-<need-slug>.md      # bake-off scorecard with comparison table
  newsletter/
    YYYY-MM-<client-name>.md           # per-client newsletter draft, post-review only
```

## 6. The manifest

`fit-scorer/manifest.md` is the skill's entry point. One table:

| client | dossier path | weights path | last check-in | next check-in | threshold | newsletter cadence |

The skill never guesses at file locations. If a client is not in the manifest, the skill stops and says so.

## 7. Pre-run validation

Before scoring anything, the skill checks:

1. Client exists in the manifest.
2. Dossier contains all required sections: stack, pain points, AI maturity, budget posture, compliance constraints, past decisions.
3. Weights file exists, weights sum to 100%, gates are defined.
4. Dossier last-updated date is newer than the client's last check-in date.
5. Product has not already been logged as passed-on for this client (check recommendations.md). If it has, the skill flags the prior entry and asks whether to re-score, rather than silently re-pitching.

Any failure: do not score. Name what failed and stop.

## 8. Scoring model

- Scale: 0 to 5 per dimension. Weights are client-specific and sum to 100%.
- Gates are pass/fail. Fail any gate and the score is 0, no recommendation.
- Formula: FitScore = (sum of Score x Weight) x GateMultiplier, normalized to 100.

### Default weights (baseline, adjusted per client in weights.md)

| Dimension | Default | Measures |
|---|---|---|
| Need severity | 20% | How acute the pain is and how directly this addresses it |
| Total cost vs displaced cost | 20% | 12 to 24 month TCO vs what it replaces or consolidates |
| Stack fit | 15% | Native integrations, API quality, compatibility with current stack |
| Data and security posture | 15% | Encryption, access controls, retention, incident history |
| Adoption lift | 10% | Change management, training burden, workflow disruption |
| Vendor durability | 10% | Funding, team stability, release cadence, acquisition risk |
| Time to value | 10% | How fast the client feels it |

Cost horizon is 12 to 24 months, matching contracts these clients actually sign. No multi-year projections on early-stage vendors.

### Gates (defined per client in weights.md, examples)

- Security and compliance: required certifications, data residency, breach history per client policy.
- Integration: must connect to at least N core systems via supported methods.
- Budget: estimated TCO over the likely contract term within the client's stated posture.
- Legal: vendor can sign the client's required DPA or addenda.

For regulated clients, security is a gate, never just a weight.

### Rubric anchors

5 ideal, 4 strong, 3 acceptable with trade-offs, 2 weak, 1 poor, 0 non-starter. Anchors are written in full in scorecard-template.md so scores are defensible, not vibes.

## 9. Confidence tiers

Every dimension score carries a confidence flag:

- **Verified**: GPC has used the product firsthand (house lane trial or client deployment).
- **High**: official sources, recent, consistent.
- **Medium**: mix of official and community sources, some assumptions.
- **Low**: estimated, older than 6 months, or conflicting signals.

**The firsthand rule:** a research-only score, however high, caps at Watchlist for client-facing purposes. Promotion to a client recommendation requires either Verified confidence from GPC hands-on use, or an explicit human decision to recommend on research alone, recorded in the log with that caveat. Research scores at or above threshold enter the demo queue: a "get hands-on" action item for GPC, not client content.

## 10. Research protocol

- Live web research only. Never score from training knowledge.
- Never bypass paywalls or logins.
- Prefer sources under 6 months old.
- Source trust order: official docs and changelogs, then verified reviews and reputable analysts, then anonymous forums and social threads.
- Vendor marketing is the lowest-trust source about the vendor. Weight third-party signal over the vendor's own claims.
- Pricing: pull from community sources (Reddit, reviews, forums) when not published. Record as a range with assumptions, flagged "estimated, pending demo." High scorers with estimated pricing auto-generate a book-the-demo task.
- Conflicting sources: record both claims with dates and source types, mark the dimension Low confidence.
- Product research is saved to `products/<product-name>.md` with a research date, so a bake-off does not repeat work, and stale research (over 90 days) gets refreshed before reuse.

## 11. Run modes

**Pointed run.** One product, one client (or the house). Triggered by product news, a newsjack digest item, or by request. Output: one scorecard file, one internal report.

**Bake-off run.** One client need, multiple products. Every product scored on the same client weights. Output includes a comparison table with the full spread across all products and dimensions, not just the winner. The spread is the point: a close second with better pricing is a negotiation lever with the first-place vendor.

## 12. Thresholds and outcomes

Tuned per client in the manifest. Defaults:

- **75 and above**: recommend-eligible. Enters demo queue (research-only) or goes to human review for newsletter inclusion (Verified).
- **60 to 74**: Watchlist. Discussed internally, auto re-scored next check-in cycle.
- **Below 60**: pass. Logged with reasons.
- **Buy nothing**: first-class outcome when nothing clears gates or thresholds. Must name which gates failed or which dimensions were weak, and list "what would change the outcome" triggers (certification achieved, pricing drop, feature shipped). Those triggers become standing watch items.

Every non-recommend includes the what-would-change section. No dead-end nos.

## 13. Output layers

**Internal report** (every run): full honesty. Vendor concerns, partner status, uncertainties, the spread, negotiation angles. Never shown to clients. Stored in the client's runs folder.

**Client-facing content** (threshold-clearing runs only, after human review): the primary vehicle is the per-client newsletter. A scored recommendation becomes a newsletter section: what it is, what it solves for them specifically, why GPC recommends it, what it costs (flagged if estimated). Newsletters can also carry relevant news and context for the client's stack, which is where the newsjack signal feed adds value beyond scoring. A standalone brief remains available for urgent or check-in-driven recommendations that should not wait for a newsletter cycle.

Every client-facing recommendation includes:

- Partner disclosure, mandatory and non-omittable, when the product is a GPC vendor partner.
- Standing disclaimer: this is GPC's review of the product against this client's specific needs, not an endorsement of the company. Cost figures are estimates, not financial advice.

A newsletter with no threshold-clearing products still ships if it carries relevant news; it just carries no recommendation that cycle. Recommendations appear only when something clears the bar. Scarcity keeps them meaningful.

## 14. Recommendation log

`clients/<client-name>/recommendations.md`, append-only by convention. Past entries are never edited; corrections are new entries referencing the old one. Git history is the audit trail.

Each entry: date, product, run mode, lane, score, confidence profile, outcome (recommend, watchlist, pass, buy nothing, demo queue), key reasons, what-would-change triggers, partner status, and where it shipped (newsletter issue or standalone brief).

Once 20+ entries exist across clients, roll up a quarterly summary: evaluated, recommended, adopted, notable buy-nothings, and whether partners are winning disproportionately. If partners only win because they are partners, the rubric is broken and gets audited. The fix is never staging non-partner wins for optics.

## 15. Cadence

- Client-lane runs happen the week of that client's retainer check-in. Findings feed the newsletter and the check-in; no findings means the newsletter runs on news alone.
- Newsletter cadence per client is set in the manifest. Default assumption is monthly, pending Grant's call.
- Watchlist items re-score automatically at the next cycle.
- Dossier and weights review is a required check-in step. Changed needs re-run affected watchlist scores.
- House-lane runs happen when a product looks relevant to GPC's own delivery stack, or when a digest item suggests one.
- Newsjack digest items relevant to any client's stack get queued as scoring candidates as part of digest review.

## 16. Skill triggers

The skill fires on: "fit score," "score [product] against [client]," "bake-off," "run the scorer," "is [product] a fit for [client]," "what should [client] use for [need]," "demo queue," "anything new for [client]," "[client] newsletter," and check-in prep requests that mention product recommendations.

## 17. Open decisions (Grant)

1. Newsletter format, cadence, and who drafts the non-scoring sections (news and context). The scorer produces the recommendation sections; ownership of the rest is open.
2. Client-facing service name and whether this is packaged as a retainer add-on or folded into existing retainers.
3. Recommendation threshold defaults and whether clients ever see the rubric itself.
4. How digest-to-scorer handoff works operationally with Dustin's pipeline.
5. What client data is allowed to live in the GitHub repo (dossiers hold strategy context, never credentials or client-confidential documents; confirm the line).
6. Partner program mechanics, and therefore what the disclosure line says exactly.

## 18. Pilot plan

1. Build the repo structure, manifest, scorecard template, and house dossier.
2. Write dossiers and weights for 2 to 3 real clients from existing assessments. Candidates: Freeman, Delta.
3. Pull scoring candidates from a recent newsjack digest run plus any products already under discussion.
4. Run 1 to 2 bake-offs per pilot client manually. Produce internal reports; draft one newsletter recommendation section only if something clears and Grant signs off.
5. Run one house-lane score on a product GPC is already considering, to test the Verified promotion path against a known answer.
6. Refine rubric anchors and thresholds from friction, then write the actual SKILL.md.

The pilot is the calibration. If the rubric fights the operator on real clients, the anchors change before anything automates.
