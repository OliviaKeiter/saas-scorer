# scorecard-template.md

Dimensions, rubric anchors, formula, and output templates for every fit-scorer
run. Anchors are written in full so scores are defensible, not vibes. Weights
below are the baseline; each client's `weights.md` overrides them.

## Formula

```
FitScore = (Σ DimensionScore × Weight) × GateMultiplier, normalized to 0–100
```

- Dimension scores: 0–5. Weights sum to 100%.
- GateMultiplier: 1 if every gate passes, 0 if any gate fails. A gated-out
  product scores 0 and cannot be recommended, whatever its dimension scores.
- Normalization: (weighted sum ÷ 5) × 100.
- Cost horizon for all cost math: 12–24 months, matching contracts these
  clients actually sign. No multi-year projections on early-stage vendors.

## Thresholds (defaults; per-client overrides in the manifest)

- **75+** — recommend-eligible. Research-only → demo queue. Verified → human
  review for newsletter inclusion.
- **60–74** — watchlist. Discussed internally, auto re-scored next check-in.
- **<60** — pass. Logged with reasons.
- **Buy nothing** — first-class outcome when nothing clears. Name the failed
  gates or weak dimensions and list what-would-change triggers; triggers become
  standing watch items.

## Default weights

| Dimension | Default | Measures |
|---|---|---|
| Need severity | 20% | How acute the pain is and how directly this addresses it |
| Total cost vs displaced cost | 20% | 12–24 month TCO vs what it replaces or consolidates |
| Stack fit | 15% | Native integrations, API quality, compatibility with current stack |
| Data and security posture | 15% | Encryption, access controls, retention, incident history |
| Adoption lift | 10% | Change management, training burden, workflow disruption |
| Vendor durability | 10% | Funding, team stability, release cadence, acquisition risk |
| Time to value | 10% | How fast the client feels it |

## Rubric anchors

General scale: 5 ideal, 4 strong, 3 acceptable with trade-offs, 2 weak,
1 poor, 0 non-starter.

### Need severity

- **5** — Directly eliminates a pain the dossier names as acute; the client is
  actively losing time, money, or deals to it today.
- **4** — Squarely addresses a named pain point; impact is clear but the pain
  is costly rather than bleeding.
- **3** — Addresses a real but secondary pain, or covers most of a named pain
  with workarounds for the rest.
- **2** — Touches a pain the client has, but indirectly; the client would have
  to change how they work to feel the benefit.
- **1** — Solves a problem the dossier doesn't show this client having.
- **0** — No connection to any documented need; a solution looking for a
  problem here.

### Total cost vs displaced cost

- **5** — 12–24 month TCO is clearly below what it replaces or consolidates;
  net savings on top of the capability gain.
- **4** — TCO roughly washes against displaced cost; capability gain comes at
  near-zero net cost.
- **3** — Net cost increase, but proportionate to the value and inside the
  client's budget posture.
- **2** — Meaningful net cost increase with soft or speculative offsets.
- **1** — Expensive relative to value; displaces little or nothing.
- **0** — TCO can't be estimated even as a range, or is flatly outside the
  client's stated budget posture.

### Stack fit

- **5** — Native, supported integrations with the client's core systems; clean,
  documented API; nothing displaced that the client wants to keep.
- **4** — Native integrations for most core systems; minor gaps covered by a
  solid API or supported middleware.
- **3** — Workable via middleware (e.g. iPaaS) or light custom glue; API is
  adequate but a real project.
- **2** — Integration is possible but brittle: unsupported connectors,
  CSV/export workflows, or an immature API.
- **1** — Substantially overlaps or conflicts with systems the client is
  keeping; integration would be fought, not built.
- **0** — Cannot connect to the client's core systems by any supported method.

### Data and security posture

- **5** — Certifications and controls exceed the client's requirements;
  encryption, access controls, retention, and residency all documented; clean
  incident history.
- **4** — Meets the client's requirements with current certifications; minor
  gaps with credible, dated remediation plans.
- **3** — Meets the essentials; some controls unverified or documentation
  thin; no known incidents.
- **2** — Notable gaps (missing cert the client expects, vague retention,
  shared-tenancy concerns) or an incident with unclear remediation.
- **1** — Poor posture: material unresolved gaps or a serious recent incident.
- **0** — Fails a stated client security or compliance requirement outright.
  (For regulated clients this is gated, not just scored.)

### Adoption lift

- **5** — Fits existing workflows as-is; users productive in days with no
  formal training.
- **4** — Light lift: short training, minor workflow changes, low resistance
  risk.
- **3** — Moderate lift: structured rollout and training needed, but within
  the client's demonstrated change capacity.
- **2** — Heavy lift: significant workflow redesign or role changes; success
  depends on sustained management push.
- **1** — Disruptive: cuts against how the org works; high abandonment risk.
- **0** — Requires capacity (technical staff, admin time, change tolerance)
  the dossier shows the client does not have.

