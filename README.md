# Criterion Drift Local

Healthcare intake automation must catch when payer or clinic rules drift before documents route to the wrong workflow.

The implementation is a laptop-scale proof of the workflow behind that claim, with `intake packet` fixtures and falsifiable gates.

## Design intent

Offline healthcare document-intake criterion drift detector for referrals, prior auth, and fax workflows.

## Implementation notes

- Compiles 220 replayable `intake packet` fixtures that make the `criterion-drift` assumptions observable.
- Treats `criterion_recall`, `patient_split_accuracy`, `routing_precision`, and `evidence_span_coverage` as release gates, not dashboard decoration.
- Plants degraded cases for `multi_patient_packet`, `payer_rule_drift`, `missing_required_doc`, and `wrong_patient_merge` and checks whether the harness catches them.
- Exports the `Criterion Drift Local` run as structured reports, static HTML, benchmark numbers, and a shareable package.

## Reproduce the run

```bash
uv sync --extra dev
uv run criterion-drift init-demo --force
uv run criterion-drift run-suite
uv run criterion-drift verify
uv run criterion-drift dashboard
uv run criterion-drift benchmark --iterations 100
uv run criterion-drift export-demo-pack
```

## Artifacts

- `data/scenarios.json`
- `outputs/summary.json`
- `outputs/reports.json`
- `outputs/evidence_pack.md`
- `outputs/dashboard.html`
- `outputs/benchmark.json`
- `outputs/demo-pack.zip`

## Release checks

```bash
uv run ruff check .
uv run pytest -q
uv run criterion-drift run-suite
uv run criterion-drift verify
uv run criterion-drift benchmark --iterations 100
```

## Public data stance

The `criterion-drift-local` public surface is source, tests, lockfile, and docs. It does not need credentials, browser state, customer records, or hosted services.
