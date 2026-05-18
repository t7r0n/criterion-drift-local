# Criterion Drift Local

Offline healthcare document-intake criterion drift detector for referrals, prior auth, and fax workflows.

This is a local-first, synthetic-data prototype inspired by a company-specific project plan for **Tennr**. It is built to demonstrate the engineering shape of `criterion-drift` without private data, credentials, external APIs, or hosted services.

## Why it matters

Healthcare intake automation must catch when payer or clinic rules drift before documents route to the wrong workflow.

## What it does

- Generates deterministic synthetic `intake packet` scenarios.
- Scores each scenario against domain-specific quality gates.
- Produces evidence-backed findings for realistic failure modes.
- Writes a static dashboard, JSON reports, benchmark output, and a portable demo pack.
- Exposes a JSONL tool loop for local agent integration.

## Metrics

- `criterion_recall`
- `patient_split_accuracy`
- `routing_precision`
- `evidence_span_coverage`

## Failure modes

- `multi_patient_packet`
- `payer_rule_drift`
- `missing_required_doc`
- `wrong_patient_merge`

## Quickstart

```bash
uv sync --extra dev
uv run criterion-drift init-demo --force
uv run criterion-drift run-suite
uv run criterion-drift verify
uv run criterion-drift dashboard
uv run criterion-drift benchmark --iterations 100
uv run criterion-drift export-demo-pack
```

## Expected outputs

- `data/scenarios.json`
- `outputs/summary.json`
- `outputs/reports.json`
- `outputs/evidence_pack.md`
- `outputs/dashboard.html`
- `outputs/benchmark.json`
- `outputs/demo-pack.zip`

## Validation

```bash
uv run ruff check .
uv run pytest -q
uv run criterion-drift run-suite
uv run criterion-drift verify
uv run criterion-drift benchmark --iterations 100
```

## Demo hook

A packet split/reasoning report shows exactly which page triggered each intake criterion and route.