### Vendor durability

- **5** — Established or well-capitalized vendor; stable team; steady release
  cadence; low acquisition/shutdown risk over the contract horizon.
- **4** — Healthy trajectory: solid funding or revenue, active development,
  no red flags.
- **3** — Viable but unproven: early-stage, adequate runway, real customers;
  acceptable with a 12–24 month contract, not beyond.
- **2** — Warning signs: layoffs, stalled releases, leadership churn, or
  funding pressure.
- **1** — Visible distress or an acquisition likely to strand the product.
- **0** — Vendor unlikely to exist, or product unlikely to be supported,
  through the contract term.

### Time to value

- **5** — Client feels the benefit in days; setup is hours, not weeks.
- **4** — First real value inside 2–4 weeks with a straightforward setup.
- **3** — Value in 1–2 months after moderate implementation.
- **2** — A quarter or more before benefit lands; long implementation drag.
- **1** — Value depends on a long project with meaningful failure risk before
  any payoff.
- **0** — No credible path to felt value inside the 12–24 month horizon.

## Gates

Pass/fail, defined per client in `weights.md`. Fail any gate → FitScore 0, no
recommendation. Example gates (spec §8):

- **Security and compliance** — required certifications, data residency,
  breach history per client policy. For regulated clients security is a gate,
  never just a weight.
- **Integration** — must connect to at least N core systems via supported
  methods.
- **Budget** — estimated TCO over the likely contract term within the client's
  stated posture.
- **Legal** — vendor can sign the client's required DPA or addenda.

## Confidence tiers

Every dimension score carries one flag:

- **Verified** — GPC has used the product firsthand (house-lane trial or
  client deployment).
- **High** — official sources, recent, consistent.
- **Medium** — mix of official and community sources, some assumptions.
- **Low** — estimated, older than 6 months, or conflicting signals.

**The firsthand rule:** a research-only score, however high, caps at Watchlist
for client-facing purposes. Promotion to a client recommendation requires
Verified confidence or an explicit human decision recorded in the
recommendation log with that caveat. Research scores at/above threshold enter
the demo queue as a "get hands-on" action item for GPC, not client content.

---

## Pointed run scorecard template

Save as `clients/<client>/runs/YYYY-MM-DD-<product-name>.md`
(house lane: date-stamped file alongside `house-dossier.md` runs, same format).

```markdown
# <Product> vs <Client> — YYYY-MM-DD

Lane: client | house
Run mode: pointed
Trigger: <product news / newsjack digest item / request>
Product research file: products/<product-name>.md (research date: YYYY-MM-DD)
Dossier last updated: YYYY-MM-DD · Last check-in: YYYY-MM-DD

## Pre-run validation

- [ ] Client in manifest
- [ ] Dossier complete (stack, pain points, AI maturity, budget posture,
      compliance constraints, past decisions)
- [ ] Weights sum to 100%, gates defined
- [ ] Dossier newer than last check-in
- [ ] Not previously passed-on (recommendations.md checked)

## Gates

| Gate | Result | Evidence |
|---|---|---|

## Scores

| Dimension | Weight | Score (0–5) | Confidence | Evidence |
|---|---|---|---|---|

**FitScore: NN / 100** · GateMultiplier: 1|0
**Outcome:** recommend-eligible | demo queue | watchlist | pass | buy nothing

## Internal report (never client-facing)

<Full honesty: vendor concerns, partner status, uncertainties, negotiation
angles.>

## What would change the outcome

- <trigger — becomes a standing watch item>

## Log entry

<Copy the entry appended to recommendations.md.>
```

## Bake-off scorecard template

Save as `clients/<client>/runs/YYYY-MM-DD-bakeoff-<need-slug>.md`. Same
validation and gates per product, then:

```markdown
## Comparison table

| Dimension (weight) | Product A | Product B | Product C |
|---|---|---|---|
| ... per-dimension scores with confidence flags ... |
| **FitScore** | | | |
| **Outcome** | | | |

The spread is the point: show all products across all dimensions, not just the
winner. A close second with better pricing is a negotiation lever with the
first-place vendor — record that angle in the internal report.
```

---

## Client-facing recommendation block (post-review only)

Used in `newsletter/YYYY-MM-<client-name>.md` or a standalone brief. Only for
threshold-clearing, Verified (or explicitly human-promoted) runs, only after
human review.

Required, non-omittable elements:

- **Partner disclosure** (when the product is a GPC vendor partner; exact
  wording pending Grant, spec §17.6): "<Product> is a GPC vendor partner. This
  recommendation was scored on the same rubric applied to non-partners."
- **Standing disclaimer:** "This is GPC's review of the product against your
  specific needs, not an endorsement of the company. Cost figures are
  estimates, not financial advice."

Section shape: what it is · what it solves for them specifically · why GPC
recommends it · what it costs (flagged if estimated).
