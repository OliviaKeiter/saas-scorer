# saas-scorer

GPC Client-Product Fit Scorer: scores SaaS and AI products against specific
GPC clients (from their AI strategy assessments) and against GPC's own stack.
Answers "is this a good fit, and if not, what instead." Scored findings feed
per-client newsletters as the retainer delivery vehicle.

- **Spec (source of truth):** [`fit-scorer.spec.md`](fit-scorer.spec.md)
- **Operating rules for Claude:** [`CLAUDE.md`](CLAUDE.md)
- **Entry point:** [`fit-scorer/manifest.md`](fit-scorer/manifest.md)

Status: pilot (spec §18). Structure and templates are in place; client
dossiers and the house dossier are unpopulated skeletons that intentionally
fail pre-run validation until filled from real assessments. The SKILL.md gets
written after the pilot calibrates the rubric.

Owner: Olivia Keiter. Packaging, naming, pricing, and newsletter format:
Grant Hushek.

Private repo. Dossiers hold strategy context only — never credentials, never
client-confidential documents.
