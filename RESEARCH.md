# Research And Plan Review

Company: Tennr
Project: `criterion-drift`

## Refined Thesis

Healthcare intake automation must catch when payer or clinic rules drift before documents route to the wrong workflow.

The implementation is intentionally local and synthetic, but the test harness is shaped around the real operating question: can the proposed artifact create evidence a founder, CTO, or product leader would immediately recognize as useful?

## Fresh Sources Checked

- https://www.tennr.com/
- https://www.reddit.com/r/healthIT/comments/1kymsc2
- https://www.reddit.com/r/healthIT/comments/1kymt3m

## Plan Excerpt Used

## The Gap

Tennr's wedge is RaeLM's ability to read a faxed clinical document, extract structured fields, and *evaluate them against 8,000 payer-specific criteria sets* ([Lightspeed memo](https://lsvp.com/stories/investing-in-tennr-machine-learning-for-healthcare-documents/)). The criteria are dynamic — payer policies for prior auth and medical necessity change quarterly, sometimes monthly, often without machine-readable notice. There is no public Tennr posture on **how a criterion change is detected, regression-tested, and promoted** across the 150+ customer orgs. Their first-ML-Ops-Engineer JD is the giveaway: someone needs to build the infra for "iterating on foundational ML and AI systems" — and the highest-leverage thing that engineer should ship in their first 90 days is *not* a training pipeline (Tennr already has one). It's a **criterion-drift detector** that catches the moment Aetna silently changes a sleep-study prior-auth rule and Tennr's autopilot starts producing the wrong "deny" predictions. Today, the failure mode of a stale criterion is invisible: it just becomes a slow-rising denial rate that customers see weeks later. That's the gap.

## The Project — `criterion-drift`

> A drift-detector for payer-policy and clinical-criteria models — so Tennr's autopilot knows the moment a rule changed, before a denial spike teaches the customer.

**What it is.** A monitoring system that watches three signals per (payer × service-line × criterion) tuple: (1) the **public source** (payer policy PDFs / portals), (2) the **model's outcome distribution** on incoming documents, and (3) the **ground-truth outcome** (denial/approval letters Tennr receives back). It runs a triangulated detector — drift in (1) without matching drift in (2)–(3) means Tennr is missing a policy update; drift in (3) without (1) means a silent policy change; drift in (2) without (3) means a model regression. Each detected drift becomes a card in a "Criterion Drift Dashboard" with an evidence bundle: PDF diff, distribution plot, denial-rate change, suggested rule-patch.

**Why it solves the gap.** Tennr's published narrative is "service-to-criteria mapping + quality-controlled autopilot" ([source](https://www.tennr.com/product)). Both phrases assume the criteria are *fresh*. `criterion-drift` is the infra that keeps them fresh — and the natural first thing Tennr's first ML Ops Engineer should ship. It directly enables Trey's stated quote that RaeLM was built to handle the "nuance of checkboxes" — but nuance only matters if the rule the checkbox maps to is current.

**The "wow" moment.** A 90-second demo. I show a synthetic Aetna sleep-study policy change (we modify a PDF to remove a "BMI > 30" eligibility requirement). The dashboard flips a card from green to red within two ingestion cycles, surfaces the PDF diff (red-strikethrough on the removed line), shows the cohort of patients whose "deny" predictions would flip to "approve," and one-click promotes a rule patch to staging. Trey sees the failure mode that *would* have shown up as a denial-rate uptick a month later, caught in 90 seconds.

## Prototype Plan

- **CLI / surface.** `criterion-drift watch --payer aetna --service sleep-study` for ad-hoc; a small Next.js dashboard for the visual; webhook for Slack notifications.
- **Five representative criteria.**
  1. *Aetna — home sleep apnea testing* (eligibility thresholds change yearly).
  2. *UnitedHealthcare — physical therapy frequency caps*.
  3. *Anthem — wheelchair DME documentation*.
  4. *Medicare LCD — continuous glucose monitor* (regional carrier variation).
  5. *Cigna — prior auth for MRI without contrast*.
- **Expected output.** A dashboard with five tiles, each showing source-change badge, current denial rate vs. baseline (sparkline), PSI on model output, and a "view evidence" button that reveals PDF diff + sample misclassified documents.
- **Metrics.** Time-to-detect a synthetic policy injection (target < 12h from when ingestion sees the changed criterion); false-positive rate on a 30-day historical replay (target < 1/criterion/month); precision of the "silent change" classifier (target > 0.7 on a hand-labeled set of 50 known historical changes).


## Build Acceptance Criteria

- Deterministic local fixtures.
- Domain-specific metrics and failure modes.
- Passing unit tests.
- Passing CLI verifier.
- Static dashboard generated locally.
- Benchmark output under the project `outputs/` folder.
- Public-safe README: no founder emails, no private outreach text, no credentials.
