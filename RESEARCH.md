# Research And Plan Review

Project: `intake-page-evidence`

## Refined Thesis

Healthcare intake automation needs page-level evidence that routing decisions still match current payer and clinical criteria.

The implementation is intentionally local and synthetic, but the test harness is shaped around the real operating question: can the proposed artifact create evidence a founder, CTO, or healthcare-automation leader would immediately recognize as useful?

## Fresh Sources Checked

- Public healthcare prior-authorization and medical-necessity workflow patterns.
- Local synthetic payer-criteria drift fixture model.

## Plan Excerpt Used

## The Gap

Clinical intake systems can extract fields from faxes and PDFs, but payer criteria change frequently and often without machine-readable notice. A stale criterion can silently turn into wrong routing, wrong denial prediction, or missing evidence until a customer notices a denial-rate change.

The missing artifact is a local drift detector that ties a routing decision back to specific page evidence, criterion version, outcome distribution, and suggested rule patch. The key is not just detecting drift; it is preserving the page-level reason that a reviewer can inspect.

## The Project - `intake-page-evidence`

> A local criterion-drift and page-evidence harness for healthcare intake routing, built around replayable synthetic documents and payer-policy changes.

**What it is.** A deterministic CLI, fixture generator, evaluator, dashboard, benchmark, and evidence-pack exporter. It models payer-policy changes, document evidence, outcome-distribution drift, stale criteria, and rule-patch readiness.

**Why it solves the gap.** Three vectors:

1. **Page-level traceability.** Each routing or denial-risk finding maps back to the specific synthetic page evidence.
2. **Drift triangulation.** The harness separates source-policy drift, model-output drift, and ground-truth outcome drift.
3. **Patch readiness.** Findings include enough context to decide whether a criterion update should move to staging.

**The demonstration moment.** A synthetic sleep-study criterion changes. The dashboard flips the relevant card red, shows the page evidence and criterion delta, identifies the affected cohort, and records the suggested rule patch in the evidence pack.

## Prototype Plan

- **Fixture generator:** deterministic clean and drifted payer-criteria cases.
- **Evaluator:** scores criterion freshness, page evidence coverage, outcome-drift detection, and patch readiness.
- **Dashboard:** visualizes drift cards, metric means, failure modes, and top affected scenarios.
- **Evidence pack:** Markdown plus zipped artifacts for portable review.
- **What I measure:**
  - Criterion freshness.
  - Page evidence coverage.
  - Outcome-drift detection.
  - Patch readiness.

## Build Acceptance Criteria

- Deterministic local fixtures.
- Domain-specific metrics and failure modes.
- Passing unit tests.
- Passing CLI verifier.
- Static dashboard generated locally.
- Benchmark output under the project `outputs/` folder.
- Public-safe README: no founder emails, no private outreach text, no credentials.
