# CLAUDE.md

This repo is the GPC Client-Product Fit Scorer: a system for scoring SaaS and AI
products against specific GPC clients (using their AI strategy assessments) and
against GPC's own stack. It answers "is this a good fit for this client, and if
not, what instead." It is a documents-and-judgment repo — markdown files, no
application code, no build, no tests.

The source of truth is `fit-scorer.spec.md` at the repo root. When anything in
this file or in a template conflicts with the spec, the spec wins. Read it
before doing scoring work.

## Repo layout

Everything operational lives under `fit-scorer/`. The directory keeps that name
(rather than flattening into the repo root) so the spec's paths stay literal and
the folder can be moved into the main GPC repo later without rewrites.

```
fit-scorer.spec.md                     # source of truth
fit-scorer/
  manifest.md                          # entry point: client -> paths, weights, cadence
  scorecard-template.md                # dimensions, rubric anchors, formula, output templates
  house-dossier.md                     # GPC's own stack and needs (house lane)
  products/<product-name>.md           # research file per product, reused across runs
  clients/<client-name>/
    dossier.md                         # stack, pain, maturity, budget, constraints, past decisions
    weights.md                         # client-specific weights and gates
    recommendations.md                 # append-only log
    runs/YYYY-MM-DD-<product-name>.md          # pointed run
    runs/YYYY-MM-DD-bakeoff-<need-slug>.md     # bake-off
  newsletter/YYYY-MM-<client-name>.md  # per-client newsletter draft, post-review only
```

`clients/client-template/` is the skeleton for onboarding a new client. Copy it,
fill it from the client's real assessment, then add the client to the manifest.
It is not a real client and never gets scored.

## Conventions

- All filenames lowercase, hyphen-separated. Dates are ISO (`YYYY-MM-DD`).
- Markdown everywhere. No docx, no spreadsheets, unless explicitly requested.
- `manifest.md` is the only entry point. Never guess at file locations; if a
  client is not in the manifest, stop and say so.
- `recommendations.md` files are append-only. Never edit or delete a past
  entry — corrections are new entries referencing the old one. Git history is
  the audit trail.
- Product research lives in `products/`, one file per product, with a research
  date. Reuse it across runs; refresh it if older than 90 days.

## Hard rules for scoring runs

These are the non-negotiables from the spec. Do not soften them.

1. **Validate before scoring, refuse on failure.** Client in manifest; dossier
   complete (stack, pain points, AI maturity, budget posture, compliance
   constraints, past decisions); weights sum to 100% with gates defined; dossier
   updated more recently than the last check-in; product not already logged as
   passed-on for this client (if it is, flag the prior entry and ask before
   re-scoring). Any failure: do not score, name what failed, stop.
2. **Gates are pass/fail.** Fail any gate → score is 0, no recommendation. For
   regulated clients, security is a gate, never just a weight.
3. **The firsthand rule.** A research-only score caps at Watchlist for
   client-facing purposes, however high it is. Promotion to a client
   recommendation requires Verified confidence (GPC hands-on use) or an explicit
   human decision recorded in the log with that caveat. Research scores at or
   above threshold go to the demo queue, not to the client.
4. **Live research only.** Never score from training knowledge. Never bypass
   paywalls or logins. Prefer sources under 6 months old. Vendor marketing is
   the lowest-trust source about the vendor. Unpublished pricing is recorded as
   an estimated range with assumptions, flagged "estimated, pending demo."
5. **Partner-blind scoring.** Partners and non-partners score on identical
   rubrics. Partner status appears in disclosures, never in scores.
6. **"Buy nothing" is a first-class outcome.** Every non-recommend names what
   failed and lists what-would-change triggers. No dead-end nos.
7. **Nothing ships to a client from here.** Internal reports are fully honest
   and never shown to clients. Client-facing content is drafted only for
   threshold-clearing runs and goes out only after human review. Every
   client-facing recommendation carries the partner disclosure (when
   applicable) and the standing disclaimer, verbatim from the scorecard
   template.
8. **This skill scores; it does not scout and it does not market.** Candidates
   come from humans and the newsjack digest. Marketing content belongs to the
   newsjack engine, not here.

## Two lanes

- **Client lane**: product vs. a client dossier. Threshold-clearing output
  feeds that client's newsletter and check-in.
- **House lane**: product vs. `house-dossier.md`. Output decides whether GPC
  gets hands-on. Firsthand use earned here becomes Verified confidence in
  future client-lane runs. The house lane feeds the client lane.

## Current status

Pilot phase (spec §18). Structure, manifest, templates, and client skeletons
exist; the Freeman and Delta dossiers and the house dossier are unpopulated
skeletons and will fail pre-run validation until filled from real assessments —
that is intentional. No SKILL.md yet: the spec says it gets written only after
the pilot calibrates the rubric. Open decisions (newsletter format/cadence,
naming, thresholds, partner disclosure wording, what client data may live in
this repo) belong to Grant — flag them, don't decide them.

Owner: Olivia Keiter. Packaging, naming, pricing, newsletter format: Grant
Hushek. Client data line (spec §17.5): dossiers hold strategy context only —
never credentials, never client-confidential documents.
