# NetraDrift

Private MPLADS review pilot using the five supplied Maharashtra CSV reports. Snapshot: 9 September 2026. Contains 4,798 linked works and recommendations, 2,437 sanctions, 937 completed works and 1,730 payment entries.

## Implemented

CSV cleaning and joins; transparent rule priority; reproducible Isolation Forest training and identical hosted inference; animated projected 3D network; search and filters; evidence comparisons and timelines; per-user persistent review history; retained dataset imports; demo presentation.

## Runtime

React/TypeScript on Vinext/Cloudflare Workers, with D1 (SQLite) persistence. This replaces separate FastAPI/PostgreSQL hosting for the first pilot, allowing the connected app to run without additional external accounts. Python trains offline and exports the model as JSON for hosted inference.

- `app/api`: authenticated data, reviews and imports.
- `lib/pipeline.mjs`: CSV parsing and rules.
- `analysis/train.py`: reproducible model training/export.
- `lib/inference.mjs`: hosted Isolation Forest inference.
- `data/source`: original CSVs, unmodified.
- `data/analyzed.json`: analysis snapshot.
- `drizzle/0000_netradrift.sql`: schema and initial records.
- `components`: shared controls and application views.

The entire Site must remain private. Demo presentation masks identifying text in the UI and exports but is not access control. Hosted identity headers are trusted only because the Sites platform authenticates and injects them; do not expose a standalone server that trusts caller-forged headers.

## Reproduce analysis

Node 22+ and Python with `analysis/requirements.txt`:

```sh
node scripts/prepare-data.mjs
python analysis/train.py
node scripts/seed-db.mjs
node scripts/verify-data.mjs
python scripts/verify-database.py
```

The initial migration is immutable once deployed. Import later snapshots or create new migrations rather than editing an applied migration.

## Verification

Typecheck: `node node_modules/typescript/bin/tsc --noEmit`

Build: `npm run build`

Tests cover counts, joins, reconciliation, scoring, malformed inputs, Python/JavaScript model parity for all 2,437 training records, seed SQL, review persistence, retry idempotency and owner-scoped queries. These are not browser tests or an accuracy evaluation.

## Interpretation limits

- Priority is a rule-based ranking, not fraud probability. ML percentile remains separate.
- Peer comparisons use total amounts, not quantities or construction specifications.
- Token overlap supplies duplicate candidates; it is not semantic embedding similarity.
- Missing completion and long intervals do not establish physical non-completion or a legal violation.
- Payments retain separate successful/in-progress totals. Repeated work IDs may represent legitimate instalments; no unique transaction IDs exist in the CSV.
- Conflicting report statuses are preserved. Missing amounts remain null.
- Thresholds are prototype assumptions displayed in Methods. There are no reviewed fraud labels or a predictive-delay model.

## Next milestones

Review alert samples; add official-document retrieval and a configured language model for RAG; improve semantic duplicate matching; obtain quantities, approved deadlines and field evidence; define delegated organisation roles; run browser visual/accessibility and full hosted workflow checks before broader rollout. RAG is not implemented in this pilot.

## Complete public demonstration
The root route opens immediately with 36 synthetic works. It includes the reference-led network and tile map, auto-building peer comparison, unit-cost and delay signals, paired duplicate review, timeline, field camera/location reports, a paginated register, MP/field/citizen demo personas, saved review outcomes and action tasks, and text case exports. `/research` retains the authenticated five-CSV pipeline and Isolation Forest analysis.

Demo visitors receive isolated random HttpOnly sessions; no role picker grants real authority. New D1 migration `0001_wet_thing.sql` adds demo sessions and records without changing the applied original migration. Photo payloads are bounded JPEGs; no R2 binding is required. Sessions expire after 30 days; offline drafts use explicitly queued browser-session storage. Method note search is deterministic retrieval, not a configured external LLM/RAG service. Infrastructure art and portraits are authored illustrations, not physical verification evidence. Tiles come from OpenStreetMap with attribution and a network failure state.

Validation: `node scripts/verify-demo.cjs` checks calculations, session isolation, idempotency, review history, input validation and cross-origin/owner rejection using the actual route handlers against in-memory SQLite. `node node_modules/typescript/bin/tsc --noEmit` and production build also pass. Browser appearance and hardware camera/GPS were not exercised in this environment.
