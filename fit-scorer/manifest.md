# manifest.md

Entry point for the fit scorer. If a client is not in this table, the scorer
stops and says so — no guessing at file locations.

House lane is not a client: it always scores against `house-dossier.md` with
the default weights in `scorecard-template.md`, threshold 75.

| client | dossier path | weights path | last check-in | next check-in | threshold | newsletter cadence |
|---|---|---|---|---|---|---|
| freeman | clients/freeman/dossier.md | clients/freeman/weights.md | TBD | TBD | 75 | monthly (pending Grant) |
| delta | clients/delta/dossier.md | clients/delta/weights.md | TBD | TBD | 75 | monthly (pending Grant) |

Notes:

- Thresholds are the spec defaults (75 recommend-eligible, 60–74 watchlist,
  below 60 pass) until Grant sets per-client values (spec §17.3).
- Newsletter cadence default is monthly, pending Grant's call (spec §15).
- `last check-in` must be a real date before a client's first run: pre-run
  validation compares it against the dossier's last-updated date. TBD fails
  validation, which is correct until the dossier is populated.
